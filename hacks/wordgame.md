---
layout: base
title: Word Game
permalink: /wordgame
---

<center>
<a href="https://compsciteam.github.io/student/wordgame/writeup">
<strong>
Writeup
</strong>
</a>
</center>

<style>
    #wordCanvas { 
        border: 10px solid #000;
        display: block;
        margin-left: auto;
        margin-right: auto;
    }
    
    h2 {
        text-align: center;
        margin-top: 20px;
    }
    #options {
        margin-top: 20px;
        margin-bottom: 10px;
        padding: 10px 20px;
        font-size: 16px;
        border: none;
        background-color: #007BFF;
        color: white;
        border-radius: 5px;
    }

    /* progress bar */
    .progress-bar { width: 80%; height: 18px; background: #222; border-radius: 9px; margin: 12px auto; position: relative; }
    .progress-fill { height: 100%; width: 0%; background: linear-gradient(90deg,#6be3a8,#6bb6ff); border-radius: 9px; transition: width 120ms linear; }
    .progress-text { position: absolute; right: 8px; top: 0; bottom: 0; display:flex; align-items:center; color:#071127; font-weight:700; font-size:12px; }
</style>

<h2 style="display: inline-block; margin-right: auto;">Word Game</h2>
<p>Select a game mode (string length) from the options menu and try to type the prompt as quickly and accurately as possible!</p>
<button style="float: right;" id="options">Options</button>

<p>WPM: <span class="wpm"></span></p>
<p>Accuracy: <span class="accuracy"></span></p>

<div class="progress-bar" aria-hidden="true"><div class="progress-fill"></div><div class="progress-text">0%</div></div>
<canvas id="wordCanvas" width="800" height="200"></canvas>

<script>
    const wordCanvas = document.getElementById('wordCanvas');
    const wordCtx = wordCanvas.getContext('2d');
    const optionsButton = document.getElementById('options');
    const progressFill = document.querySelector('.progress-fill');
    const progressText = document.querySelector('.progress-text');

    // Hide stats initially and clear their text so they won't show while typing
    const wpmEl = document.querySelector('.wpm');
    const accEl = document.querySelector('.accuracy');
    if (wpmEl) { wpmEl.style.visibility = 'hidden'; wpmEl.textContent = ''; }
    if (accEl) { accEl.style.visibility = 'hidden'; accEl.textContent = ''; }

    let currentString = "";
    let userInput = "";
    let startTime = null;
    let finished = false;
    let mistakes = 0;
    let currentPrompt = ""; // the active prompt text for redraws
    let caretInterval = null; // interval id for caret redraws

    const short_strings = ["The quick brown fox jumps over the lazy dog", "Pack my box with five dozen liquor jugs", "How quickly daft jumping zebras vex", "Jinxed wizards pluck ivy from the quilt", "Bright vixens jump dozy fowl quack", "Sphinx of black quartz judge my vow", "Two driven jocks help fax my big quiz", "Five quacking zephyrs jolt my wax bed", "The five boxing wizards jump quickly", "Jackdaws love my big sphinx of quartz", "Quick zephyrs blow vexing daft Jim", "Zany gnomes fix blighted quartz vases", "Bold foxes jump quickly past the lazy hound", "Mix two dozen plums with five ripe figs", "Anish is the GOAT"];
    const medium_strings = ["Amazingly few discotheques provide jukeboxes", "Back in June we delivered oxygen equipment of the same size", "The public was amazed to view the quickness and dexterity of the juggler", "Jovial zanies quickly gave up their quest for the exotic fish", "The wizard quickly jinxed the gnomes before they vaporized", "All questions asked by five watched experts amaze the judge", "The job requires extra pluck and zeal from every young wage earner", "Crazy Frederick bought many very exquisite opal jewels", "We promptly judged antique ivory buckles for the next prize", "Sixty zippers were quickly picked from the woven jute bag", "The boxed wizards quickly zap a smiling gnome", "A quick movement of the enemy will jeopardize six gunboats", "Mixing jellied plums with zesty lemon makes a fine tart", "The eccentric juggler amazed crowds with odd feats of dexterity", "A dozen movers quickly packed heavy boxes into the van"];
    const long_strings = ["The wizard quickly jinxed the gnomes before they vaporized just beyond the village gates", "Heavy boxes perform quick waltzes and jigs while the young fox plays his fiddle nearby", "My faxed joke won a pager in the cable TV quiz show making everyone in the room laugh", "Back in the quaint valley jovial hikers mixed exotic fruit juice and warm bread by the campfire", "The public was amazed to view the quickness and dexterity of the juggler as he performed his tricks", "Amazingly few discotheques provide jukeboxes making it hard for music lovers to enjoy their favorite tunes", "We promptly judged antique ivory buckles for the next prize in the competition impressing all the judges", "Crazy Frederick bought many very exquisite opal jewels from the ancient market in the old town square", "Sixty zippers were quickly picked from the woven jute bag by the skilled tailor in the bustling city", "Back in June we delivered oxygen equipment of the same size and shape to all the hospitals in the region", "In the sleepy coastal town the fishermen mended their nets beneath a crimson sunset while gulls wheeled overhead", "Under a canopy of stars the traveling minstrel strummed his lute and told tales of distant lands to eager listeners", "During the harvest festival the village square filled with laughter as families shared spiced bread and warm cider by the bonfire", "A curious apprentice studied ancient tomes in the candlelit library dreaming of spells that might mend broken things", "Across the prairie the herd thundering past left clouds of dust and the sun glinted on a thousand tiny hooves"];

    function drawText(text) {
        wordCtx.clearRect(0, 0, wordCanvas.width, wordCanvas.height);
        wordCtx.font = '24px "Times New Roman", Times, serif';
        wordCtx.fillStyle = '#dededeff';
        wordCtx.textAlign = 'center';
    
        const maxWidth = wordCanvas.width - 20; // Leave some padding
        const lineHeight = 30; // Line height for wrapped text
        const lines = wrapText(text, maxWidth);
    
        const startY = (wordCanvas.height - lines.length * lineHeight) / 2; // Center vertically
        lines.forEach((line, index) => {
            wordCtx.fillText(line, wordCanvas.width / 2, startY + index * lineHeight);
        });
    }
    
    function wrapText(text, maxWidth) {
        const words = text.split(' ');
        const lines = [];
        let currentLine = words[0];
    
        for (let i = 1; i < words.length; i++) {
            const word = words[i];
            const width = wordCtx.measureText(currentLine + ' ' + word).width;
            if (width < maxWidth) {
                currentLine += ' ' + word;
            } else {
                lines.push(currentLine);
                currentLine = word;
            }
        }
        lines.push(currentLine); // Add the last line
        return lines;
    }

    function drawUserText(prompt, input) {
        wordCtx.clearRect(0, 0, wordCanvas.width, wordCanvas.height);
        wordCtx.font = '24px "Times New Roman", Times, serif';
        wordCtx.textAlign = 'left';
    
        const maxWidth = wordCanvas.width - 20; // Leave enough padding
        const lineHeight = 30; // Line height for wrapped text
        const lines = wrapText(prompt, maxWidth);
    
        const startY = (wordCanvas.height - lines.length * lineHeight) / 2; // Center vertically
    
        // Draw the prompt text line by line
        lines.forEach((line, lineIndex) => {
            const lineY = startY + lineIndex * lineHeight;
            const lineX = (wordCanvas.width - wordCtx.measureText(line).width) / 2; // Center each line
            wordCtx.fillStyle = '#dededeff';
            wordCtx.fillText(line, lineX, lineY);
    
            // Draw user input for the current line
            let currentX = lineX;
            const startCharIndex = lines.slice(0, lineIndex).join(' ').length + (lineIndex > 0 ? 1 : 0);
            const endCharIndex = startCharIndex + line.length;
    
            for (let i = startCharIndex; i < Math.min(input.length, endCharIndex); i++) {
                const typedChar = input[i];
                const promptChar = prompt[i] || '';
                // Show the prompt character itself; color green if correct, red if incorrect.
                const color = typedChar === promptChar ? 'green' : 'red';
                wordCtx.fillStyle = color;
                wordCtx.fillText(promptChar, currentX, lineY);
                currentX += wordCtx.measureText(promptChar).width;
            }

            // Draw caret if the caret position (input.length) is on this line and game not finished
            if (!finished) {
                const caretIndex = input.length;
                if (caretIndex >= startCharIndex && caretIndex <= endCharIndex) {
                    // caret X is currentX (after drawn chars on this line)
                    const caretX = currentX;
                    const caretY = lineY - 20; // approximate top of text
                    // Blinking: visible half the time
                    const showCaret = Math.floor(Date.now() / 500) % 2 === 0;
                    if (showCaret) {
                        wordCtx.fillStyle = '#ffffff';
                        const caretWidth = 2;
                        const caretHeight = 22;
                        wordCtx.fillRect(caretX, caretY, caretWidth, caretHeight);
                    }
                }
            }
        });
    }

    function updateStats(prompt, input, startTime) {
        // Accuracy calculation
        const totalTyped = input.length;
        // Only show stats when the game has finished
        if (finished) {
            const accuracy = totalTyped > 0 ? Math.round(((totalTyped - mistakes) / totalTyped) * 100) : 100;
            document.querySelector('.accuracy').textContent = accuracy + '%';

            // WPM calculation
            if (startTime) {
                const elapsed = (Date.now() - startTime) / 1000 / 60; // minutes
                const words = prompt.length / 5;
                const wpm = elapsed > 0 ? Math.round(words / elapsed) : 0;
                document.querySelector('.wpm').textContent = wpm;
            } else {
                document.querySelector('.wpm').textContent = '0';
            }
            // Ensure stats are visible
            document.querySelector('.wpm').style.visibility = 'visible';
            document.querySelector('.accuracy').style.visibility = 'visible';
        }
    }

    function finishGame(prompt, input, startTime) {
        finished = true;
        // stop caret redraws
        if (caretInterval) { clearInterval(caretInterval); caretInterval = null; }
        // Compute final mistakes by comparing prompt vs input (count mismatches and missing/extra chars)
        let finalMistakes = 0;
        const maxLen = Math.max(prompt.length, input.length);
        for (let i = 0; i < maxLen; i++) {
            if (prompt[i] !== input[i]) finalMistakes++;
        }
        mistakes = finalMistakes;
        updateStats(prompt, input, startTime);
        // Reveal stats
        document.querySelector('.wpm').style.visibility = 'visible';
        document.querySelector('.accuracy').style.visibility = 'visible';
    setProgress(100);

    const wpm = document.querySelector('.wpm').textContent;
    const accuracy = document.querySelector('.accuracy').textContent;

    // Create the finish screen overlay
    const finishScreen = document.createElement('div');
    finishScreen.style.position = 'fixed';
    finishScreen.style.top = '0';
    finishScreen.style.left = '0';
    finishScreen.style.width = '100%';
    finishScreen.style.height = '100%';
    finishScreen.style.backgroundColor = 'rgba(0, 0, 0, 0.8)';
    finishScreen.style.display = 'flex';
    finishScreen.style.justifyContent = 'center';
    finishScreen.style.alignItems = 'center';
    finishScreen.style.zIndex = '1000';
    finishScreen.style.color = 'white';
    finishScreen.style.flexDirection = 'column';
    finishScreen.style.textAlign = 'center';
    finishScreen.innerHTML = `
        <h2 style="color: #6be3a8;">🎉 Game Finished!</h2>
        <p style="font-size: 2em; margin: 20px;">Your results are in!</p>
        <p style="font-size: 1.5em;"><strong>WPM:</strong> <span style="color: #6bb6ff;">${wpm}</span></p>
    <p style="font-size: 1.5em;"><strong>Accuracy:</strong> <span style="color: #6bb6ff;">${accuracy}</span></p>
    <p style="font-size: 1.2em; margin-top: 10px;"><strong>Mistakes:</strong> <span style="color: #ff6b6b;">${mistakes}</span></p>
    <button id="playAgain" style="margin-top: 30px; padding: 10px 20px; font-size: 1em; cursor: pointer; border: none; background-color: #007BFF; color: white; border-radius: 5px;">Next</button>
    `;

    document.body.appendChild(finishScreen);

    // Add an event listener to the "Next" button: clear scores and let the user pick again
    document.getElementById('playAgain').addEventListener('click', () => {
        document.body.removeChild(finishScreen);
        // Clear the game state but do NOT start a new prompt automatically.
        userInput = '';
        mistakes = 0;
        // mark finished so typing is ignored until a new game is started
        finished = true;
        startTime = null;
        // Stop caret blinking if running
        if (caretInterval) { clearInterval(caretInterval); caretInterval = null; }
        // Clear the canvas (no prompt shown) and reset progress
        drawText('');
        setProgress(0);
        // Hide and clear stats so user can pick again without seeing old scores
        const w = document.querySelector('.wpm');
        const a = document.querySelector('.accuracy');
        if (w) { w.style.visibility = 'hidden'; w.textContent = ''; }
        if (a) { a.style.visibility = 'hidden'; a.textContent = ''; }
        // Clear current selection so the user is encouraged to pick a new length
        currentString = '';
        // Disable global key handler so keystrokes do nothing until startGame rebinds it
        document.onkeydown = null;
    });
}

    function startGame() {
        if (currentString === "") {
            alert("Please select a string length from the options menu.");
            return;
        }

        let stringArray;
        if (currentString === "short_strings") {
            stringArray = short_strings;
        } else if (currentString === "medium_strings") {
            stringArray = medium_strings;
        } else if (currentString === "long_strings") {
            stringArray = long_strings;
        }

        const randomIndex = Math.floor(Math.random() * stringArray.length);
        const selectedString = stringArray[randomIndex];
        userInput = "";
        mistakes = 0; // Reset mistakes at the start of the game
    finished = false;
    // Do not start the timer until the user types the first character
    startTime = null;
        drawText(selectedString);
    // Hide stats while typing
    document.querySelector('.wpm').textContent = '';
    document.querySelector('.accuracy').textContent = '';
    document.querySelector('.wpm').style.visibility = 'hidden';
    document.querySelector('.accuracy').style.visibility = 'hidden';

        document.onkeydown = function (e) {
            if (finished) return;

            if (e.key.length === 1 && userInput.length < selectedString.length) {
                // Start timer on first character
                if (!startTime) startTime = Date.now();
                // Start caret redraw interval
                if (!caretInterval) {
                    caretInterval = setInterval(() => drawUserText(selectedString, userInput), 250);
                }
                // Record typed character
                userInput += e.key;
            } else if (e.key === 'Backspace' && userInput.length > 0) {
                userInput = userInput.slice(0, -1);
            }

            drawUserText(selectedString, userInput);
            updateStats(selectedString, userInput, startTime);
            updateProgress(selectedString, userInput);

            if (userInput.length === selectedString.length) {
                finishGame(selectedString, userInput, startTime);
            }
        };
        // init progress
        setProgress(0);
    }

    function setProgress(percent){
        percent = Math.max(0, Math.min(100, percent));
        progressFill.style.width = percent + '%';
        progressText.textContent = Math.round(percent) + '%';
    }

    function updateProgress(prompt, input){
        const pct = (input.length / prompt.length) * 100;
        setProgress(pct);
    }

    optionsButton.addEventListener('click', () => {
        // Create overlay
        const overlay = document.createElement('div');
        overlay.style.position = 'fixed';
        overlay.style.top = '0';
        overlay.style.left = '0';
        overlay.style.width = '100%';
        overlay.style.height = '100%';
        overlay.style.backgroundColor = 'rgba(0,0,0,0.6)';
        overlay.style.display = 'flex';
        overlay.style.justifyContent = 'center';
        overlay.style.alignItems = 'center';
        overlay.style.zIndex = '2000';

        // Create modal
        const modal = document.createElement('div');
        modal.style.width = '360px';
        modal.style.maxWidth = '90%';
        modal.style.background = 'linear-gradient(180deg,#1b1b2f,#2d3350)';
        modal.style.borderRadius = '12px';
        modal.style.padding = '18px';
        modal.style.boxShadow = '0 10px 30px rgba(0,0,0,0.4)';
        modal.style.color = '#fff';
        modal.style.textAlign = 'center';

        // Close (X) button
        const closeBtn = document.createElement('button');
        closeBtn.innerHTML = '&times;';
        closeBtn.setAttribute('aria-label', 'Close');
        closeBtn.style.position = 'absolute';
        closeBtn.style.top = '12px';
        closeBtn.style.right = '18px';
        closeBtn.style.background = 'transparent';
        closeBtn.style.border = 'none';
        closeBtn.style.color = '#fff';
        closeBtn.style.fontSize = '22px';
        closeBtn.style.cursor = 'pointer';

        const title = document.createElement('h3');
        title.textContent = 'Select String Length';
        title.style.marginTop = '4px';
        title.style.marginBottom = '12px';
        title.style.fontWeight = '700';

        const createOptionButton = (text, val) => {
            const b = document.createElement('button');
            b.textContent = text;
            b.style.display = 'block';
            b.style.width = '100%';
            b.style.margin = '8px 0';
            b.style.padding = '12px 10px';
            b.style.borderRadius = '8px';
            b.style.border = '1px solid rgba(255,255,255,0.08)';
            b.style.background = 'linear-gradient(90deg,#6be3a8,#6bb6ff)';
            b.style.color = '#071127';
            b.style.fontWeight = '700';
            b.style.cursor = 'pointer';
            b.addEventListener('mouseenter', () => b.style.transform = 'translateY(-2px)');
            b.addEventListener('mouseleave', () => b.style.transform = 'translateY(0)');
            b.addEventListener('click', () => {
                currentString = val;
                startGame();
                document.body.removeChild(overlay);
                window.removeEventListener('keydown', escHandler);
            });
            return b;
        };

        const shortOption = createOptionButton('Short Strings', 'short_strings');
        const mediumOption = createOptionButton('Medium Strings', 'medium_strings');
        const longOption = createOptionButton('Long Strings', 'long_strings');

        // Escape handler to close modal
        const escHandler = (ev) => {
            if (ev.key === 'Escape') {
                if (document.body.contains(overlay)) document.body.removeChild(overlay);
                window.removeEventListener('keydown', escHandler);
            }
        };

        closeBtn.addEventListener('click', () => {
            if (document.body.contains(overlay)) document.body.removeChild(overlay);
            window.removeEventListener('keydown', escHandler);
        });

        modal.appendChild(closeBtn);
        modal.appendChild(title);
        modal.appendChild(shortOption);
        modal.appendChild(mediumOption);
        modal.appendChild(longOption);
        overlay.appendChild(modal);
        document.body.appendChild(overlay);

        // Focus and esc listener
        window.addEventListener('keydown', escHandler);
        shortOption.focus();
    });
</script>
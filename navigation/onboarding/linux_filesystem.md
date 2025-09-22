---
layout: base
title: Linux Filesystem Basics
permalink: /onboarding/linux-filesystem
---

<style>
/* Reuse the stepper styles from KASM setup (kept local to this page) */
.stepper { max-width: 880px; margin: 18px auto; background: rgba(255,255,255,0.02); padding: 18px; border-radius: 10px; }
.step { display: none; }
.step.active { display: block; }
.stepper-header { display:flex; justify-content:space-between; align-items:center; margin-bottom: 10px; }
.step-count { color: #cfe8ff; font-weight:600 }
.cmd { background:#0b1220; color:#9fe2a8; padding:10px 12px; border-radius:6px; font-family: monospace; display:inline-block; }
.controls { display:flex; gap:8px; align-items:center }
.arrow { background: #ffffff; color: #0b1220; padding: 6px 10px; border-radius: 6px; border: 1px solid rgba(255,255,255,0.08); cursor: pointer; font-weight: 600; }
.copy-btn { background: #ffffff; color: #0b1220; padding: 6px 10px; border-radius: 6px; border: 1px solid rgba(255,255,255,0.08); cursor: pointer; font-weight: 600; margin-left:8px; }
.lesson { background: linear-gradient(180deg,#071127,#0e2946); padding:12px; border-radius:8px; color:#e6eef8; overflow:visible; min-height:220px; transition: all 160ms ease; }
/* Fullscreen / expanded lesson mode: covers most of the viewport so the box appears to "fill the page".
   Uses position:fixed to overlay the page and shows its own scrollbar if content exceeds viewport. */
.lesson.fullscreen {
  position: fixed;
  left: 28px;
  right: 28px;
  top: 72px;
  bottom: 36px;
  z-index: 1200;
  padding: 20px;
  border-radius: 10px;
  overflow: auto; /* allow scrolling inside only when content exceeds viewport */
  box-shadow: 0 10px 40px rgba(0,0,0,0.6);
}
.lesson .lesson-controls { display:flex; justify-content:flex-end; gap:8px; margin-bottom:8px; }
.small { font-size:0.95rem; color:#cbd5e1 }
</style>

<div class="stepper" id="linux-stepper">
  <div class="stepper-header">
    <div>
      <div class="step-count">Linux Basics — Step <span id="step-number">1</span> of <span id="step-total">4</span></div>
      <div class="small">A short guided tour of the Linux filesystem and commands.</div>
    </div>
    <div class="controls">
      <button class="copy-btn" id="copy-cmd">Copy command</button>
      <button class="arrow" id="prev">◀ Prev</button>
      <button class="arrow" id="next">Next ▶</button>
    </div>
  </div>

  <div class="steps">
    <div class="step active" data-step="1">
      <h3>1. What is Linux?</h3>
      <div class="lesson">
        <p class="small">Linux is an operating system kernel and, via distributions (Ubuntu, Fedora, Debian, etc.), a complete OS used on servers, desktops, embedded devices, and containers. It's open-source, stable, and widely used in development and production.</p>
      </div>
    </div>

    <div class="step" data-step="2">
      <h3>2. Why is Linux used?</h3>
      <div class="lesson">
        <p class="small">Linux is popular because it's reliable, efficient, flexible, and free. It's the backbone of most servers, cloud infrastructure, and many developer tools. It provides powerful command-line tools and scripting for automation.</p>
      </div>
    </div>

    <div class="step" data-step="3">
      <h3>3. Files and folders — "everything is a file"</h3>
      <div class="lesson">
        <p class="small">In Linux, files, directories (folders), devices, and even some system interfaces are represented as files. This unified model makes tools simple and composable: you read, write, and pipe data between programs and files.</p>
        <p class="small">Examples: configuration files are plain text under <code>/etc</code>, user files live in <code>/home/&lt;user&gt;</code>, and device interfaces appear under <code>/dev</code>.</p>
      </div>
    </div>

    <div class="step" data-step="4">
      <h3>4. Common commands</h3>
      <div class="lesson">
        <p class="small"><strong>ls</strong> — list files (<code>ls -la</code> for details)</p>
        <p class="small"><strong>cd</strong> — change directory (e.g. <code>cd /path/to/dir</code>)</p>
        <p class="small"><strong>pwd</strong> — print working directory</p>
        <p class="small"><strong>mkdir</strong> — make directories (e.g. <code>mkdir -p a/b</code>)</p>
        <p class="small"><strong>cp</strong> — copy files/directories (e.g. <code>cp source dest</code>)</p>
        <p class="small"><strong>mv</strong> — move / rename files</p>
        <p class="small"><strong>rm</strong> — remove files (<code>rm -r</code> for directories — use carefully)</p>
        <p class="small"><strong>cat</strong> / <strong>less</strong> — view file contents</p>
        <p class="small"><strong>sudo</strong> — run a command as root (administrator)</p>
        <p class="small">Try this sequence in order to explore (copy the commands):</p>
        <pre class="cmd">ls -la
pwd
mkdir demo_folder
cd demo_folder
touch hello.txt
ls -la
cat hello.txt
</pre>
      </div>
    </div>

    <div class="step" data-step="5">
      <h3>You're done!</h3>
      <div class="lesson" style="text-align:center;">
        <p class="small">Nice work — you've completed the quick Linux filesystem tour.</p>
        <p class="small">Click the button below to return to the onboarding navigation.</p>
        <button class="arrow" id="finish-btn" style="margin-top:10px;">You're done — go to onboarding navigation</button>
      </div>
      <div style="height:0; overflow:visible; position:relative;"></div>
    </div>

    <!-- Mini-quiz step -->
    <div class="step" data-step="6">
      <h3>Mini quiz — check your knowledge</h3>
      <div class="lesson">
        <div class="lesson-controls">
          <button type="button" class="copy-btn" id="toggle-expand" aria-pressed="false">Expand</button>
        </div>
        <p class="small">Answer the short multiple-choice quiz. Your score will appear after you submit and correct answers will be revealed.</p>

        <form id="linux-quiz" class="small" style="display:flex;flex-direction:column;gap:10px;">
          <div>
            <strong>1.</strong> Which directory typically contains system-wide configuration files?
            <div><label><input type="radio" name="q1" value="/etc"> /etc</label></div>
            <div><label><input type="radio" name="q1" value="/home"> /home</label></div>
            <div><label><input type="radio" name="q1" value="/dev"> /dev</label></div>
          </div>

          <div>
            <strong>2.</strong> Which command shows the current working directory?
            <div><label><input type="radio" name="q2" value="pwd"> pwd</label></div>
            <div><label><input type="radio" name="q2" value="ls"> ls</label></div>
            <div><label><input type="radio" name="q2" value="cd"> cd</label></div>
          </div>

          <div>
            <strong>3.</strong> The command to create nested directories in one step is:
            <div><label><input type="radio" name="q3" value="mkdir -p"> mkdir -p</label></div>
            <div><label><input type="radio" name="q3" value="touch -r"> touch -r</label></div>
            <div><label><input type="radio" name="q3" value="cp -r"> cp -r</label></div>
          </div>

          <div style="display:flex;gap:8px;margin-top:6px;align-items:center;">
            <button type="button" class="arrow" id="submit-quiz">Submit</button>
            <div id="quiz-result" class="small" aria-live="polite"></div>
          </div>

          <details id="quiz-answers" style="margin-top:8px;">
            <summary class="small">Show correct answers</summary>
            <ol class="small">
              <li><code>/etc</code> — system-wide configuration files</li>
              <li><code>pwd</code> — print working directory</li>
              <li><code>mkdir -p</code> — create parent directories as needed</li>
            </ol>
          </details>
        </form>
      </div>
    </div>

  </div>
</div>

<script>
(function(){
  const steps = Array.from(document.querySelectorAll('#linux-stepper .step'));
  const total = steps.length;
  const stepNumber = document.getElementById('step-number');
  const stepTotal = document.getElementById('step-total');
  const prev = document.getElementById('prev');
  const next = document.getElementById('next');
  const copyBtn = document.getElementById('copy-cmd');
  let idx = 0;
  stepTotal.textContent = total;

  function show(i){
    steps.forEach(s => s.classList.remove('active'));
    steps[i].classList.add('active');
    stepNumber.textContent = i+1;
    const code = steps[i].querySelector('.cmd');
    copyBtn.textContent = code ? 'Copy command' : 'Copy';
    prev.disabled = (i === 0);
    next.disabled = (i === total-1);
  }

  prev.addEventListener('click', ()=>{ if(idx>0) idx--; show(idx); maybeCelebrate(idx); });
  next.addEventListener('click', ()=>{ if(idx<total-1) idx++; show(idx); maybeCelebrate(idx); });

  // simple confetti impl (small, dependency-free)
  function burstConfetti(x, y, count){
    const colors = ['#ff6b6b','#ffd93d','#6be3a8','#6bb6ff','#c58cff'];
    for(let i=0;i<count;i++){
      const el = document.createElement('div');
      el.style.position = 'fixed';
      el.style.left = (x + (Math.random()-0.5)*120) + 'px';
      el.style.top = (y + (Math.random()-0.5)*40) + 'px';
      el.style.width = el.style.height = (6 + Math.floor(Math.random()*8)) + 'px';
      el.style.background = colors[Math.floor(Math.random()*colors.length)];
      el.style.opacity = '0.95';
      el.style.transform = 'rotate(' + (Math.random()*360) + 'deg)';
      el.style.borderRadius = (Math.random()>0.6? '2px':'50%');
      el.style.zIndex = 9999;
      document.body.appendChild(el);
      const dx = (Math.random()-0.5)*800;
      const dy = 600 + Math.random()*200;
      el.animate([
        { transform: 'translate(0,0) rotate(0deg)', opacity:1 },
        { transform: `translate(${dx}px, ${dy}px) rotate(${Math.random()*720}deg)`, opacity:0 }
      ], { duration: 1400 + Math.random()*800, easing: 'cubic-bezier(.2,.8,.2,1)' });
      setTimeout(()=> el.remove(), 2500);
    }
  }

  const finishBtn = document.getElementById('finish-btn');
  function maybeCelebrate(i){
    if(i === total-1){
      const rect = document.getElementById('linux-stepper').getBoundingClientRect();
      burstConfetti(rect.left + rect.width/2, rect.top + 40, 36);
      setTimeout(()=> burstConfetti(rect.left + rect.width/2 - 80, rect.top + 20, 18), 300);
      setTimeout(()=> burstConfetti(rect.left + rect.width/2 + 80, rect.top + 20, 18), 600);
    }
  }

  if(finishBtn){
    finishBtn.addEventListener('click', ()=>{
      try{ localStorage.setItem('onboard:completed:linux-filesystem','1'); }catch(e){}
      window.location.href = '{{ site.baseurl }}/onboarding/navigation';
    });
  }

  copyBtn.addEventListener('click', ()=>{
    const codeEl = steps[idx].querySelector('.cmd');
    if(!codeEl) return;
    const text = codeEl.textContent.trim();
    navigator.clipboard.writeText(text).then(()=>{
      copyBtn.textContent = 'Copied!';
      setTimeout(()=> copyBtn.textContent = 'Copy command', 1200);
    });
  });

  // Quiz grading logic (for the mini-quiz step)
  const submitQuiz = document.getElementById('submit-quiz');
  const quizResult = document.getElementById('quiz-result');
  const quizForm = document.getElementById('linux-quiz');
  const quizAnswers = { q1: '/etc', q2: 'pwd', q3: 'mkdir -p' };

  function gradeQuiz(){
    if(!quizForm) return;
    const form = new FormData(quizForm);
    let score = 0; let total = 0; let missing = false;
    Object.keys(quizAnswers).forEach(k => {
      total += 1;
      const val = form.get(k);
      if(!val) missing = true;
      if(val === quizAnswers[k]) score += 1;
    });
    if(missing){ quizResult.textContent = 'Please answer all questions.'; quizResult.style.color = '#e09'; return; }
    quizResult.textContent = `Score: ${score} / ${total}`;
    quizResult.style.color = score === total ? '#2ea44f' : '#ffd166';
    // open the details to show correct answers so learners can review
    const det = document.getElementById('quiz-answers');
    if(det && !det.open) det.open = true;
  }

  if(submitQuiz){ submitQuiz.addEventListener('click', gradeQuiz); }

  // Expand / collapse lesson full-screen toggle
  const toggleExpand = document.getElementById('toggle-expand');
  const quizLesson = document.querySelector('.step[data-step="6"] .lesson');
  if(toggleExpand && quizLesson){
    toggleExpand.addEventListener('click', ()=>{
      const isFull = quizLesson.classList.toggle('fullscreen');
      toggleExpand.textContent = isFull ? 'Collapse' : 'Expand';
      toggleExpand.setAttribute('aria-pressed', isFull ? 'true' : 'false');
      // when expanded, ensure it's visible in viewport
      if(isFull) quizLesson.focus();
    });
  }

  // initialize
  show(0);
})();
</script>

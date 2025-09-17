---
layout: base
title: Software Development Life Cycle (SDLC)
permalink: /onboarding/sdlc
---

<style>
/* Reuse stepper styles used elsewhere */
.stepper { max-width: 880px; margin: 18px auto; background: rgba(255,255,255,0.02); padding: 18px; border-radius: 10px; }
.step { display: none; }
.step.active { display: block; }
.stepper-header { display:flex; justify-content:space-between; align-items:center; margin-bottom: 10px; }
.step-count { color: #cfe8ff; font-weight:600 }
.cmd { background:#0b1220; color:#9fe2a8; padding:10px 12px; border-radius:6px; font-family: monospace; display:inline-block; }
.controls { display:flex; gap:8px; align-items:center }
.arrow { background: #ffffff; color: #0b1220; padding: 6px 10px; border-radius: 6px; border: 1px solid rgba(255,255,255,0.08); cursor: pointer; font-weight: 600; }
.arrow:disabled{ opacity:0.45 }
.copy-btn { background: #ffffff; color: #0b1220; padding: 6px 10px; border-radius: 6px; border: 1px solid rgba(255,255,255,0.08); cursor: pointer; font-weight: 600; margin-left:8px; }
.lesson { background: linear-gradient(180deg,#071127,#0e2946); padding:12px; border-radius:8px; color:#e6eef8 }
.small { font-size:0.95rem; color:#cbd5e1 }
</style>

<div class="stepper" id="sdlc-stepper">
  <div class="stepper-header">
    <div>
      <div class="step-count">SDLC — Step <span id="step-number">1</span> of <span id="step-total">5</span></div>
      <div class="small">A short guide to the Software Development Life Cycle for building your GitHub Pages site.</div>
    </div>
    <div class="controls">
      <button class="copy-btn" id="copy-cmd">Copy command</button>
      <button class="arrow" id="prev">◀ Prev</button>
      <button class="arrow" id="next">Next ▶</button>
    </div>
  </div>

  <div class="steps">
    <div class="step active" data-step="1">
      <h3>1. Overview</h3>
      <div class="lesson">
        <p class="small">The development cycle involves iterative steps of running the server, making changes, testing, committing, and syncing changes to GitHub. This ensures that your website is updated and functioning correctly both locally and on GitHub Pages.</p>
      </div>
    </div>

    <div class="step" data-step="2">
      <h3>2. SDLC Workflow</h3>
      <div class="lesson">
        <pre class="cmd">Run Server  ->  Make Changes  ->  Commit  ->  Test  ->  Sync</pre>
        <p class="small">Start the local server, edit files, commit locally, verify the site, then push changes to GitHub.</p>
      </div>
    </div>

    <div class="step" data-step="3">
      <h3>3. What is make?</h3>
      <div class="lesson">
        <p class="small">Think of make as a smart task helper for developers. It automates commands you would normally type one by one, starts a localhost server, and runs project tasks listed in the Makefile.</p>
        <p class="small">To run the default workflow type:</p>
        <div class="cmd">make</div>
        <p class="small">Other useful commands: <code>make clean</code>, <code>make stop</code>, <code>make convert</code></p>
      </div>
    </div>

    <div class="step" data-step="4">
      <h3>4. VS Code Commit and Sync Workflow</h3>
      <div class="lesson">
        <p class="small">Save files in VS Code, stage and commit via the Source Control pane, then push your commits to GitHub. Always test locally (make) before pushing — this prevents broken site builds on GitHub Pages.</p>
        <p class="small">Detailed steps:</p>
        <ol class="small">
          <li>Edit files and save (Ctrl/Cmd+S)</li>
          <li>Open Source Control, stage files, add a commit message, and commit</li>
          <li>Open a terminal, ensure your venv is active, run <code>make</code> and verify the local site</li>
          <li>Push to the remote repository to trigger GitHub Pages deployment</li>
        </ol>
      </div>
    </div>

    <div class="step" data-step="5">
      <h3>You're done!</h3>
      <div class="lesson" style="text-align:center;">
        <p class="small">You're familiar with the SDLC: run the server, edit, test, commit, and sync. Follow this cycle whenever you make changes to your site.</p>
        <p class="small">Click the button below to mark this tutorial complete and return to onboarding navigation.</p>
        <button class="arrow" id="finish-btn" style="margin-top:10px;">You're done — go to onboarding navigation</button>
      </div>
      <div style="height:0; overflow:visible; position:relative;"></div>
    </div>

  </div>
</div>

<script>
(function(){
  const steps = Array.from(document.querySelectorAll('#sdlc-stepper .step'));
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

  copyBtn.addEventListener('click', ()=>{
    const codeEl = steps[idx].querySelector('.cmd');
    if(!codeEl) return;
    const text = codeEl.textContent.trim();
    navigator.clipboard.writeText(text).then(()=>{
      copyBtn.textContent = 'Copied!';
      setTimeout(()=> copyBtn.textContent = 'Copy command', 1200);
    });
  });

  prev.addEventListener('click', ()=>{ if(idx>0) idx--; show(idx); maybeCelebrate(idx); });
  next.addEventListener('click', ()=>{ if(idx<total-1) idx++; show(idx); maybeCelebrate(idx); });

  // simple confetti impl
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
      const rect = document.getElementById('sdlc-stepper').getBoundingClientRect();
      burstConfetti(rect.left + rect.width/2, rect.top + 40, 36);
      setTimeout(()=> burstConfetti(rect.left + rect.width/2 - 80, rect.top + 20, 18), 300);
      setTimeout(()=> burstConfetti(rect.left + rect.width/2 + 80, rect.top + 20, 18), 600);
    }
  }

  if(finishBtn){
    finishBtn.addEventListener('click', ()=>{
      try{ localStorage.setItem('onboard:completed:sdlc','1'); }catch(e){}
      window.location.href = '{{ site.baseurl }}/onboarding/navigation';
    });
  }

  // initialize
  show(0);
})();
</script>

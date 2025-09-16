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
.lesson { background: linear-gradient(180deg,#071127,#0e2946); padding:12px; border-radius:8px; color:#e6eef8 }
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

  prev.addEventListener('click', ()=>{ if(idx>0) idx--; show(idx); });
  next.addEventListener('click', ()=>{ if(idx<total-1) idx++; show(idx); });

  copyBtn.addEventListener('click', ()=>{
    const codeEl = steps[idx].querySelector('.cmd');
    if(!codeEl) return;
    const text = codeEl.textContent.trim();
    navigator.clipboard.writeText(text).then(()=>{
      copyBtn.textContent = 'Copied!';
      setTimeout(()=> copyBtn.textContent = 'Copy command', 1200);
    });
  });

  // initialize
  show(0);
})();
</script>

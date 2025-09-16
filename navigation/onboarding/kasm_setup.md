---
layout: base
title: KASM Setup
permalink: /onboarding/kasm-setup
---

<style>
/* Simple stepper styles */
.stepper { max-width: 880px; margin: 18px auto; background: rgba(255,255,255,0.02); padding: 18px; border-radius: 10px; }
.step { display: none; }
.step.active { display: block; }
.stepper-header { display:flex; justify-content:space-between; align-items:center; margin-bottom: 10px; }
.step-count { color: #cfe8ff; font-weight:600 }
.cmd { background:#0b1220; color:#9fe2a8; padding:10px 12px; border-radius:6px; font-family: monospace; display:inline-block; }
.controls { display:flex; gap:8px; align-items:center }
.arrow {
	background: #ffffff;
	color: #0b1220;
	padding: 6px 10px;
	border-radius: 6px;
	border: 1px solid rgba(255,255,255,0.08);
	cursor: pointer;
	font-weight: 600;
}
.arrow:disabled{ opacity:0.45 }
.copy-btn {
	background: #ffffff;
	color: #0b1220;
	padding: 6px 10px;
	border-radius: 6px;
	border: 1px solid rgba(255,255,255,0.08);
	cursor: pointer;
	font-weight: 600;
	margin-left:8px;
}
.lesson { background: linear-gradient(180deg,#071127,#0e2946); padding:12px; border-radius:8px; color:#e6eef8 }
.small { font-size:0.9rem; color:#cbd5e1 }
</style>

<div class="stepper" id="kasm-stepper">
	<div class="stepper-header">
		<div>
			<div class="step-count">KASM Setup — Step <span id="step-number">1</span> of <span id="step-total">9</span></div>
			<div class="small">Follow these commands in order. Use the arrows to move between steps.</div>
		</div>
		<div class="controls">
			<button class="copy-btn" id="copy-cmd">Copy command</button>
			<button class="arrow" id="prev">◀ Prev</button>
			<button class="arrow" id="next">Next ▶</button>
		</div>
	</div>

	<div class="steps">
		<div class="step active" data-step="1">
			<h3>1. Create a workspace folder</h3>
			<div class="cmd">mkdir opencs</div>
			<p class="small">Creates a new directory named <code>opencs</code> in your current working directory.</p>
		</div>

		<div class="step" data-step="2">
			<h3>2. Enter the folder</h3>
			<div class="cmd">cd opencs</div>
			<p class="small">Switch your shell into the <code>opencs</code> folder so subsequent commands operate there.</p>
		</div>

		<div class="step" data-step="3">
			<h3>3. Clone the repository</h3>
			<div class="cmd">git clone https://github.com/Open-Coding-Society/student.git</div>
			<p class="small">Downloads a local copy of the course repository (including history). Replace the URL if you're given a different repo.</p>
		</div>

		<div class="step" data-step="4">
			<h3>4. Enter the project</h3>
			<div class="cmd">cd student/</div>
			<p class="small">Move into the freshly cloned project directory so you can run project scripts.</p>
		</div>

		<div class="step" data-step="5">
			<h3>5. Mini-lesson: Basic shell commands</h3>
			<div class="lesson">
				<p><strong>ls</strong> — list files and folders in the current directory. Try <code>ls -la</code> to see hidden files and permissions.</p>
				<p><strong>cd &lt;folder&gt;</strong> — change directory. Use <code>cd ..</code> to move up one level.</p>
				<p><strong>mkdir &lt;folder&gt;</strong> — make a new directory. Use <code>mkdir -p a/b/c</code> to create nested directories at once.</p>
				<p class="small">These simple commands are the basis for navigating and organizing files on the command line. Practice them until they feel natural — they speed up every workflow.</p>
			</div>
		</div>

		<div class="step" data-step="6">
			<h3>6. Run the project's activation script</h3>
			<div class="cmd">./scripts/activate.sh</div>
			<p class="small">Some projects include a script to set up environment variables or helper functions. If it isn't executable, run <code>chmod +x ./scripts/activate.sh</code> or execute it with <code>bash ./scripts/activate.sh</code>.</p>
		</div>

		<div class="step" data-step="7">
			<h3>7. Create and activate the virtualenv</h3>
			<div class="cmd">./scripts/venv.sh</div>
			<p class="small">This script typically creates a Python virtual environment and installs the project's dependencies so they don't affect your system Python. On Windows PowerShell you may need to run <code>.\venv\Scripts\Activate.ps1</code> or <code>.\venv\Scripts\activate</code> depending on the shell.</p>
		</div>

		<div class="step" data-step="8">
			<h3>8. Open the project in VS Code</h3>
			<div class="cmd">code .</div>
			<p class="small">Opens Visual Studio Code in the current folder so you can edit files and use integrated terminals. If <code>code</code> is not recognized, enable the 'code' shell command from VS Code's Command Palette.</p>
		</div>

		<div class="step" data-step="9">
			<h3>You're done!</h3>
			<div class="lesson" style="text-align:center;">
				<p class="small">Congratulations — you've finished the setup steps.</p>
				<p class="small">Click the button below to return to the onboarding navigation.</p>
				<button class="arrow" id="finish-btn" style="margin-top:10px;" href="{{site.baseurl}}/onboarding/navigation">You're done — go to onboarding navigation</button>
			</div>
			<div style="height:0; overflow:visible; position:relative;"></div>
		</div>

		</div>
</div>

<script>
	(function(){
		const steps = Array.from(document.querySelectorAll('#kasm-stepper .step'));
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
			// update copy button text
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

		// initialize
		show(0);

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

		// trigger confetti when showing the last step
		const finishBtn = document.getElementById('finish-btn');
		function maybeCelebrate(i){
			if(i === total-1){
				const rect = document.getElementById('kasm-stepper').getBoundingClientRect();
				burstConfetti(rect.left + rect.width/2, rect.top + 40, 36);
				setTimeout(()=> burstConfetti(rect.left + rect.width/2 - 80, rect.top + 20, 18), 300);
				setTimeout(()=> burstConfetti(rect.left + rect.width/2 + 80, rect.top + 20, 18), 600);
			}
		}

		prev.addEventListener('click', ()=>{ if(idx>0) idx--; show(idx); maybeCelebrate(idx); });
		next.addEventListener('click', ()=>{ if(idx<total-1) idx++; show(idx); maybeCelebrate(idx); });

		// wire the finish button to the onboarding navigation
		if(finishBtn){
			finishBtn.addEventListener('click', ()=>{
				window.location.href = '{{ site.baseurl }}/onboarding/navigation';
			});
		}
	})();
</script>




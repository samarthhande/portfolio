---
layout: base
title: Onboarding Navigation
permalink: /onboarding/navigation
---
<style>
/* Small game-style onboarding grid */
.onboard-game { max-width:760px; margin:22px auto; text-align:center; }
.grid { display:grid; grid-template-columns: repeat(3, 1fr); gap:12px; margin:18px 0; }
.tile { background:#0b1220; color:#e6eef8; padding:20px; border-radius:10px; border:2px solid rgba(255,255,255,0.03); cursor:pointer; font-weight:700; box-shadow: 0 6px 18px rgba(2,6,23,0.5); user-select:none; }
.tile:focus { outline:3px solid rgba(99,102,241,0.22); }
.tile.selected { background: #ffffff; color:#071127; transform: translateY(-4px); box-shadow: 0 10px 30px rgba(2,6,23,0.6); }
.tile.done { background: linear-gradient(90deg,#0f5132,#0d3e2a); color: #dff6e1; border-color: rgba(107,189,129,0.18); box-shadow: 0 6px 18px rgba(6,55,12,0.45); }
.tile .label { display:block; font-size:0.85rem; color:inherit; opacity:0.8; }
.controls-small { color:#cbd5e1; font-size:0.95rem; margin-top:6px }
.hint { color:#9fe2a8; font-family: monospace; background:#071127; display:inline-block; padding:6px 8px; border-radius:6px; margin-top:10px }
</style>

<div class="onboard-game">
	<h2>Onboarding Activity</h2>
	<p class="controls-small">Use the arrow keys or WASD to move. Press <span class="hint">Enter</span> or click a tile to open that step.</p>

	<div class="grid" id="nav-grid" role="application" aria-label="Onboarding navigation grid">
		<button class="tile" data-row="0" data-col="0" data-href="{{  site.baseurl  }}/onboarding/kasm-setup">
			<div class="num">1</div>
			<div class="label">KASM setup</div>
		</button>
		<button class="tile" data-row="0" data-col="1" data-href="{{  site.baseurl  }}/onboarding/linux-filesystem">
			<div class="num">2</div>
			<div class="label">Linux filesystem tutorial</div>
		</button>
		<button class="tile" data-row="0" data-col="2" data-href="{{  site.baseurl  }}/onboarding/sdlc">
			<div class="num">3</div>
			<div class="label">Software Development Life Cycle</div>
		</button>
	</div>

</div>

<script>
	(function(){
		const rows = 3, cols = 3;
		let r = 0, c = 0; // start at top-left (KASM)
		const grid = document.getElementById('nav-grid');
		const tiles = Array.from(grid.querySelectorAll('.tile'));

		function findTile(row, col){
			return tiles.find(t => Number(t.dataset.row) === row && Number(t.dataset.col) === col);
		}

		function setSelected(row, col){
			tiles.forEach(t => t.classList.remove('selected'));
			const t = findTile(row,col);
			if(t){ t.classList.add('selected'); t.focus(); }
		}

		function clamp(v, a, b){ return Math.max(a, Math.min(b, v)); }

		function activateTile(row, col){
			const t = findTile(row,col);
			if(!t) return;
			const href = t.dataset.href;
			if(href && href !== '#'){
				window.location.href = href;
			} else {
				// small feedback for placeholders
				t.animate([{ transform: 'scale(1)' },{ transform: 'scale(1.04)' },{ transform: 'scale(1)' }], { duration: 240 });
			}
		}

		// keyboard nav
		document.addEventListener('keydown', (ev)=>{
			const key = ev.key.toLowerCase();
			let moved = false;
			if(['arrowup','w'].includes(key)) { r = clamp(r-1,0,rows-1); moved = true; }
			if(['arrowdown','s'].includes(key)) { r = clamp(r+1,0,rows-1); moved = true; }
			if(['arrowleft','a'].includes(key)) { c = clamp(c-1,0,cols-1); moved = true; }
			if(['arrowright','d'].includes(key)) { c = clamp(c+1,0,cols-1); moved = true; }
			if(moved){ ev.preventDefault(); setSelected(r,c); }
			if(key === 'enter' || key === ' ') { ev.preventDefault(); activateTile(r,c); }
		});

		// click handlers
		tiles.forEach(t => {
			// decorate tiles based on completion stored in localStorage
			(function decorateDone(tile){
				try{
					const href = tile.dataset.href || '';
					if(!href) return;
					const parts = href.replace(/\/$/, '').split('/');
					const key = parts[parts.length-1];
					if(localStorage.getItem('onboard:completed:' + key)){
						tile.classList.add('done');
					}
				}catch(e){ /* ignore */ }
			})(t);
			t.addEventListener('click', ()=>{
				const row = Number(t.dataset.row), col = Number(t.dataset.col);
				r = row; c = col; setSelected(r,c); activateTile(r,c);
			});
			t.addEventListener('focus', ()=>{
				r = Number(t.dataset.row); c = Number(t.dataset.col); setSelected(r,c);
			});
		});

		// initialize selection on load
		setSelected(r,c);
	})();
</script>


<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Prompt Gallery — Premium</title>
  <meta name="description" content="Premium Prompt Gallery — add photo prompts, copy prompts, save favorites." />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg1: linear-gradient(135deg,#0f172a 0%, #071636 50%, #2b2340 100%);
      --glass: rgba(255,255,255,0.06);
      --glass-2: rgba(255,255,255,0.04);
      --accent: #7c3aed;
      --accent-2: #06b6d4;
      --muted: rgba(255,255,255,0.7);
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
    }
    html,body{height:100%;margin:0;background:var(--bg1);color:white;-webkit-font-smoothing:antialiased}
    .wrap{max-width:1150px;margin:28px auto;padding:22px}
    header{display:flex;align-items:center;justify-content:space-between;gap:12px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent-2));display:flex;align-items:center;justify-content:center;font-weight:800;color:white}
    h1{margin:0;font-size:22px}
    p.lead{margin:4px 0 0;color:var(--muted);font-size:13px}

    .controls{display:flex;gap:12px;align-items:center}
    .btn{background:linear-gradient(90deg,var(--accent),var(--accent-2));padding:10px 14px;border-radius:10px;border:none;color:white;font-weight:700;cursor:pointer}
    .ghost{background:transparent;border:1px solid rgba(255,255,255,0.08);padding:10px 12px;border-radius:10px;color:var(--muted);cursor:pointer}

    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:18px;margin-top:22px}
    .card{background:linear-gradient(180deg,rgba(255,255,255,0.03),rgba(255,255,255,0.02));border-radius:14px;padding:12px;backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px);box-shadow:0 8px 30px rgba(2,6,23,0.6);border:1px solid rgba(255,255,255,0.04)}
    .imgwrap{width:100%;height:160px;border-radius:10px;overflow:hidden;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(0,0,0,0.15));display:flex;align-items:center;justify-content:center}
    .imgwrap img{width:100%;height:100%;object-fit:cover}
    .meta{display:flex;justify-content:space-between;align-items:center;margin-top:10px}
    .prompt{font-size:13px;color:var(--muted);line-height:1.4;max-height:72px;overflow:hidden}

    .card-footer{display:flex;justify-content:space-between;align-items:center;margin-top:12px;gap:8px}
    .small{font-size:12px;color:var(--muted)}
    .icon-btn{background:transparent;border:1px solid rgba(255,255,255,0.04);padding:8px;border-radius:10px;cursor:pointer}

    .floating-add{position:fixed;right:22px;bottom:22px}

    .modal-backdrop{position:fixed;inset:0;background:linear-gradient(180deg,rgba(2,6,23,0.6),rgba(2,6,23,0.75));display:none;align-items:center;justify-content:center}
    .modal{width:720px;max-width:94%;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));border-radius:14px;padding:18px;border:1px solid rgba(255,255,255,0.04);backdrop-filter:blur(8px)}
    .form-row{display:flex;gap:12px}
    .input,textarea{width:100%;padding:10px;border-radius:10px;border:1px dashed rgba(255,255,255,0.06);background:transparent;color:white}
    textarea{min-height:120px}
    .drop{border:2px dashed rgba(255,255,255,0.04);border-radius:10px;padding:22px;text-align:center;color:var(--muted);cursor:pointer}

    footer{margin-top:28px;color:var(--muted);font-size:13px}

    @media (max-width:720px){.form-row{flex-direction:column}.imgwrap{height:140px}}
    .badge{font-size:12px;padding:6px 8px;border-radius:999px;background:rgba(255,255,255,0.04);color:var(--muted)}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="brand">
        <div class="logo">PG</div>
        <div>
          <h1>Prompt Gallery — Premium</h1>
          <p class="lead">Image + prompt showcase. Add your photo prompts, copy text, save favorites.</p>
        </div>
      </div>
      <div class="controls">
        <button id="openModal" class="btn">Add Prompt</button>
        <button id="clearStorage" class="ghost">Clear Saved</button>
      </div>
    </header>

    <main>
      <section class="grid" id="gallery"></section>
      <div style="display:flex;justify-content:space-between;align-items:center;margin-top:18px">
        <div class="small">Saved locally in your browser • No server required</div>
        <div class="small">Tip: Click <span class="badge">Copy</span> to use prompt in AI tools</div>
      </div>
      <footer>
        <div>Follow me on Instagram <a href="https://instagram.com/s0hail_23" target="_blank">@s0hail_23</a> | All links: <a href="https://linktr.ee/sohailhussain" target="_blank">linktr.ee/sohailhussain</a></div>
      </footer>
    </main>
  </div>

  <div class="floating-add">
    <button id="openModal2" class="btn">+ Add</button>
  </div>

  <div id="backdrop" class="modal-backdrop">
    <div class="modal" role="dialog" aria-modal="true">
      <h3 style="margin-top:0">Add Photo Prompt</h3>
      <div class="form-row">
        <div style="flex:1">
          <div id="dropZone" class="drop">Drop image here or click to upload<br><small class="small">PNG/JPG (max 5MB)</small></div>
          <input id="imgInput" type="file" accept="image/*" style="display:none" />
        </div>
        <div style="flex:1">
          <input id="title" class="input" placeholder="Title (optional)" />
          <textarea id="promptText" placeholder="Write the image prompt here — e.g. 'A futuristic city at night, neon reflections, cinematic, 4k'" class="input"></textarea>
          <div style="display:flex;gap:8px;margin-top:8px">
            <button id="savePrompt" class="btn">Save Prompt</button>
            <button id="cancel" class="ghost">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <template id="cardTpl">
    <article class="card">
      <div class="imgwrap"><img src="" alt="prompt image"/></div>
      <div class="meta">
        <div>
          <div class="prompt"></div>
          <div style="margin-top:8px;display:flex;gap:8px;align-items:center">
            <button class="icon-btn copyBtn">Copy</button>
            <button class="icon-btn favBtn">♥</button>
            <div class="small date"></div>
          </div>
          <div class="small" style="margin-top:4px">Prompt by <a href="https://instagram.com/s0hail_23" target="_blank">@s0hail_23</a></div>
        </div>
      </div>
    </article>
  </template>

  <script>
    const KEY = 'prompt_gallery_v1';
    const gallery = document.getElementById('gallery');
    const tpl = document.getElementById('cardTpl');
    const backdrop = document.getElementById('backdrop');
    const openModal = document.getElementById('openModal');
    const openModal2 = document.getElementById('openModal2');
    const cancel = document.getElementById('cancel');
    const savePrompt = document.getElementById('savePrompt');
    const imgInput = document.getElementById('imgInput');
    const dropZone = document.getElementById('dropZone');
    const titleInput = document.getElementById('title');
    const promptText = document.getElementById('promptText');
    const clearStorage = document.getElementById('clearStorage');

    function uid(){return Math.random().toString(36).slice(2,9)}
    function nowISO(){return new Date().toISOString().slice(0,19).replace('T',' ')}
    function load(){const raw = localStorage.getItem(KEY);try{return raw?JSON.parse(raw):[]}catch(e){return []}}
    function save(list){localStorage.setItem(KEY,JSON.stringify(list))}

    let prompts = load();

    function render(){
      gallery.innerHTML='';
      prompts.slice().reverse().forEach(item=>{
        const node = tpl.content.cloneNode(true);
        const img = node.querySelector('img');
        const promptEl = node.querySelector('.prompt');
        const copyBtn = node.querySelector('.copyBtn');
        const favBtn = node.querySelector('.favBtn');
        const dateEl = node.querySelector('.date');

        img.src = item.data || placeholderData(item.title||'Image');
        img.alt = item.title || 'Prompt Image';
        promptEl.textContent = item.text || '(no prompt)';
        dateEl.textContent = item.date || '';
        favBtn.textContent = item.fav? '♥':'♡';
        favBtn.style.opacity = item.fav? '1':'0.7';

        copyBtn.addEventListener('click', ()=>{
          navigator.clipboard.writeText(item.text||'').then(()=>{copyBtn.textContent='Copied';setTimeout(()=>copyBtn.textContent='Copy',1200);})
        });

        favBtn.addEventListener('click', ()=>{
          item.fav = !item.fav; save(prompts); render();
        });

        gallery.appendChild(node);
      })
    }

    function placeholderData(txt){const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='800' height='600'><rect width='100%' height='100%' fill='#1f2937'/><text x='50%' y='50%' fill='#9ca3af' font-size='24' font-family='Arial' dominant-baseline='middle' text-anchor='middle'>${escapeHtml(txt)}</text></svg>`;return 'data:image/svg+xml;utf8,'+svg;}
    function escapeHtml(s){return (s+'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')}

    function showModal(){backdrop.style.display='flex'}
    function hideModal(){backdrop.style.display='none';titleInput.value='';promptText.value='';imgData=null}

    openModal.addEventListener('click', showModal)
    openModal2.addEventListener('click', showModal)
    cancel.addEventListener('click', hideModal)

    let imgData = null;
    dropZone.addEventListener('click', ()=>imgInput.click())
    imgInput.addEventListener('change', (e)=>{const f = e.target.files[0]; if(!f) return; readFile(f);})
    dropZone.addEventListener('dragover', (e)=>{e.preventDefault(); dropZone.style.borderColor='rgba(255,255,255,0.18)'});
    dropZone.addEventListener('dragleave', (e)=>{e.preventDefault(); dropZone.style.borderColor='rgba(255,255,255,0.04)'});
    dropZone.addEventListener('drop', (e)=>{e.preventDefault(); dropZone.style.borderColor='rgba(255,255,255,0.04)'; const f = e.dataTransfer.files[0]; if(!f) return; readFile(f);});

    function readFile(file){if(!file.type.startsWith('image/')){alert('Please upload an image');return}if(file.size>5*1024*1024){alert('Max 5MB allowed');return}const r = new FileReader();r.onload = ()=>{imgData = r.result; dropZone.innerHTML = '<img style="max-width:100%;max-height:140px;border-radius:8px" src="'+imgData+'"/>'};r.readAsDataURL(file);}

    savePrompt.addEventListener('click', ()=>{
      const text = promptText.value.trim();
      if(!text){alert('Prompt text is required');return}
      const obj = {id:uid(),title:titleInput.value.trim(),text:text,data:imgData||placeholderData(titleInput.value||'Image'),date:nowISO(),fav:false};
      prompts.push(obj); save(prompts); render(); hideModal();
    })

    clearStorage.addEventListener('click', ()=>{if(confirm('Clear all saved prompts?')){localStorage.removeItem(KEY);prompts=[];render()}})

    if(prompts.length===0){
      prompts = [
        {id:uid(),title:'Cyberpunk City',text:"A futuristic city at night, neon reflections, rainy streets, cinematic wide-angle, 8k photorealistic",data:placeholderData('Cyberpunk City'),date:nowISO(),fav:false},
        {id:uid(),title:'Misty Forest',text:"A dreamy misty forest with soft volumetric light, moss-covered rocks, ultra-detailed, 35mm lens",data:placeholderData('Misty Forest'),date:nowISO(),fav:false}
      ];
      save(prompts);
    }

    render();
    backdrop.addEventListener('click',(e)=>{if(e.target===backdrop) hideModal()});
  </script>
</body>
</html>

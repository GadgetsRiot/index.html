# index.html

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#101827">
<title>OpenLens Search</title>
<style>
:root{color-scheme:dark;--bg:#0b1120;--panel:#151f32;--text:#f3f6ff;--muted:#aab7cc;--accent:#76e4c3}
*{box-sizing:border-box}
body{margin:0;background:linear-gradient(150deg,#111c32,#080d17);color:var(--text);font-family:system-ui,-apple-system,sans-serif;min-height:100vh}
header{padding:25px 18px 12px;text-align:center}
.logo{font-size:34px;font-weight:850;letter-spacing:-1.5px}
.logo span{color:var(--accent)}
header p{color:var(--muted);margin:8px auto;max-width:440px}
main{max-width:620px;margin:auto;padding:14px}
.panel{background:var(--panel);border:1px solid #293750;border-radius:20px;padding:20px;margin-bottom:16px}
h2{font-size:19px;margin:0 0 12px}
.upload{display:block;border:2px dashed #536581;border-radius:15px;padding:27px 12px;text-align:center;cursor:pointer}
.upload strong{display:block;font-size:17px;margin-bottom:7px}
small,.muted{color:var(--muted)}
input[type=file]{position:absolute;width:1px;height:1px;opacity:0}
#preview{display:none;width:100%;max-height:340px;object-fit:contain;border-radius:12px;margin-top:15px;background:#080d17}
button,.searchlink{display:block;width:100%;border:0;border-radius:12px;padding:14px;margin-top:10px;font-size:15px;font-weight:700;text-align:center;text-decoration:none;cursor:pointer}
.primary{background:var(--accent);color:#092019}
.secondary,.searchlink{background:#24334b;color:#f3f6ff;border:1px solid #394b67}
button:disabled{opacity:.4;cursor:not-allowed}
#status{font-size:13px;color:var(--muted);margin-top:10px}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:9px}
.grid .searchlink{margin:0}
footer{text-align:center;padding:8px 18px 30px;color:var(--muted);font-size:12px}
</style>
</head>
<body>
<header>
  <div class="logo">Open<span>Lens</span></div>
  <p>One image. More ways to find it online.</p>
</header>
<main>
  <section class="panel">
    <h2>1. Choose an image</h2>
    <label class="upload" for="imageInput">
      <strong>＋ Upload a photo</strong>
      <span class="muted">Tap to select an image from your phone</span>
      <br><small>JPG, PNG, WEBP and other browser-supported images</small>
    </label>
    <input id="imageInput" type="file" accept="image/*">
    <img id="preview" alt="Selected image preview">
    <button id="clearBtn" class="secondary" hidden>Remove image</button>
    <div id="status" aria-live="polite">Your image stays on this page until you choose a search service.</div>
  </section>

  <section class="panel">
    <h2>2. Search the web</h2>
    <p class="muted">Choose a service to look for visually similar images. Some services may require you to upload the image again.</p>
    <a class="searchlink" href="https://lens.google.com/" target="_blank" rel="noopener">Google Lens ↗</a>
    <a class="searchlink" href="https://images.google.com/" target="_blank" rel="noopener">Google Images ↗</a>
    <a class="searchlink" href="https://www.bing.com/visualsearch" target="_blank" rel="noopener">Bing Visual Search ↗</a>
    <a class="searchlink" href="https://tineye.com/" target="_blank" rel="noopener">TinEye ↗</a>
    <p class="muted">These links open external services. OpenLens does not automatically send your photo to them.</p>
  </section>

  <section class="panel">
    <h2>3. What's coming next</h2>
    <p>Planned features:</p>
    <ul class="muted">
      <li>Find duplicate or nearly identical photos</li>
      <li>Compare images using perceptual hashes</li>
      <li>Explore visual similarity matching</li>
      <li>Install OpenLens on your phone</li>
    </ul>
    <p class="muted">This starter version uses free external search links. Local image matching and advanced visual search are not implemented yet.</p>
  </section>
</main>
<footer>OpenLens Search · Free starter project · Built for mobile</footer>
<script>
const input=document.getElementById('imageInput');
const preview=document.getElementById('preview');
const status=document.getElementById('status');
const clearBtn=document.getElementById('clearBtn');
let currentUrl=null;

input.addEventListener('change',()=>{
  const file=input.files && input.files[0];
  if(!file)return;
  if(!file.type.startsWith('image/')){
    status.textContent='Please choose an image file.';
    input.value='';
    return;
  }
  if(currentUrl)URL.revokeObjectURL(currentUrl);
  currentUrl=URL.createObjectURL(file);
  preview.src=currentUrl;
  preview.style.display='block';
  clearBtn.hidden=false;
  status.textContent='Selected: '+file.name+'. Choose a search service above to continue.';
});

clearBtn.addEventListener('click',()=>{
  input.value='';
  preview.removeAttribute('src');
  preview.style.display='none';
  clearBtn.hidden=true;
  if(currentUrl)URL.revokeObjectURL(currentUrl);
  currentUrl=null;
  status.textContent='Choose an image to get started.';
});
</script>
</body>
</html>

<!doctype html>
<html lang="az">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Üzr — Barışaqmı?</title>
<style>
  :root{--bg:#fff8fb;--card:#fff;--text:#111827;--muted:#6b7280;--accent:#6d28d9;--danger:#dc2626;--glass:rgba(255,255,255,0.6)}
  *{box-sizing:border-box}html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial}
  body{display:flex;align-items:center;justify-content:center;padding:18px;background:linear-gradient(180deg,#f7f6ff 0%,#fff8f3 100%);color:var(--text)}
  .wrap{width:100%;max-width:720px;background:var(--card);border-radius:16px;padding:26px;box-shadow:0 12px 40px rgba(20,20,40,0.08);text-align:center}
  .emoji{font-size:44px}
  h1{font-size:22px;margin:10px 0}
  p.lead{color:var(--muted);margin:0 0 16px}
  .row{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin-top:16px}
  button{cursor:pointer;border:0;padding:12px 18px;border-radius:12px;font-weight:700;font-size:16px;box-shadow:0 8px 20px rgba(15,15,30,0.06)}
  .btn-yes{background:var(--accent);color:white;min-width:120px}
  .btn-no{background:#f1f5f9;color:var(--text);min-width:120px}
  .small{font-size:13px;color:var(--muted);margin-top:12px}
  /* modal */
  .modal-backdrop{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(8,10,15,0.45);z-index:60;padding:20px}
  .modal{background:white;border-radius:14px;padding:18px;max-width:560px;width:100%;text-align:center;box-shadow:0 14px 40px rgba(10,12,30,0.18)}
  .modal h2{margin:0 0 6px;font-size:20px}
  .modal p{margin:0 0 12px;color:var(--muted)}
  .modal .row{margin-top:12px}
  .danger{background:var(--danger);color:white}
  .ghost{background:#f3f4f6;color:var(--text)}
  /* fullscreen yes */
  .yes-screen{position:fixed;inset:0;background:linear-gradient(180deg,#5b21b6,#8b5cf6);color:white;display:none;align-items:center;justify-content:center;flex-direction:column;z-index:80;padding:24px}
  .yes-badge{width:96px;height:96px;border-radius:18px;background:rgba(255,255,255,0.12);display:flex;align-items:center;justify-content:center;font-size:44px;margin-bottom:18px}
  .yes-screen h3{margin:6px 0;font-size:26px}
  .yes-screen p{margin:6px 0;color:rgba(255,255,255,0.95)}
  .close-link{display:inline-block;margin-top:14px;padding:10px 14px;border-radius:12px;background:rgba(255,255,255,0.12);color:white;text-decoration:none}
  @media (max-width:420px){ .wrap{padding:18px} button{padding:12px 14px;font-size:15px} .yes-badge{width:76px;height:76px;font-size:34px} }
</style>
</head>
<body>
  <div class="wrap" role="main" aria-labelledby="title">
    <div class="emoji">💌</div>
    <h1 id="title">Səni incitdiyim üçün üzr istəyirəm</h1>
    <p class="lead">Bağışla, barışaqmı?</p>
    <div class="row">
      <button id="yes" class="btn-yes" aria-label="Hə düyməsi">Hə</button>
      <button id="no" class="btn-no" aria-label="Yox düyməsi">Yox</button>
    </div>
    <p class="small">Əgər in-app (WhatsApp) brauzerdə problem yaranarsa, "Open in Browser" seçərək tam brauzerdə aç.</p>
  </div>

  <div id="backdrop" class="modal-backdrop" aria-hidden="true">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <h2 id="modalTitle">Əminsən??</h2>
      <p id="modalText">Bir daha fikirləş, canım.</p>
      <div class="row">
        <button id="again" class="danger">Yenə əminsən</button>
        <button id="back" class="ghost">Geri</button>
      </div>
      <a id="close" class="close-link" href="#" style="background:transparent;color:var(--muted);text-decoration:none">Bağla</a>
    </div>
  </div>

  <div id="yesScreen" class="yes-screen" aria-hidden="true">
    <div class="yes-badge">💙</div>
    <h3>Məni bağışlayacağıvı bilirdim!</h3>
    <div style="font-size:64px;margin:8px 0">🐻🤗🐻</div>
    <p>Səni çoox sevirəm! 💖</p>
    <a id="backFromYes" class="close-link" href="#" role="button">Geri</a>
  </div>

<script>
(function(){
  const steps = [
    {title: "Əminsən??", text: "Bir daha fikirləş, canım. Bəlkə “Hə” deyək? 💙"},
    {title: "Hələ də əminsən?", text: "Mən səni incitmək istəmirdim… Bir fürsət verərsən? 🥺"},
    {title: "Axı qəlbim səninlədir…", text: "Bir “Hə” desən, hər şeyi daha gözəl edəcəm. ✨"},
    {title: "Bir az da fikirləşək?", text: "Sevgi qalib gəlsin deyə səninlə danışıram. 💫"},
    {title: "Son qərarın budur?", text: "Bir qucaqlayım səni, sonra deyərsən… 🤗"},
    {title: "Bir az da səbr et", text: "Məni dinlə: səhvimi qəbul edirəm və düzəldəcəm. 🌷"},
    {title: "Ürəyinin səsi nə deyir?", text: "Mənim səsim: “Onu çox sevirəm.” 💖"},
    {title: "Qırmayaq bu bağımızı", text: "Sən mənim ən dəyərli insansıñ. 🌙"},
    {title: "Bir “Hə” möcüzə yaradar", text: "Gəl yenidən başlayaq, daha diqqətli olacam. 🌟"},
    {title: "Son dəfə soruşuram…", text: "Həyatım, barışaq? Məni xoşbəxt edəcəksən. 🫶"}
  ];

  const yesBtn = document.getElementById('yes');
  const noBtn = document.getElementById('no');
  const backdrop = document.getElementById('backdrop');
  const modalTitle = document.getElementById('modalTitle');
  const modalText = document.getElementById('modalText');
  const againBtn = document.getElementById('again');
  const backBtn = document.getElementById('back');
  const closeLink = document.getElementById('close');
  const yesScreen = document.getElementById('yesScreen');
  const backFromYes = document.getElementById('backFromYes');

  let idx = 0;

  function openModal(){
    backdrop.style.display = 'flex';
    backdrop.setAttribute('aria-hidden', 'false');
    renderModal();
    document.body.style.overflow = 'hidden';
  }

  function closeModal(){
    backdrop.style.display = 'none';
    backdrop.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
  }

  function renderModal(){
    const s = steps[Math.min(idx, steps.length-1)];
    modalTitle.textContent = s.title;
    modalText.textContent = s.text;
    if(idx >= steps.length - 1){
      againBtn.textContent = 'Yaxşı, barışaq';
      againBtn.classList.remove('danger');
      againBtn.style.background = 'linear-gradient(90deg,#10b981,#06b6d4)';
      againBtn.style.color = '#fff';
    } else {
      againBtn.textContent = 'Yenə əminsən';
      againBtn.classList.add('danger');
      againBtn.style.background = '';
      againBtn.style.color = '';
    }
  }

  againBtn.addEventListener('click', function(e){
    e.preventDefault();
    if(idx >= steps.length - 1){
      closeModal();
      showYesScreen();
      return;
    }
    idx++;
    renderModal();
  });

  noBtn.addEventListener('click', function(e){
    e.preventDefault();
    openModal();
  });

  backBtn.addEventListener('click', function(e){ e.preventDefault(); closeModal(); });
  closeLink.addEventListener('click', function(e){ e.preventDefault(); closeModal(); });
  backdrop.addEventListener('click', function(e){ if(e.target===backdrop) closeModal(); });
  yesBtn.addEventListener('click', function(e){ e.preventDefault(); showYesScreen(); });

  function showYesScreen(){
    yesScreen.style.display = 'flex';
    yesScreen.setAttribute('aria-hidden','false');
    document.querySelector('.wrap').style.visibility = 'hidden';
    idx = 0;
    renderModal();
  }

  backFromYes.addEventListener('click', function(e){
    e.preventDefault();
    yesScreen.style.display = 'none';
    yesScreen.setAttribute('aria-hidden','true');
    document.querySelector('.wrap').style.visibility = 'visible';
    idx = 0;
    renderModal();
  });

})();
</script>
</body>
</html>

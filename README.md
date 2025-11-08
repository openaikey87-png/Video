
<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>กล่องของขวัญโมเมนต์รัก (พูดไทย)</title>
<style>
  :root{ --size:360px; }
  body{
    margin:0;
    height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    background: #f3f4f6;
    font-family: "Noto Sans Thai", system-ui, sans-serif;
  }
  .card{
    width:var(--size);
    max-width:90vw;
    text-align:center;
    background:#fff;
    border-radius:14px;
    box-shadow:0 12px 30px rgba(16,24,40,0.08);
    padding:18px;
    position:relative;
  }
  img{
    width:100%;
    border-radius:10px;
    display:block;
    margin:0 auto 12px;
    animation:pose 4s ease-in-out infinite;
    transform-origin:center bottom;
  }
  @keyframes pose{
    0%{ transform: translateY(0) rotate(0deg); }
    25%{ transform: translateY(-6px) rotate(1deg); }
    50%{ transform: translateY(0) rotate(0deg); }
    75%{ transform: translateY(-6px) rotate(-1deg); }
    100%{ transform: translateY(0) rotate(0deg); }
  }
  .controls{
    display:flex;
    gap:8px;
    justify-content:center;
    align-items:center;
  }
  button{
    padding:8px 12px;
    border-radius:10px;
    border:none;
    cursor:pointer;
    font-weight:600;
  }
  .play{
    background:#0f172a;
    color:#fff;
  }
  .retry{
    background:#eef2ff;
    color:#3730a3;
  }
  .hint{
    font-size:13px;
    color:#6b7280;
    margin-top:8px;
  }
  .badge{
    position:absolute;
    top:12px;
    left:12px;
    background:#fff7ed;
    color:#b45309;
    padding:6px 10px;
    border-radius:999px;
    font-weight:700;
    font-size:13px;
  }
</style>
</head>
<body>
  <div class="card">
    <div class="badge">กล่องของขวัญโมเมนต์รัก</div>

    <!-- ✅ เปลี่ยนตรงนี้เป็นรูปของคุณ -->
    <img src="your-image.jpg" alt="ภาพคู่รักและของขวัญ" />

    <div class="controls">
      <button id="btnPlay" class="play">เล่นเสียง</button>
      <button id="btnRetry" class="retry">ลองอีกครั้ง</button>
    </div>
    <div class="hint" id="status">แตะหน้าจอ 1 ครั้งเพื่ออนุญาตเสียง</div>
  </div>

<script>
(function(){

  const text = `
ผู้ชาย:
ที่รัก... ผมมีของขวัญมาให้

ผู้หญิง:
หื้ม ของขวัญเหรอ

ผู้ชาย:
อือ ลองเปิดดูสิ

ผู้หญิง:
แล้วที่รักว่า หนู ดี ไหม

ผู้ชาย:
ดีสิ ดีที่สุดสำหรับผมเลย

ผู้หญิง:
แค่นั้นแหละ หนูชอบแบบนี้

ใครที่ยังไม่มีกล่องของขวัญให้แฟนในปีใหม่นี้
ของขวัญนี้... ได้โมเมนต์แบบนี้เลย
  `;

  const btnPlay = document.getElementById("btnPlay");
  const btnRetry = document.getElementById("btnRetry");
  const statusEl = document.getElementById("status");

  let thaiVoice = null;

  function loadVoices(){
    const voices = speechSynthesis.getVoices();
    thaiVoice =
      voices.find(v => v.lang === "th-TH") ||
      voices.find(v => v.lang.toLowerCase().includes("th")) ||
      null;

    if(thaiVoice){
      statusEl.textContent = "พร้อมเล่นเสียงไทย ✅";
    } else {
      statusEl.textContent = "⚠ อุปกรณ์นี้ยังไม่มีเสียงไทย — แนะนำเปิดใน Chrome บนมือถือ";
    }
  }

  speechSynthesis.onvoiceschanged = loadVoices;
  loadVoices();

  function speak(){
    if(!window.speechSynthesis) return;
    const utter = new SpeechSynthesisUtterance(text);
    utter.lang = "th-TH";
    utter.rate = 1;
    utter.pitch = 1;
    utter.volume = 1;
    if(thaiVoice) utter.voice = thaiVoice;
    speechSynthesis.cancel();
    speechSynthesis.speak(utter);
  }

  btnPlay.onclick = speak;
  btnRetry.onclick = () => { speechSynthesis.cancel(); speak(); };

  window.addEventListener("click", speak, { once:true });

})();
</script>

</body>
</html>

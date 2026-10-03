# index.html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>校园社团文化节 · 摊位申请</title>
<style>
  :root{
    --primary:#4f46e5;
    --primary-light:#eef2ff;
    --text:#1e293b;
    --muted:#64748b;
    --border:#e2e8f0;
  }

  *{ margin:0; padding:0; box-sizing:border-box; }

  html,body{ height:100%; }

  body{
    font-family:"PingFang SC","Microsoft YaHei",system-ui,-apple-system,sans-serif;
    background:radial-gradient(circle at 25% 15%, #eef2ff, #f8fafc 55%, #f1f5f9);
    color:var(--text);
    overflow:hidden;              /* 横屏整屏，不滚动 */
    -webkit-font-smoothing:antialiased;
  }

  /* 背景装饰光斑 */
  body::before,body::after{
    content:''; position:fixed; border-radius:50%;
    filter:blur(80px); opacity:.5; z-index:0; pointer-events:none;
  }
  body::before{ width:460px; height:460px; background:#c7d2fe; top:-160px; left:-120px; }
  body::after{ width:420px; height:420px; background:#fbcfe8; bottom:-170px; right:-110px; }

  /* ============ 主界面 ============ */
  .page{
    position:relative; z-index:1;
    height:100vh;
    display:flex; flex-direction:column;
    align-items:center; justify-content:center;
    gap:12px;
    padding-right:100px;          /* 给右侧按钮留位置 */
    user-select:none;
  }
  .page h1{
    font-size:clamp(30px, 3.6vw, 56px);
    font-weight:800; letter-spacing:6px; color:#312e81;
    text-shadow:0 6px 22px rgba(99,102,241,.18);
  }
  .page-sub{
    font-size:clamp(14px,1.1vw,18px);
    color:#6366f1; letter-spacing:10px; margin-left:10px;
  }
  .page-tip{
    margin-top:30px; font-size:14px; color:#94a3b8; letter-spacing:1px;
  }

  /* ============ 右侧「申请摊位」标识 ============ */
  .apply-btn{
    position:fixed;
    right:0; top:50%;
    transform:translateY(-50%);
    z-index:10;
    writing-mode:vertical-rl;
    padding:34px 18px;
    font-size:19px; font-weight:700; letter-spacing:8px;
    color:#fff;
    background:linear-gradient(160deg,#7c5cff,#4f46e5 70%);
    border:none; cursor:pointer;
    border-radius:18px 0 0 18px;
    box-shadow:-8px 10px 26px rgba(79,70,229,.38);
    transition:padding .25s ease, box-shadow .25s ease, filter .25s ease;
  }
  .apply-btn:hover{
    padding:34px 26px;
    filter:brightness(1.06);
    box-shadow:-10px 12px 32px rgba(79,70,229,.5);
  }
  .apply-btn:active{ filter:brightness(.95); }
  .apply-btn::after{
    content:''; position:absolute; inset:0; border-radius:inherit;
    animation:pulse 2.2s infinite;
  }
  @keyframes pulse{
    0%   { box-shadow:0 0 0 0 rgba(99,102,241,.55); }
    70%  { box-shadow:0 0 0 16px rgba(99,102,241,0); }
    100% { box-shadow:0 0 0 0 rgba(99,102,241,0); }
  }

  /* ============ 遮罩 & 弹窗 ============ */
  .overlay{
    position:fixed; inset:0; z-index:100;
    background:rgba(15,23,42,.55);
    backdrop-filter:blur(5px);
    display:flex; align-items:center; justify-content:center;
    opacity:0; pointer-events:none;
    transition:opacity .3s ease;
  }
  .overlay.show{ opacity:1; pointer-events:auto; }

  .overlay-close{
    position:fixed; top:22px; right:26px;
    width:40px; height:40px; border-radius:50%;
    border:none; cursor:pointer;
    background:rgba(255,255,255,.18);
    color:#fff; font-size:18px; line-height:1;
    transition:background .2s, transform .2s;
  }
  .overlay-close:hover{ background:rgba(255,255,255,.32); transform:rotate(90deg); }

  .modal{
    width:min(780px, 88vw);
    max-height:84vh;
    overflow-y:auto;
    background:#fff;
    border-radius:22px;
    padding:32px 38px;
    box-shadow:0 30px 70px rgba(15,23,42,.4);
    transform:translateY(18px) scale(.97);
    transition:transform .32s cubic-bezier(.2,.8,.3,1);
  }
  .overlay.show .modal{ transform:none; }
  .modal::-webkit-scrollbar{ width:8px; }
  .modal::-webkit-scrollbar-thumb{ background:#cbd5e1; border-radius:4px; }
  .modal::-webkit-scrollbar-track{ background:transparent; }

  /* ============ 步骤切换 ============ */
  .step{ display:none; }
  .step.active{ display:block; animation:fadeUp .3s ease; }
  @keyframes fadeUp{
    from{ opacity:0; transform:translateY(10px); }
    to  { opacity:1; transform:none; }
  }

  h2{ font-size:21px; font-weight:700; margin-bottom:6px; }
  .sub{ font-size:13px; color:var(--muted); margin-bottom:18px; }

  /* 阅读框 */
  .read-box{
    background:#f8fafc;
    border:1px solid var(--border);
    border-left:4px solid var(--primary);
    border-radius:12px;
    padding:18px 22px;
    max-height:36vh;
    overflow-y:auto;
    line-height:2;
  }
  .read-box p{ font-size:14px; color:#475569; }
  .read-box p + p{ margin-top:4px; }
  .read-box::-webkit-scrollbar{ width:6px; }
  .read-box::-webkit-scrollbar-thumb{ background:#cbd5e1; border-radius:3px; }

  /* 底部操作行 */
  .footer-row{
    display:flex; align-items:center; justify-content:space-between;
    gap:16px;
    margin-top:22px;
    padding-top:18px;
    border-top:1px dashed var(--border);
  }
  .footer-row.end{ justify-content:flex-end; }

  .countdown{ font-size:14px; color:var(--muted); }
  .countdown b{
    font-size:21px; color:#ef4444; margin:0 3px;
    font-variant-numeric:tabular-nums;
  }
  .countdown.done{ color:#10b981; font-weight:600; }

  /* 按钮 */
  .btn{
    padding:12px 36px;
    border:none; border-radius:999px;
    font-size:15px; font-weight:600;
    font-family:inherit; cursor:pointer;
    transition:transform .2s, box-shadow .2s, filter .2s;
  }
  .btn.primary{
    color:#fff;
    background:linear-gradient(135deg,#6366f1,#4f46e5);
    box-shadow:0 10px 22px rgba(79,70,229,.35);
    animation:fadeUp .3s ease;
  }
  .btn.primary:hover{
    transform:translateY(-2px);
    box-shadow:0 14px 28px rgba(79,70,229,.45);
  }
  .btn.primary:active{ transform:translateY(0); filter:brightness(.96); }

  .hidden{ display:none !important; }

  /* 勾选同意 */
  .agree{
    display:flex; align-items:center; gap:12px;
    margin-top:20px; padding:16px 18px;
    background:#f8fafc;
    border:1.5px solid var(--border);
    border-radius:14px;
    cursor:pointer; user-select:none;
    transition:border-color .2s, background .2s;
  }
  .agree:hover{ border-color:#c7d2fe; background:#f5f3ff; }
  .agree input{
    width:19px; height:19px; flex:none;
    accent-color:var(--primary); cursor:pointer;
  }
  .agree span{ font-size:14px; color:#334155; }

  /* 表单区 */
  .section{ margin-bottom:24px; }
  .label{
    font-size:14px; font-weight:600; color:#334155;
    margin-bottom:12px;
  }
  .label em{ font-style:normal; font-weight:400; font-size:12px; color:var(--muted); }

  .exhibits{
    display:grid;
    grid-template-columns:repeat(auto-fill, minmax(150px,1fr));
    gap:10px;
  }
  .exhibit-item{
    display:flex; align-items:center; gap:9px;
    padding:11px 14px;
    border:1.5px solid var(--border);
    border-radius:12px;
    background:#fff;
    cursor:pointer;
    transition:.18s;
  }
  .exhibit-item:hover{ border-color:#c7d2fe; background:#fafaff; }
  .exhibit-item input{
    width:16px; height:16px; flex:none;
    accent-color:var(--primary); cursor:pointer;
  }
  .exhibit-item span{ font-size:14px; color:#475569; }
  .exhibit-item.active{
    border-color:var(--primary);
    background:var(--primary-light);
    box-shadow:0 4px 12px rgba(99,102,241,.15);
  }
  .exhibit-item.active span{ color:#4338ca; font-weight:600; }

  .text-input{
    width:100%;
    padding:13px 18px;
    border:1.5px solid var(--border);
    border-radius:12px;
    font-size:15px; font-family:inherit; color:var(--text);
    outline:none;
    transition:.2s;
  }
  .text-input::placeholder{ color:#b6c0cf; }
  .text-input:focus{
    border-color:#818cf8;
    box-shadow:0 0 0 4px rgba(99,102,241,.13);
  }

  .zones{
    display:flex; flex-wrap:wrap; gap:9px;
  }
  .zone-btn{
    width:40px; height:40px;
    border:1.5px solid var(--border);
    border-radius:11px;
    background:#fff;
    font-size:15px; font-weight:600; color:#475569;
    font-family:inherit; text-transform:lowercase;
    cursor:pointer;
    transition:.18s;
  }
  .zone-btn:hover{
    border-color:#a5b4fc; color:var(--primary);
    transform:translateY(-2px);
  }
  .zone-btn.active{
    color:#fff; border-color:transparent;
    background:linear-gradient(135deg,#6366f1,#4f46e5);
    box-shadow:0 8px 16px rgba(79,70,229,.35);
    transform:translateY(-2px);
  }

  .hint{
    flex:1;
    font-size:13px; color:#f59e0b;
  }

  /* 成功页 */
  .success-wrap{ text-align:center; padding:14px 0 6px; }
  .success-icon{
    width:78px; height:78px; margin:0 auto 20px;
    border-radius:50%;
    background:linear-gradient(135deg,#34d399,#10b981);
    color:#fff; font-size:40px; line-height:78px;
    box-shadow:0 14px 30px rgba(16,185,129,.35);
    animation:pop .5s cubic-bezier(.34,1.56,.64,1);
  }
  @keyframes pop{
    from{ transform:scale(0); opacity:0; }
    to  { transform:scale(1); opacity:1; }
  }
  .summary{
    text-align:left;
    background:#f8fafc;
    border:1px solid var(--border);
    border-radius:14px;
    padding:18px 22px;
    margin:20px 0 26px;
    font-size:14px; line-height:2.1; color:#334155;
    word-break:break-all;
  }
  .summary b{ color:#4338ca; }
</style>
</head>
<body>

  <!-- ========== 主界面 ========== -->
  <div class="page">
    <h1>校园社团文化节</h1>
    <p class="page-sub">摊位申请系统</p>
    <p class="page-tip">→ 点击右侧「申请摊位」标识，开始申请流程</p>
  </div>

  <button class="apply-btn" id="applyBtn">申请摊位</button>

  <!-- ========== 弹窗 ========== -->
  <div class="overlay" id="overlay">
    <button class="overlay-close" id="closeBtn">✕</button>

    <div class="modal" id="modal">

      <!-- 步骤一：阅读提示 + 9 秒倒计时 -->
      <section class="step" id="stepRead">
        <h2>阅读提示</h2>
        <p class="sub">请仔细阅读以下内容，倒计时结束后方可进入下一步</p>
        <div class="read-box">
          <p>1. 本次摊位申请仅限「校园社团文化节」活动期间使用。</p>
          <p>2. 申请人须保证所售商品为原创或已获得合法授权，严禁售卖违规物品。</p>
          <p>3. 摊位位置由主办方统一分配，申请成功后不可随意转让或转租。</p>
          <p>4. 请自觉保持摊位周边卫生，活动结束后自行清理垃圾。</p>
          <p>5. 摊位用电需提前报备，禁止私拉电线及使用大功率电器。</p>
          <p>6. 如遇不可抗力因素导致活动变动，主办方将另行通知。</p>
        </div>
        <div class="footer-row">
          <div class="countdown" id="countdownText"></div>
          <button class="btn primary hidden" id="btnStep1Next">下一步</button>
        </div>
      </section>

      <!-- 步骤二：注意事项 + 勾选同意 -->
      <section class="step" id="stepNotice">
        <h2>申摊注意事项</h2>
        <p class="sub">请确认无误后勾选同意，方可继续</p>
        <div class="read-box">
          <p>1. 每个摊位最多可陈列 10 件展品，且至少选择 1 件。</p>
          <p>2. 摊位名称须健康积极，不得含有违法违规或侵权内容。</p>
          <p>3. 专区字母仅作为意向登记，最终摊位位置以主办方分配为准。</p>
          <p>4. 申请信息提交后不可修改，请务必确认填写无误。</p>
          <p>5. 活动当天需提前 30 分钟到场布置摊位，迟到超过 30 分钟视为放弃。</p>
        </div>

        <label class="agree">
          <input type="checkbox" id="agreeCheck">
          <span>我已认真阅读并同意以上全部注意事项</span>
        </label>

        <div class="footer-row end">
          <button class="btn primary hidden" id="btnStep2Next">下一步</button>
        </div>
      </section>

      <!-- 步骤三：填写申摊信息 -->
      <section class="step" id="stepForm">
        <h2>填写申摊信息</h2>
        <p class="sub">请完成以下三项信息填写</p>

        <div class="section">
          <div class="label">选择展品 <em>（至少选择 1 件）</em></div>
          <div class="exhibits" id="exhibitList"></div>
        </div>

        <div class="section">
          <div class="label">摊位名称</div>
          <input type="text" class="text-input" id="stallName" placeholder="请输入摊位名称（如：星野手作小铺）" maxlength="20">
        </div>

        <div class="section">
          <div class="label">选择专区 <em>（a ~ z，任选其一）</em></div>
          <div class="zones" id="zoneList"></div>
        </div>

        <div class="footer-row end">
          <div class="hint" id="hint"></div>
          <button class="btn primary hidden" id="btnSubmit">申请摊位</button>
        </div>
      </section>

      <!-- 步骤四：申请成功 -->
      <section class="step" id="stepSuccess">
        <div class="success-wrap">
          <div class="success-icon">✓</div>
          <h2>申请成功！</h2>
          <p class="sub">主办方将在 3 个工作日内与你联系，请留意通知</p>
          <div class="summary" id="summary"></div>
          <button class="btn primary" id="btnFinish">完成</button>
        </div>
      </section>

    </div>
  </div>

<script>
(function () {
  'use strict';

  /* ---------------- 数据 ---------------- */
  const EXHIBITS = [
    '手作饰品', '原创插画', '毛绒玩偶', '手账文具', '陶艺小物',
    '香薰蜡烛', '复古胶片', '桌面绿植', '扭蛋盲盒', '自制徽章'
  ];
  const ZONES = 'abcdefghijklmnopqrstuvwxyz'.split('');

  /* ---------------- 元素 ---------------- */
  const overlay    = document.getElementById('overlay');
  const modal      = document.getElementById('modal');
  const applyBtn   = document.getElementById('applyBtn');
  const closeBtn   = document.getElementById('closeBtn');

  const steps = {
    read:    document.getElementById('stepRead'),
    notice:  document.getElementById('stepNotice'),
    form:    document.getElementById('stepForm'),
    success: document.getElementById('stepSuccess')
  };

  const countdownText = document.getElementById('countdownText');
  const btnStep1Next  = document.getElementById('btnStep1Next');
  const agreeCheck    = document.getElementById('agreeCheck');
  const btnStep2Next  = document.getElementById('btnStep2Next');
  const exhibitList   = document.getElementById('exhibitList');
  const zoneList      = document.getElementById('zoneList');
  const stallName     = document.getElementById('stallName');
  const btnSubmit     = document.getElementById('btnSubmit');
  const hint          = document.getElementById('hint');
  const summary       = document.getElementById('summary');
  const btnFinish     = document.getElementById('btnFinish');

  let countdownTimer = null;
  let selectedZone   = null;

  /* ---------------- 初始化展品 / 专区 ---------------- */
  EXHIBITS.forEach(function (name) {
    const label = document.createElement('label');
    label.className = 'exhibit-item';
    label.innerHTML = '<input type="checkbox" value="' + name + '"><span>' + name + '</span>';
    exhibitList.appendChild(label);
  });

  ZONES.forEach(function (z) {
    const btn = document.createElement('button');
    btn.type = 'button';
    btn.className = 'zone-btn';
    btn.textContent = z;
    btn.addEventListener('click', function () {
      selectedZone = z;
      zoneList.querySelectorAll('.zone-btn').forEach(function (b) {
        b.classList.remove('active');
      });
      btn.classList.add('active');
      updateState();
    });
    zoneList.appendChild(btn);
  });

  /* ---------------- 事件绑定 ---------------- */
  exhibitList.addEventListener('change', function (e) {
    const cb = e.target;
    if (cb.matches('input[type="checkbox"]')) {
      cb.closest('.exhibit-item').classList.toggle('active', cb.checked);
      updateState();
    }
  });

  stallName.addEventListener('input', updateState);

  agreeCheck.addEventListener('change', function () {
    btnStep2Next.classList.toggle('hidden', !agreeCheck.checked);
  });

  applyBtn.addEventListener('click', openFlow);
  closeBtn.addEventListener('click', closeFlow);
  btnStep1Next.addEventListener('click', function () { goStep('notice'); });
  btnStep2Next.addEventListener('click', function () { goStep('form'); });
  btnSubmit.addEventListener('click', submit);
  btnFinish.addEventListener('click', closeFlow);

  /* ---------------- 流程控制 ---------------- */
  function openFlow() {
    resetAll();
    overlay.classList.add('show');
    goStep('read');
    startCountdown();
  }

  function closeFlow() {
    clearInterval(countdownTimer);
    overlay.classList.remove('show');
  }

  function goStep(key) {
    Object.keys(steps).forEach(function (k) {
      steps[k].classList.toggle('active', k === key);
    });
    modal.scrollTop = 0;
  }

  /* ---------------- 9 秒强制阅读倒计时 ---------------- */
  function startCountdown() {
    clearInterval(countdownTimer);
    let left = 9;

    countdownText.classList.remove('done');
    countdownText.innerHTML = '请仔细阅读，剩余 <b>' + left + '</b> 秒';
    btnStep1Next.classList.add('hidden');

    countdownTimer = setInterval(function () {
      left -= 1;
      if (left > 0) {
        countdownText.innerHTML = '请仔细阅读，剩余 <b>' + left + '</b> 秒';
      } else {
        clearInterval(countdownTimer);
        countdownText.classList.add('done');
        countdownText.textContent = '阅读完成，可以进入下一步';
        btnStep1Next.classList.remove('hidden');
      }
    }, 1000);
  }

  /* ---------------- 表单校验：全部填完才出现「申请摊位」 ---------------- */
  function updateState() {
    const picked = exhibitList.querySelectorAll('input:checked').length;
    const missing = [];

    if (picked === 0) missing.push('至少选择 1 件展品');
    if (stallName.value.trim() === '') missing.push('填写摊位名称');
    if (!selectedZone) missing.push('选择专区');

    btnSubmit.classList.toggle('hidden', missing.length > 0);
    hint.textContent = missing.length ? '还需：' + missing.join('、') : '';
  }

  /* ---------------- 提交申请 ---------------- */
  function submit() {
    const picks = Array.prototype.slice
      .call(exhibitList.querySelectorAll('input:checked'))
      .map(function (i) { return i.value; });

    if (picks.length === 0 || stallName.value.trim() === '' || !selectedZone) return;

    summary.innerHTML =
      '<div><b>摊位名称：</b>' + escapeHtml(stallName.value.trim()) + '</div>' +
      '<div><b>所属专区：</b>' + selectedZone + ' 区</div>' +
      '<div><b>参展展品：</b>' + picks.map(escapeHtml).join('、') +
      '（共 ' + picks.length + ' 件）</div>';

    goStep('success');
  }

  /* ---------------- 重置 ---------------- */
  function resetAll() {
    clearInterval(countdownTimer);

    exhibitList.querySelectorAll('input:checked').forEach(function (cb) {
      cb.checked = false;
      cb.closest('.exhibit-item').classList.remove('active');
    });

    stallName.value = '';
    selectedZone = null;
    zoneList.querySelectorAll('.zone-btn').forEach(function (b) {
      b.classList.remove('active');
    });

    agreeCheck.checked = false;
    btnStep1Next.classList.add('hidden');
    btnStep2Next.classList.add('hidden');
    btnSubmit.classList.add('hidden');
    hint.textContent = '';
    summary.innerHTML = '';

    countdownText.classList.remove('done');
    countdownText.textContent = '';
  }

  /* ---------------- 工具 ---------------- */
  function escapeHtml(str) {
    return String(str).replace(/[&<>"']/g, function (m) {
      return { '&':'&amp;', '<':'&lt;', '>':'&gt;', '"':'&quot;', "'":'&#39;' }[m];
    });
  }
})();
</script>
</body>
</html>

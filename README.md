  <!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>生日快乐！</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      -webkit-user-select: none;
    }

    body {
      width: 100vw;
      height: 100vh;
      overflow: hidden;
      background-color: #0b0c1b;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", sans-serif;
      position: relative;
    }

    /* 弹窗容器 */
    #windows-container {
      position: absolute;
      inset: 0;
      pointer-events: none;
      z-index: 10;
      transition: opacity 1.2s ease, transform 1.2s ease;
    }

    /* 拟真 Windows 提示弹窗 */
    .win-modal {
      position: absolute;
      width: 125px;
      height: 75px;
      border-radius: 6px;
      box-shadow: 0 6px 18px rgba(0, 0, 0, 0.4);
      border: 1px solid rgba(255, 255, 255, 0.55);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      transition: left 1.5s cubic-bezier(0.25, 1, 0.5, 1), 
                  top 1.5s cubic-bezier(0.25, 1, 0.5, 1), 
                  transform 1.5s cubic-bezier(0.25, 1, 0.5, 1),
                  opacity 0.8s ease;
    }

    /* 窗口标题栏 */
    .win-header {
      height: 20px;
      background: rgba(255, 255, 255, 0.6);
      backdrop-filter: blur(4px);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 5px;
      font-size: 9px;
      color: #444;
      font-weight: 500;
    }

    .win-dots {
      display: flex;
      gap: 3px;
    }

    .win-dot {
      width: 5px;
      height: 5px;
      border-radius: 50%;
      background: #aaa;
    }

    /* 窗口内容文字 */
    .win-body {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11.5px;
      font-weight: 700;
      color: #2c3e50;
      padding: 3px 6px;
      text-align: center;
      line-height: 1.25;
      letter-spacing: 0.3px;
    }

    /* 阶段二：浩瀚星空与3D冷粉蓝粒子蛋糕舞台 */
    #cake-stage {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      z-index: 20;
      opacity: 0;
      pointer-events: none;
      transition: opacity 1.8s ease;
      background: radial-gradie
t(circle at center, #0d0f22 0%, #030308 100%);
    }

    #cake-stage.show {
      opacity: 1;
      pointer-events: auto;
    }

    /* 3D 旋转粒子画布 */
    #cakeCanvas {
      position: absolute;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 21;
    }

     /* 冷光蓝粉流光标题（上移，紧贴蛋糕下方） */
    .hbd-title {
      position: absolute;
      bottom: 27%; /* 原来是 15%，向上抬高 */
      font-size: 28px;
      font-weight: 300;
      letter-spacing: 5px;
      background: linear-gradient(135deg, #dff9fb 0%, #fbc2eb 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-shadow: 0 0 25px rgba(251, 194, 235, 0.5);
      z-index: 22;
      opacity: 0;
      transform: translateY(18px);
      transition: opacity 2s ease 0.8s, transform 2s ease 0.8s;
      pointer-events: none;
    }

    #cake-stage.show .hbd-title {
      opacity: 1;
      transform: translateY(0);
    }

    /* 吹蜡烛许愿按钮（向上抬高） */
    .wish-btn {
      position: absolute;
      bottom: 18%; /* 原来是 7%，向上抬高 */
      padding: 12px 30px;
      font-size: 15px;
      font-weight: 500;
      letter-spacing: 2px;
      color: #fff;
      background: rgba(255, 255, 255, 0.12);
      border: 1px solid rgba(255, 255, 255, 0.4);
      border-radius: 30px;
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      box-shadow: 0 4px 20px rgba(116, 185, 255, 0.35);
      cursor: pointer;
      z-index: 60;
      opacity: 0;
      transform: translateY(15px);
      transition: all 1.8s ease 1.2s;
    }

    #cake-stage.show .wish-btn {
      opacity: 1;
      transform: translateY(0);
    }

    .wish-btn:active {
      transform: scale(0.95);
      background: rgba(255, 255, 255, 0.25);
    }

    /* 愿望升空动效浮层（移到蛋糕正上方 + 暖金星光高亮） */
    .wish-toast {
      position: absolute;
      top: 18%; /* 移到屏幕上方偏居中位置，避开蛋糕中心 */
      left: 50%;
      transform: translate(-50%, -50%) scale(0.8);
      text-align: center;
      opacity: 0;
      pointer-events: none;
      z-index: 70;
      padding: 14px 28px;
      background: rgba(10, 15, 35, 0.65); /* 微暗半透明玻璃背景，彻底隔绝背景光斑 */
      border-radius: 20px;
      border: 1px solid rgba(255, 234, 167, 0.3);
      backdrop-filter: blur(6px);
      -webkit-backdrop-filter: blur(6px);
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.45);
      transition: all 1.2s cubic-bezier(0.18, 0.89, 0.32, 1.28);
    }

    .wish-toast.show {
      opacity: 1;
      transform: translate(-50%, -50%) scale(1);
    }

    /* 愿望主标题：星光香槟金，与蛋糕的蓝粉冷色形成高对比 */
    .wish-main {
      font-size: 24px;
      font-weight: 500;
      letter-spacing: 4px;
      color: #ffeaa7;
      text-shadow: 0 0 15px rgba(255, 234, 167, 0.8), 0 2px 8px rgba(0, 0, 0, 0.8);
      margin-bottom: 6px;
    }

    .wish-sub {
      font-size: 13px;
      letter-spacing: 2px;
      color: #fff;
      text-shadow: 0 0 10px rgba(255, 255, 255, 0.6), 0 2px 6px rgba(0, 0, 0, 0.8);
    }

    /* 音乐唱片按钮样式 */
    .music-disc {
      position: fixed;
      top: 18px;
      right: 18px;
      width: 48px;
      height: 48px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.25);
      border: 1px solid rgba(255, 255, 255, 0.5);
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 26px;
      z-index: 999;
      cursor: pointer;
      box-shadow: 0 4px 14px rgba(0,0,0,0.35);
      animation: spinDisc 4s linear infinite;
      animation-play-state: paused;
    }

    .music-disc.playing {
      animation-play-state: running;
    }

    @keyframes spinDisc {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
  </style>
</head>
<body>

  <!-- 右上角音乐控制旋转图标 -->
  <div id="music-btn" class="music-disc" onclick="toggleMusic(event)">🎵</div>

  <!-- 满屏弹窗及爱心汇聚容器 -->
  <div id="windows-container"></div>

  <!-- 阶段二：浩瀚星空与3D粒子自转蛋糕及许愿交互 -->
  <div id="cake-stage">
    <canvas id="cakeCanvas"></canvas>
    <div class="hbd-title">Happy Birthday,Cillian!</div>
    <button id="wish-btn" class="wish-btn" onclick="blowCandleAndMakeWish()">🕯️ 吹灭蜡烛许个愿</button>

    <div id="wish-toast" class="wish-toast">
      <div class="wish-main">愿望已抵达星空 ✨</div>
      <div class="wish-sub">祝宝宝健康平安，一切顺遂 💫</div>
    </div>
  </div>

  <script>
    // ================= 自定义配置 =================
    const MESSAGES = [
      "身心舒适", "不困于过去", "宝宝一切顺利 ✨", "永远有退路",
      "照顾好自己 🌸", "永远被坚定选择", "天天开心 🥳", "不被遗憾困住",
      "幸运常相伴 🍀", "不必事事赢", "可以享受生活", "不要那么累",
      "永远有人懂你", "睡个好觉 🌙", "永远有新的可能", "不要焦虑 ☁️",
      "我会以你的方式爱你", "少失眠 💤", "保持好奇心", "心想事成 🌟",
      "被爱包围", "不再苛责自己", "我们永远在一起 🔒", "不必独自撑着",
      "生日快乐", "不必急着证明", "永远敢于选择", "平平安安",
      "有勇气也有运气", "所有烦恼都消失", "永远不缺底气", "健健康康🍵",
      "不再生病难受", "惊喜常伴", "天冷了 多穿衣服 🧣", "前路开阔",
      "愿你从容安宁", "有我一直陪着你 💕", "不为错过遗憾", "全世界最好的宝宝 💖",
      "不必总是坚强", "所想皆有答案", "永远有热爱", "不做噩梦不惊醒",
      "永远值得期待", "被世界温柔以待", "永远不失锋芒", "有人接住情绪",
      "愿宝宝真心感到幸福", "我爱你 ❤️"
    ];

    const BG_COLORS = [
      "#ffeaa7", "#fab1a0", "#ff7675", "#fd79a8", 
      "#a29bfe", "#74b9ff", "#81ecec", "#55efc4",
      "#dff9fb", "#f6e58d", "#ffbe76", "#fdcb6e"
    ];

    const TOTAL_WINDOWS = 255;
    const POPUP_INTERVAL = 20;
    const WAIT_TO_HEART = 1200;
    const HEART_DURATION = 3800;

    const container = document.getElementById("windows-container");
    const cakeStage = document.getElementById("cake-stage");
    const winElements = [];

    const screenW = window.innerWidth;
    const screenH = window.innerHeight;
    const winW = 125;
    const winH = 75;

    // 1. 逐个弹出铺满全屏
    let currentCount = 0;
    const timer = setInterval(() => {
      if (currentCount >= TOTAL_WINDOWS) {
        clearInterval(timer);
        setTimeout(gatherIntoHeart, WAIT_TO_HEART);
        return;
      }
      createPopup(currentCount);
      currentCount++;
    }, POPUP_INTERVAL);

    function createPopup(index) {
      const el = document.createElement("div");
      el.className = "win-modal";
      el.style.backgroundColor = BG_COLORS[index % BG_COLORS.length];

      const randX = Math.random() * (screenW - winW * 0.8) - 10;
      const randY = Math.random() * (screenH - winH * 0.8) - 10;
      el.style.left = randX + "px";
      el.style.top = randY + "px";
      el.style.zIndex = index + 1;

      const msg = MESSAGES[index % MESSAGES.length];
      el.innerHTML = `
        <div class="win-header">
          <span>温馨提示</span>
          <div class="win-dots">
            <div class="win-dot"></div>
            <div class="win-dot"></div>
          </div>
        </div>
        <div class="win-body">${msg}</div>
      `;

      container.appendChild(el);
      winElements.push(el);
    }

    // 2. 双层立体心形排布
    function gatherIntoHeart() {
      const centerX = screenW / 2 - winW / 2;
      const centerY = screenH * 0.42 - winH / 2;
      
      const TOTAL_HEART_ITEMS = Math.min(winElements.length, 122);
      const outerCount = Math.floor(TOTAL_HEART_ITEMS * 0.62);
      const innerCount = TOTAL_HEART_ITEMS - outerCount;

      const baseScale = Math.min(screenW, screenH);
      const outerScale = baseScale / 34;
      const innerScale = baseScale / 45;

      const excessWindows = winElements.slice(TOTAL_HEART_ITEMS);
      excessWindows.forEach(el => {
        el.style.opacity = "0";
        el.style.transform = "scale(0.2)";
        setTimeout(() => el.remove(), 800);
      });

      for (let i = 0; i < outerCount; i++) {
        const el = winElements[i];
        const t = (i / outerCount) * Math.PI * 2;
        const x = 16 * Math.pow(Math.sin(t), 3);
        const y = -(13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));

        el.style.zIndex = i + 10;
        el.style.left = (centerX + x * outerScale) + "px";
        el.style.top = (centerY + y * outerScale) + "px";
        el.style.transform = "scale(0.72)";
        el.style.opacity = "1";
      }

      for (let j = 0; j < innerCount; j++) {
        const el = winElements[outerCount + j];
        const t = (j / innerCount) * Math.PI * 2 + 0.15;
        const x = 16 * Math.pow(Math.sin(t), 3);
        const y = -(13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));

        el.style.zIndex = outerCount + j + 20;
        el.style.left = (centerX + x * innerScale) + "px";
        el.style.top = (centerY + y * innerScale + 10) + "px";
        el.style.transform = "scale(0.65)";
        el.style.opacity = "1";
      }

      setTimeout(() => {
        container.style.opacity = "0";
        container.style.transform = "scale(0.6)";
        cakeStage.classList.add("show");
      }, HEART_DURATION);
    }

    // ================= Web Audio 纯代码生日歌八音盒 =================
    let audioCtx = null;
    let isMusicPlaying = false;
    let musicTimer = null;
    let noteIndex = 0;

    const NOTES = {
      G5: 783.99, A5: 880.00, B5: 987.77, C6: 1046.50,
      D6: 1174.66, E6: 1318.51, F6: 1396.91
    };

    const SCORE = [
      { note: 'G5', duration: 0.22 }, { note: 'G5', duration: 0.22 },
      { note: 'A5', duration: 0.45 }, { note: 'G5', duration: 0.45 },
      { note: 'C6', duration: 0.45 }, { note: 'B5', duration: 0.8 },
      { note: 'G5', duration: 0.22 }, { note: 'G5', duration: 0.22 },
      { note: 'A5', duration: 0.45 }, { note: 'G5', duration: 0.45 },
      { note: 'D6', duration: 0.45 }, { note: 'C6', duration: 0.8 },
      { note: 'G5', duration: 0.22 }, { note: 'G5', duration: 0.22 },
      { note: 'G5', duration: 0.45 }, { note: 'E6', duration: 0.45 },
      { note: 'C6', duration: 0.45 }, { note: 'B5', duration: 0.45 },
      { note: 'A5', duration: 0.7 },
      { note: 'F6', duration: 0.22 }, { note: 'F6', duration: 0.22 },
      { note: 'E6', duration: 0.45 }, { note: 'C6', duration: 0.45 },
      { note: 'D6', duration: 0.45 }, { note: 'C6', duration: 1.0 }
    ];

    function initAudioContext() {
      if (!audioCtx) {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        audioCtx = new AudioContext();
      }
      if (audioCtx.state === 'suspended') {
        audioCtx.resume();
      }
    }

    function playBuzzerNote(freq, dur) {
      if (!audioCtx || !isMusicPlaying) return;
      const now = audioCtx.currentTime;

      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(freq, now);

      const overtone = audioCtx.createOscillator();
      const overtoneGain = audioCtx.createGain();
      overtone.type = 'triangle';
      overtone.frequency.setValueAtTime(freq * 2, now);

      gain.gain.setValueAtTime(0, now);
      gain.gain.linearRampToValueAtTime(0.3, now + 0.008);
      gain.gain.exponentialRampToValueAtTime(0.0001, now + dur * 1.5);

      overtoneGain.gain.setValueAtTime(0, now);
      overtoneGain.gain.linearRampToValueAtTime(0.08, now + 0.004);
      overtoneGain.gain.exponentialRampToValueAtTime(0.0001, now + 0.15);

      osc.connect(gain);
      gain.connect(audioCtx.destination);
      overtone.connect(overtoneGain);
      overtoneGain.connect(audioCtx.destination);

      osc.start(now);
      overtone.start(now);
      osc.stop(now + dur * 1.5);
      overtone.stop(now + 0.2);
    }

    function playMelodyStep() {
      if (!isMusicPlaying) return;
      const item = SCORE[noteIndex];
      playBuzzerNote(NOTES[item.note], item.duration);
      noteIndex = (noteIndex + 1) % SCORE.length;
      musicTimer = setTimeout(playMelodyStep, item.duration * 780);
    }

    function startMusic() {
      initAudioContext();
      if (!isMusicPlaying) {
        isMusicPlaying = true;
        document.getElementById("music-btn").classList.add("playing");
        playMelodyStep();
      }
    }

    function stopMusic() {
      isMusicPlaying = false;
      clearTimeout(musicTimer);
      document.getElementById("music-btn").classList.remove("playing");
    }

    function toggleMusic(e) {
      if (e) e.stopPropagation();
      initAudioContext();
      if (isMusicPlaying) {
        stopMusic();
      } else {
        startMusic();
      }
    }

    function autoStartAudioOnTouch() {
      startMusic();
      window.removeEventListener("touchstart", autoStartAudioOnTouch);
      window.removeEventListener("click", autoStartAudioOnTouch);
    }

    window.addEventListener("touchstart", autoStartAudioOnTouch, { passive: true });
    window.addEventListener("click", autoStartAudioOnTouch, { passive: true });

       // ================= 浩瀚星空与3D冷粉蓝粒子蛋糕渲染引擎 =================
    const canvas = document.getElementById("cakeCanvas");
    const ctx = canvas.getContext("2d");
    let cW = canvas.width = window.innerWidth;
    let cH = canvas.height = window.innerHeight;

    window.addEventListener("resize", () => {
      cW = canvas.width = window.innerWidth;
      cH = canvas.height = window.innerHeight;
    });

    // 1. 浩瀚星空背景
    const STARS = [];
    for (let i = 0; i < 220; i++) {
      STARS.push({
        x: Math.random() * window.innerWidth,
        y: Math.random() * window.innerHeight,
        r: Math.random() * 1.5 + 0.3,
        alpha: Math.random(),
        speed: Math.random() * 0.02 + 0.005
      });
    }

    // 2. 三层独立冷调调色盘
    const PALETTE_BOTTOM = [ // 底层：冰川冷蓝
      "rgba(116, 185, 255, ",
      "rgba(130, 204, 255, ",
      "rgba(85, 170, 255, ",
      "rgba(223, 249, 251, "
    ];
    const PALETTE_MIDDLE = [ // 中层：冷雾粉紫
      "rgba(162, 155, 254, ",
      "rgba(180, 160, 255, ",
      "rgba(210, 180, 255, ",
      "rgba(240, 225, 255, "
    ];
    const PALETTE_TOP = [    // 顶层：幻境冷粉
      "rgba(255, 159, 243, ",
      "rgba(255, 175, 235, ",
      "rgba(255, 200, 245, ",
      "rgba(255, 240, 250, "
    ];

    const CAKE_PARTICLES = [];

    // 实心三维圆盘蛋糕生成算法
    function createSolidTier(radius, yBase, height, density, palette) {
      // 顶部实体切面
      const topCount = Math.floor(density * 0.6);
      for (let i = 0; i < topCount; i++) {
        const r = radius * Math.sqrt(Math.random());
        const theta = Math.random() * Math.PI * 2;
        CAKE_PARTICLES.push({
          x: r * Math.cos(theta),
          y: yBase,
          z: r * Math.sin(theta),
          baseColor: palette[Math.floor(Math.random() * palette.length)],
          size: Math.random() * 1.5 + 1.0
        });
      }

      // 侧壁厚度填充
      const bodyCount = Math.floor(density * 0.8);
      for (let i = 0; i < bodyCount; i++) {
        const theta = Math.random() * Math.PI * 2;
        const r = radius * (0.85 + Math.random() * 0.15);
        const h = yBase + Math.random() * height;
        CAKE_PARTICLES.push({
          x: r * Math.cos(theta),
          y: h,
          z: r * Math.sin(theta),
          baseColor: palette[Math.floor(Math.random() * palette.length)],
          size: Math.random() * 1.6 + 1.1
        });
      }

      // 边缘银白发光奶油花边
      const rimCount = Math.floor(radius * 1.8);
      for (let i = 0; i < rimCount; i++) {
        const theta = (i / rimCount) * Math.PI * 2;
        CAKE_PARTICLES.push({
          x: (radius + 2) * Math.cos(theta),
          y: yBase + (Math.sin(theta * 10) * 3),
          z: (radius + 2) * Math.sin(theta),
          baseColor: "rgba(255, 255, 255, ",
          size: Math.random() * 1.8 + 1.2
        });
      }
    }

    // 生成三层：底层蓝、中层紫、顶层粉
    createSolidTier(160, 70, 70, 1400, PALETTE_BOTTOM);
    createSolidTier(115, -15, 65, 1100, PALETTE_MIDDLE);
    createSolidTier(70, -95, 60, 850, PALETTE_TOP);

    // 蜡烛主体（纯白光柱）
    for (let h = -100; h >= -175; h -= 2) {
      for (let cr = 0; cr < 6; cr++) {
        const theta = (cr / 6) * Math.PI * 2;
        CAKE_PARTICLES.push({
          x: 4.5 * Math.cos(theta) + (Math.random() - 0.5) * 1.5,
          y: h,
          z: 4.5 * Math.sin(theta) + (Math.random() - 0.5) * 1.5,
          baseColor: "rgba(235, 245, 255, ",
          size: 1.5
        });
      }
    }

    // 烛芯
    for (let h = -175; h >= -178; h -= 2) {
      CAKE_PARTICLES.push({
        x: (Math.random() - 0.5) * 1.5,
        y: h,
        z: (Math.random() - 0.5) * 1.5,
        baseColor: "rgba(180, 180, 200, ",
        size: 1.2
      });
    }

    // 烛火微粒（浅金色 + 香槟白金）
    let isCandleLit = true;
    const FLAME_PARTICLES = [];
    for (let f = 0; f < 65; f++) {
      FLAME_PARTICLES.push({
        x: (Math.random() - 0.5) * 7,
        y: -177 - Math.random() * 16,
        z: (Math.random() - 0.5) * 7,
        r: Math.random() * 3.2 + 1.5,
        color: Math.random() > 0.35 ? "rgba(255, 234, 167, " : "rgba(255, 248, 220, "
      });
    }

    const SMOKE_PARTICLES = [];

    // 吹蜡烛交互函数
    function blowCandleAndMakeWish() {
      if (!isCandleLit) return;
      isCandleLit = false;

      const btn = document.getElementById("wish-btn");
      if (btn) {
        btn.style.opacity = "0";
        btn.style.pointerEvents = "none";
      }

      for (let i = 0; i < 90; i++) {
        SMOKE_PARTICLES.push({
          x: (Math.random() - 0.5) * 12,
          y: -177,
          vx: (Math.random() - 0.5) * 2.8,
          vy: -(Math.random() * 3.5 + 1.8),
          alpha: 1,
          size: Math.random() * 2.2 + 1,
          color: Math.random() > 0.4 ? "rgba(255, 234, 167," : "rgba(255, 255, 255,"
        });
      }

      setTimeout(() => {
        const toast = document.getElementById("wish-toast");
        if (toast) toast.classList.add("show");
      }, 500);
    }

    let cakeAngle = 0;

    function renderCakeScene() {
      ctx.clearRect(0, 0, cW, cH);

      // 背景星空
      STARS.forEach(s => {
        s.alpha += s.speed;
        const opacity = (Math.sin(s.alpha) + 1) / 2 * 0.8 + 0.2;
        ctx.fillStyle = `rgba(255, 255, 255, ${opacity})`;
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
        ctx.fill();
      });

      cakeAngle += 0.009;
      const cosA = Math.cos(cakeAngle);
      const sinA = Math.sin(cakeAngle);

      const fov = 720;
      const originX = cW / 2;
      const originY = cH * 0.46;

      const projected = CAKE_PARTICLES.map(p => {
        const rotX = p.x * cosA - p.z * sinA;
        const rotZ = p.x * sinA + p.z * cosA;
        const tilt = 0.32;
        const rotY = p.y * Math.cos(tilt) - rotZ * Math.sin(tilt);
        const finalZ = p.y * Math.sin(tilt) + rotZ * Math.cos(tilt) + 320;

        const scale = fov / (fov + finalZ);
        return {
          px: originX + rotX * scale,
          py: originY + rotY * scale,
          scale: scale,
          finalZ: finalZ,
          color: p.baseColor,
          size: p.size * scale
        };
      });

      projected.sort((a, b) => b.finalZ - a.finalZ);

      ctx.globalCompositeOperation = "lighter";

      // 绘制蛋糕
      projected.forEach(p => {
        const alpha = Math.min(Math.max((p.scale - 0.25) * 1.4, 0.25), 0.95);
        ctx.fillStyle = p.color + alpha + ")";
        ctx.beginPath();
        ctx.arc(p.px, p.py, p.size, 0, Math.PI * 2);
        ctx.fill();
      });

      // 绘制跳动的浅金烛火
      if (isCandleLit) {
        FLAME_PARTICLES.forEach(fl => {
          const fScale = fov / (fov + 320);
          const fx = originX + (fl.x + Math.sin(Date.now() * 0.007 + fl.y) * 4) * fScale;
          const fy = originY + (fl.y - 12) * fScale;
          const alpha = (Math.sin(Date.now() * 0.009 + fl.r) + 1) / 2 * 0.6 + 0.35;

          ctx.fillStyle = fl.color + alpha + ")";
          ctx.beginPath();
          ctx.arc(fx, fy, fl.r * fScale, 0, Math.PI * 2);
          ctx.fill();
        });
      }

      // 吹灭蜡烛后的升空星尘
      for (let i = SMOKE_PARTICLES.length - 1; i >= 0; i--) {
        const sm = SMOKE_PARTICLES[i];
        sm.x += sm.vx;
        sm.y += sm.vy;
        sm.alpha -= 0.007;

        if (sm.alpha <= 0) {
          SMOKE_PARTICLES.splice(i, 1);
        } else {
          const fScale = fov / (fov + 320);
          ctx.fillStyle = `${sm.color} ${sm.alpha})`;
          ctx.beginPath();
          ctx.arc(originX + sm.x * fScale, originY + sm.y * fScale, sm.size * fScale, 0, Math.PI * 2);
          ctx.fill();
        }
      }

      ctx.globalCompositeOperation = "source-over";
      requestAnimationFrame(renderCakeScene);
    }

    renderCakeScene();
       
  </script>
</body>
</html>

# birthday

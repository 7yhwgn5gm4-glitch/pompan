<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>رماد الفضاء - تردد الطوارئ الأخير</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; touch-action: none; }

        body {
            background-color: #010103;
            color: #eee;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            user-select: none;
            -webkit-user-select: none;
        }

        #game-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            transition: transform 0.05s ease;
        }

        canvas { display: block; width: 100%; height: 100%; }

        #hud {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            pointer-events: none;
            z-index: 10;
            font-family: 'Courier New', Courier, monospace;
        }

        .radio-hud-panel {
            position: absolute;
            padding: 12px 18px;
            background: linear-gradient(180deg, rgba(12,28,14,0.85) 0%, rgba(6,15,8,0.95) 100%);
            border: 2px solid #2e5c33;
            border-radius: 4px;
            color: #5cd66c;
            font-size: 0.9rem;
            font-weight: bold;
            box-shadow: inset 0 0 10px rgba(0,255,0,0.2), 0 0 15px rgba(0,0,0,0.6);
            display: flex;
            flex-direction: column;
            gap: 2px;
        }

        .radio-hud-panel::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18,16,16,0) 50%, rgba(0,0,0,0.25) 50%),
                        linear-gradient(90deg, rgba(255,0,0,0.06), rgba(0,255,0,0.02), rgba(0,0,255,0.06));
            z-index: 2;
            background-size: 100% 3px, 6px 100%;
            pointer-events: none;
        }

        .panel-subtitle {
            font-size: 0.65rem;
            color: #1b6623;
            text-transform: uppercase;
            border-bottom: 1px solid rgba(46,92,51,0.4);
            padding-bottom: 2px;
            margin-bottom: 4px;
        }

        #hud-top-left { top: 15px; left: 15px; border-left: 4px solid #48bd58; }

        #hud-top-right {
            top: 15px; right: 15px;
            border-right: 4px solid #ff3333;
            color: #ff6666;
            background: linear-gradient(180deg, rgba(35,12,14,0.85) 0%, rgba(20,6,8,0.95) 100%);
            border-color: #8f2828;
            box-shadow: inset 0 0 10px rgba(255,0,0,0.15), 0 0 15px rgba(0,0,0,0.6);
        }
        #hud-top-right .panel-subtitle { color: #8f2828; border-color: rgba(143,40,40,0.4); }

        #hud-bottom-left {
            bottom: 15px; left: 15px;
            border-left: 4px solid #666;
            color: #aaa;
            background: rgba(15,15,20,0.8);
            border-color: #444;
            box-shadow: 0 0 10px rgba(0,0,0,0.8);
        }

        /* ─── بار تقدم المرحلة ─── */
        #hud-progress {
            position: absolute;
            bottom: 15px;
            left: 50%;
            transform: translateX(-50%);
            width: 260px;
            background: rgba(10,10,15,0.85);
            border: 1px solid #333;
            border-radius: 4px;
            padding: 8px 14px;
            font-family: 'Courier New', monospace;
            font-size: 0.72rem;
            color: #555;
            text-align: center;
        }
        #progress-bar-bg {
            width: 100%;
            height: 5px;
            background: rgba(255,255,255,0.06);
            border-radius: 2px;
            margin-top: 5px;
            overflow: hidden;
        }
        #progress-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, #cc2222, #ff5500);
            border-radius: 2px;
            width: 0%;
            transition: width 0.3s ease;
            box-shadow: 0 0 6px #ff3300;
        }

        /* ─── مؤشر الكومبو ─── */
        #combo-display {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-family: 'Courier New', monospace;
            font-size: 2.2rem;
            font-weight: bold;
            color: #ff4400;
            text-shadow: 0 0 20px #ff2200;
            opacity: 0;
            pointer-events: none;
            z-index: 15;
            transition: opacity 0.3s;
        }

        .hud-glitch {
            animation: radioStatic 0.15s infinite;
            color: #ff3333 !important;
            border-color: #ff0000 !important;
        }

        @keyframes radioStatic {
            0%   { transform: translate(1px,1px) skew(0deg); opacity: 0.95; }
            50%  { transform: translate(-1px,-1px) skew(1deg); opacity: 0.8; }
            100% { transform: translate(0px,2px) skew(-1deg); opacity: 1; }
        }

        .menu-screen {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(2,1,4,0.94);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 20;
            backdrop-filter: blur(10px);
        }

        .menu-screen h1 {
            font-size: 3.5rem;
            margin-bottom: 20px;
            color: #cc2222;
            text-shadow: 0 0 25px #ff0000;
            letter-spacing: 2px;
        }

        .menu-screen p {
            font-size: 1.2rem;
            margin-bottom: 30px;
            color: #777;
            max-width: 650px;
            text-align: center;
            line-height: 1.6;
        }

        .radio-terminal {
            width: 95%;
            max-width: 750px;
            background: linear-gradient(135deg, rgba(16,6,12,0.85) 0%, rgba(6,6,10,0.95) 100%);
            border: 1px solid rgba(255,50,50,0.3);
            border-right: 4px solid #cc2222;
            padding: 22px;
            margin-bottom: 25px;
            border-radius: 6px;
            box-shadow: 0 0 30px rgba(255,0,0,0.1);
        }

        .radio-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255,50,50,0.15);
            padding-bottom: 10px;
            margin-bottom: 15px;
            font-size: 0.85rem;
        }

        .radio-title {
            color: #ff4444;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .live-dot {
            width: 8px; height: 8px;
            background-color: #ff1111;
            border-radius: 50%;
            animation: pulse 1s infinite alternate;
        }

        @keyframes pulse {
            0%   { transform: scale(0.9); opacity: 0.4; }
            100% { transform: scale(1.2); opacity: 1; box-shadow: 0 0 8px 2px rgba(255,0,0,0.6); }
        }

        .audio-waveform { display: flex; align-items: center; gap: 3px; height: 20px; }
        .bar { width: 2px; height: 100%; background-color: #cc2222; animation: bounceWave 0.6s infinite ease-in-out alternate; }
        .bar:nth-child(2) { animation-delay: 0.1s; }
        .bar:nth-child(3) { animation-delay: 0.3s; }
        .bar:nth-child(4) { animation-delay: 0.2s; }
        .bar:nth-child(5) { animation-delay: 0.4s; }
        @keyframes bounceWave { 0% { height: 20%; } 100% { height: 100%; } }

        .radio-body { min-height: 70px; font-size: 1.15rem; line-height: 1.6; color: #ffb3b3; }

        .cursor-blink {
            display: inline-block;
            width: 8px; height: 18px;
            background-color: #ff4444;
            margin-right: 4px;
            animation: blink 0.8s infinite;
            vertical-align: middle;
        }
        @keyframes blink { 0%, 100% { opacity: 0; } 50% { opacity: 1; } }

        button {
            padding: 12px 35px;
            font-size: 1.2rem;
            color: #cc2222;
            background: transparent;
            border: 1px solid #cc2222;
            border-radius: 4px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 10px;
        }
        button:hover { background: #cc2222; color: #000; box-shadow: 0 0 25px #ff3333; }

        .shop-buttons { display: flex; flex-direction: column; gap: 12px; width: 100%; align-items: center; }

        .shop-btn {
            border-color: #444; color: #999;
            width: 100%; max-width: 580px;
            text-align: right;
            padding: 14px 20px;
            border-radius: 6px;
            background: rgba(10,10,15,0.4);
            font-size: 1.05rem;
        }
        .shop-btn:hover { background: rgba(30,10,15,0.7); color: #fff; border-color: #ff3333; }

        #whiteVoidScreen { background: #ffffff; color: #111; transition: opacity 2.5s ease; }
        #whiteVoidScreen p { color: #333; font-weight: bold; }

        /* ─── رسالة طارئة مؤقتة وسط الشاشة ─── */
        #flash-msg {
            position: absolute;
            top: 38%;
            left: 50%;
            transform: translateX(-50%);
            font-family: 'Courier New', monospace;
            font-size: 1.1rem;
            color: #ff6600;
            text-shadow: 0 0 10px #ff3300;
            background: rgba(0,0,0,0.7);
            border: 1px solid #ff3300;
            padding: 10px 22px;
            border-radius: 4px;
            opacity: 0;
            pointer-events: none;
            z-index: 15;
            transition: opacity 0.4s;
            white-space: nowrap;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>
<div id="game-container">

    <div id="hud" class="hidden">
        <div id="hud-top-left" class="radio-hud-panel">
            <span class="panel-subtitle">REC_SIGNAL_LOG</span>
            <span>DESTRUCTION_PTS: <span id="scoreEl">0</span></span>
            <span style="font-size:0.75rem; color:#3a8a42;">COMBO: x<span id="comboEl">1</span></span>
        </div>
        <div id="hud-top-right" class="radio-hud-panel">
            <span class="panel-subtitle">TERRESTRIAL_COM_STATUS</span>
            <span>SURVIVORS: <span id="popEl">2,400,000</span></span>
        </div>
        <div id="hud-bottom-left" class="radio-hud-panel">
            <span class="panel-subtitle">CURRENT_ORBITAL_DEPTH</span>
            <span>SECTOR: LVL_<span id="levelEl">1</span></span>
        </div>
        <div id="hud-progress">
            <span id="progress-label">تقدم المرحلة</span>
            <div id="progress-bar-bg"><div id="progress-bar-fill"></div></div>
        </div>
    </div>

    <div id="combo-display"></div>
    <div id="flash-msg"></div>

    <canvas id="gameCanvas"></canvas>

    <!-- شاشة البداية -->
    <div id="startScreen" class="menu-screen">
        <h1>رماد الفضاء</h1>
        <p>أنت الخط الدفاعي الأخير واليائس حول كوكب محتضر.<br>حرك المركبة بالفأرة أو عبر سحب اللمس، واضغط مستمراً للإطلاق.</p>
        <button id="startBtn">ابدأ الرحلة الانتحارية</button>
    </div>

    <!-- شاشة المتجر -->
    <div id="shopScreen" class="menu-screen hidden">
        <div class="radio-terminal">
            <div class="radio-header">
                <div class="radio-title"><span class="live-dot"></span> اعتراض تردد الطوارئ الاستراتيجي الوارد...</div>
                <div class="audio-waveform">
                    <div class="bar"></div><div class="bar"></div><div class="bar"></div><div class="bar"></div><div class="bar"></div>
                </div>
            </div>
            <div id="storyLog" class="radio-body">جاري الاتصال...</div>
        </div>
        <p style="font-size:1.05rem; margin-bottom:25px; color:#555;">⚠️ تنبيه: تركيب تعديلات هندسية على السفينة سيتطلب تضحية بأنظمة أخرى:</p>
        <div class="shop-buttons">
            <button id="shopOpt1" class="shop-btn">تعديل 1</button>
            <button id="shopOpt2" class="shop-btn">تعديل 2</button>
        </div>
    </div>

    <!-- شاشة النهاية البيضاء -->
    <div id="whiteVoidScreen" class="menu-screen hidden">
        <h1 style="color:#111; text-shadow:none; font-size:2.8rem;">صمت مطلق</h1>
        <div id="voidLog" style="max-width:650px; text-align:center; line-height:1.8; font-size:1.25rem; margin-bottom:40px; min-height:100px;"></div>
        <button id="restartFromVoidBtn" class="hidden" style="border-color:#222; color:#222;">إعادة تكرار الوهم</button>
    </div>

    <!-- شاشة نهاية اللعبة -->
    <div id="gameOverScreen" class="menu-screen hidden">
        <h1>انطفأت الأنوار</h1>
        <p id="gameOverText">تحطمت السفينة وتحول جسدك إلى غبار كوني...</p>
        <p style="font-size:1.2rem; color:#cc2222;">النتيجة النهائية: <span id="finalScore">0</span> فضائي مُدمر.</p>
        <button id="restartBtn" style="border-color:#444; color:#666;">كرر المحاولة اليائسة</button>
    </div>

</div>

<script>
// ════════════════════════════════════════════════════════
//  رماد الفضاء -- نسخة محسّنة ومُصلَحة
// ════════════════════════════════════════════════════════

const gameContainer = document.getElementById('game-container');
const canvas        = document.getElementById('gameCanvas');
const ctx           = canvas.getContext('2d');

canvas.width  = window.innerWidth;
canvas.height = window.innerHeight;

// ── عناصر HUD ──
const scoreEl        = document.getElementById('scoreEl');
const comboEl        = document.getElementById('comboEl');
const popEl          = document.getElementById('popEl');
const levelEl        = document.getElementById('levelEl');
const hud            = document.getElementById('hud');
const hudLeft        = document.getElementById('hud-top-left');
const hudRight       = document.getElementById('hud-top-right');
const progressFill   = document.getElementById('progress-bar-fill');
const progressLabel  = document.getElementById('progress-label');
const comboDisplay   = document.getElementById('combo-display');
const flashMsg       = document.getElementById('flash-msg');

const startScreen       = document.getElementById('startScreen');
const shopScreen        = document.getElementById('shopScreen');
const storyLog          = document.getElementById('storyLog');
const gameOverScreen    = document.getElementById('gameOverScreen');
const whiteVoidScreen   = document.getElementById('whiteVoidScreen');
const voidLog           = document.getElementById('voidLog');
const startBtn          = document.getElementById('startBtn');
const restartBtn        = document.getElementById('restartBtn');
const restartFromVoidBtn= document.getElementById('restartFromVoidBtn');
const shopOpt1          = document.getElementById('shopOpt1');
const shopOpt2          = document.getElementById('shopOpt2');
const finalScoreEl      = document.getElementById('finalScore');

// ── حالة اللعبة ──
let isPlaying      = false;
let isShopOpen     = false;
let inCutscene     = false;
let inVoidEnd      = false;
let cutsceneTimer  = 0;
let shakeTimer     = 0;
let textTypeInterval = null;
let animationId;

// ── متغيرات اللعب ──
let score          = 0;
let population     = 2_400_000;
let level          = 1;
let frames         = 0;

// ✅ إصلاح #4 و#15: عتبة المرحلة أعلى بكثير لمراحل أطول
let scoreForNextLevel = 600;   // كان 200 -- الآن ثلاثة أضعاف

let player, boss = null;
let projectiles      = [];
let enemyProjectiles = [];
let enemies          = [];
let particles        = [];
let powerUps         = [];
let stars            = [];
let debris           = [];
let nebulaAngle      = 0;

// ── نظام الكومبو ──
let comboCount      = 1;
let comboTimer      = 0;
const COMBO_TIMEOUT = 180; // فريم (~3 ثواني)
let comboDisplayTimer = 0;

let flashTimer = 0;
let flashText  = '';

const mouse = { x: window.innerWidth / 2, y: window.innerHeight - 100, isDown: false };

// ═══════════════════════════════════════════════════════
//  أحداث الإدخال
// ═══════════════════════════════════════════════════════
window.addEventListener('mousemove', e => {
    if (inCutscene || isShopOpen || inVoidEnd) return;
    mouse.x = e.clientX; mouse.y = e.clientY;
});
window.addEventListener('mousedown', () => { if (!inCutscene && !isShopOpen && !inVoidEnd) mouse.isDown = true; });
window.addEventListener('mouseup',   () => mouse.isDown = false);

function handleTouchInput(e) {
    if (inCutscene || isShopOpen || inVoidEnd || !isPlaying) return;
    e.preventDefault();
    if (e.touches.length > 0) {
        mouse.x = e.touches[0].clientX;
        mouse.y = e.touches[0].clientY;
    }
}
window.addEventListener('touchstart', e => { handleTouchInput(e); if (!inCutscene && !isShopOpen && !inVoidEnd) mouse.isDown = true; }, { passive: false });
window.addEventListener('touchmove',  handleTouchInput, { passive: false });
window.addEventListener('touchend',   () => mouse.isDown = false);

window.addEventListener('resize', () => {
    canvas.width  = window.innerWidth;
    canvas.height = window.innerHeight;
    initStars();
});

// ═══════════════════════════════════════════════════════
//  مساعِد: إظهار رسالة مؤقتة
// ═══════════════════════════════════════════════════════
function showFlash(text, duration = 120) {
    flashText  = text;
    flashTimer = duration;
    flashMsg.textContent = text;
    flashMsg.style.opacity = '1';
}

// ═══════════════════════════════════════════════════════
//  نظام الكومبو
// ═══════════════════════════════════════════════════════
function addKill() {
    comboTimer = COMBO_TIMEOUT;
    comboCount++;
    if (comboCount > 1) {
        comboDisplayTimer = 80;
        comboDisplay.textContent = `COMBO x${comboCount}!`;
        comboDisplay.style.opacity = '1';
        comboDisplay.style.fontSize = Math.min(2.2 + comboCount * 0.1, 4) + 'rem';
    }
    comboEl.textContent = comboCount;
}

function updateCombo() {
    if (comboTimer > 0) {
        comboTimer--;
        if (comboTimer === 0) { comboCount = 1; comboEl.textContent = 1; }
    }
    if (comboDisplayTimer > 0) {
        comboDisplayTimer--;
        if (comboDisplayTimer === 0) { comboDisplay.style.opacity = '0'; }
    }
    if (flashTimer > 0) {
        flashTimer--;
        if (flashTimer === 0) { flashMsg.style.opacity = '0'; }
    }
}

// ═══════════════════════════════════════════════════════
//  إنشاء جزيئات الانفجار
// ═══════════════════════════════════════════════════════
function createExplosion(x, y, color, big = false) {
    const count = big ? 50 : (color === '#ffffff' ? 35 : 18);
    for (let i = 0; i < count; i++) {
        particles.push(new Particle(x, y, color, 0.96, Math.random() < 0.4));
    }
    // جزيئات دخان إضافية للانفجارات الكبيرة
    if (big) {
        for (let i = 0; i < 12; i++) {
            particles.push(new Particle(x + (Math.random()-0.5)*30, y + (Math.random()-0.5)*30,
                'rgba(80,80,85,0.5)', 0.98));
        }
    }
}

// ═══════════════════════════════════════════════════════
//  الفئات
// ═══════════════════════════════════════════════════════

class Star {
    constructor(isInitial = false) {
        this.x = Math.random() * canvas.width;
        this.y = isInitial ? Math.random() * canvas.height : -10;
        const layer = Math.random();
        if (layer < 0.5) {
            this.size = Math.random() * 0.5 + 0.1; this.baseSpeed = Math.random() * 0.15 + 0.05; this.alpha = Math.random() * 0.3 + 0.1;
        } else if (layer < 0.85) {
            this.size = Math.random() * 1.0 + 0.5; this.baseSpeed = Math.random() * 0.4 + 0.2; this.alpha = Math.random() * 0.5 + 0.3;
        } else {
            this.size = Math.random() * 1.6 + 1.0; this.baseSpeed = Math.random() * 1.2 + 0.7; this.alpha = Math.random() * 0.7 + 0.4;
        }
        this.speed = this.baseSpeed;
    }
    draw() {
        if (inVoidEnd) return;
        ctx.save(); ctx.globalAlpha = this.alpha;
        if (isShopOpen) {
            ctx.strokeStyle = `rgba(150,20,30,${this.alpha})`; ctx.lineWidth = this.size;
            ctx.beginPath(); ctx.moveTo(this.x, this.y); ctx.lineTo(this.x, this.y + 35); ctx.stroke();
        } else {
            ctx.fillStyle = '#b3b3cc';
            ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2); ctx.fill();
        }
        ctx.restore();
    }
    update() {
        if (isShopOpen) this.speed = this.baseSpeed * 12;
        else if (boss || inCutscene) this.speed = this.baseSpeed * 0.15;
        else this.speed = this.baseSpeed;
        this.y += this.speed;
        this.draw();
    }
}

class Debris {
    constructor() {
        this.x = Math.random() * canvas.width; this.y = -30;
        this.size = Math.random() * 15 + 5; this.speed = Math.random() * 0.4 + 0.1;
        this.angle = Math.random() * Math.PI * 2; this.rotSpeed = Math.random() * 0.01 - 0.005;
    }
    draw() {
        if (inVoidEnd) return;
        ctx.save(); ctx.translate(this.x, this.y); ctx.rotate(this.angle);
        ctx.fillStyle = 'rgba(35,35,42,0.4)'; ctx.strokeStyle = 'rgba(60,60,68,0.3)'; ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(-this.size/2, -this.size/2); ctx.lineTo(this.size/2, -this.size/4);
        ctx.lineTo(this.size/3, this.size/2);   ctx.lineTo(-this.size/2, this.size/3);
        ctx.closePath(); ctx.fill(); ctx.stroke(); ctx.restore();
    }
    update() { this.y += this.speed; this.angle += this.rotSpeed; this.draw(); }
}

class Player {
    constructor() {
        this.x = canvas.width / 2; this.y = canvas.height + 100;
        this.radius = 20; this.color = '#55555c';
        this.health = 100; this.maxHealth = 100;
        // ✅ إصلاح #6: رفع السرعة لتتبع أفضل
        this.speed = 0.13;
        this.powerUpType = 'normal'; this.powerUpTimer = 0;
        this.shootCooldown = 0;
        this.maxAmmo = 30; this.ammo = this.maxAmmo;
        this.isReloading = false; this.reloadTimer = 0; this.reloadDuration = 90;
        this.magnetRadius = 80;   // ✅ تحسين: نطاق المغناطيس أكبر افتراضياً
        this.autoHeal = false;
        this.projectileSpeedBonus = 0;
        // ✅ إصلاح #6: سرعة إطلاق أسرع قليلاً
        this.shootCooldownDuration = 8;
        this.invincibleTimer = 0; // لحظات اللامسيسة بعد الضرر
    }

    draw() {
        ctx.save(); ctx.translate(this.x, this.y);

        // لهب المحرك
        if (isPlaying && !isShopOpen && !inVoidEnd) {
            ctx.beginPath();
            ctx.moveTo(-6, this.radius); ctx.lineTo(0, this.radius + (Math.random()*15+10)); ctx.lineTo(6, this.radius);
            ctx.closePath();
            ctx.fillStyle = Math.random() < 0.5 ? '#cc2200' : '#ff5500'; ctx.fill();
        }

        // جسم السفينة
        ctx.beginPath();
        ctx.moveTo(0, -this.radius); ctx.lineTo(this.radius, this.radius);
        ctx.lineTo(0, this.radius - 4); ctx.lineTo(-this.radius, this.radius);
        ctx.closePath();
        ctx.shadowBlur    = (inCutscene || inVoidEnd) ? 25 : 8;
        ctx.shadowColor   = inVoidEnd ? '#222' : '#cc1111';
        ctx.fillStyle     = inVoidEnd ? '#222' : '#121214';
        ctx.fill();
        ctx.strokeStyle = '#444'; ctx.lineWidth = 2; ctx.stroke();

        // وميض اللامسيسة
        if (this.invincibleTimer > 0 && Math.floor(this.invincibleTimer / 5) % 2 === 0) {
            ctx.strokeStyle = 'rgba(255,200,0,0.6)'; ctx.lineWidth = 3;
            ctx.beginPath(); ctx.arc(0, 0, this.radius + 6, 0, Math.PI * 2); ctx.stroke();
        }
        ctx.restore();

        // حلقات الصحة والذخيرة
        if (isPlaying && !isShopOpen && !inCutscene && !inVoidEnd) {
            ctx.save(); ctx.translate(this.x, this.y); ctx.globalAlpha = 0.55;

            // صحة (يمين)
            ctx.beginPath(); ctx.arc(0, 0, this.radius + 12, -Math.PI/2, Math.PI/2, false);
            ctx.strokeStyle = 'rgba(255,0,0,0.12)'; ctx.lineWidth = 3; ctx.stroke();
            ctx.beginPath();
            ctx.arc(0, 0, this.radius + 12, -Math.PI/2, -Math.PI/2 + Math.PI*(this.health/this.maxHealth), false);
            ctx.strokeStyle = '#ff3333'; ctx.stroke();

            // ذخيرة (يسار)
            ctx.beginPath(); ctx.arc(0, 0, this.radius + 12, -Math.PI/2, -Math.PI*1.5, true);
            ctx.strokeStyle = 'rgba(0,150,255,0.12)'; ctx.stroke();
            ctx.beginPath();
            if (this.isReloading) {
                const rPct = (this.reloadDuration - this.reloadTimer) / this.reloadDuration;
                ctx.arc(0, 0, this.radius + 12, -Math.PI/2, -Math.PI/2 - Math.PI*rPct, true);
                ctx.strokeStyle = '#ffcc00';
            } else {
                const aPct = this.ammo / this.maxAmmo;
                ctx.arc(0, 0, this.radius + 12, -Math.PI/2, -Math.PI/2 - Math.PI*aPct, true);
                ctx.strokeStyle = '#00ccff';
            }
            ctx.stroke(); ctx.restore();
        }
    }

    update() {
        if (inCutscene) {
            this.x = canvas.width / 2;
            if (this.y > canvas.height - 100) this.y -= 0.8;
        } else if (inVoidEnd) {
            this.y -= 0.15; this.x += Math.sin(frames * 0.01) * 0.2;
        } else if (!isShopOpen) {
            this.x += (mouse.x - this.x) * this.speed;
            this.y += (mouse.y - this.y) * this.speed;
            this.x = Math.max(this.radius, Math.min(canvas.width - this.radius, this.x));
            // ✅ إصلاح #12: السماح للاعب بالتحرك في 70% العلوي (كان 60%)
            this.y = Math.max(canvas.height * 0.25, Math.min(canvas.height - this.radius, this.y));
        }

        if (this.invincibleTimer > 0) this.invincibleTimer--;

        if (this.isReloading && !isShopOpen && !inVoidEnd) {
            this.reloadTimer--;
            if (this.reloadTimer <= 0) {
                this.ammo = this.maxAmmo; this.isReloading = false; updateHUD();
                for (let k = 0; k < 8; k++) particles.push(new Particle(this.x, this.y, '#00ffff', 0.9, true));
            }
        }

        if (this.shootCooldown > 0) this.shootCooldown--;
        if (mouse.isDown && this.shootCooldown === 0 && !this.isReloading && !inCutscene && !isShopOpen && !inVoidEnd) {
            this.shoot();
        }

        if (this.powerUpTimer > 0) {
            this.powerUpTimer--;
            if (this.powerUpTimer === 0) this.powerUpType = 'normal';
        }

        if (this.autoHeal && frames % 90 === 0 && this.health < this.maxHealth && !isShopOpen && !inVoidEnd) {
            this.health = Math.min(this.maxHealth, this.health + 2); updateHUD();
        }

        if (this.health < 80 && frames % (this.health <= 30 ? 3 : 8) === 0 && !inCutscene && !isShopOpen && !inVoidEnd) {
            particles.push(new Particle(this.x + (Math.random()*10-5), this.y + this.radius, 'rgba(10,10,10,0.6)', 0.94));
            if (this.health <= 40 && Math.random() < 0.5)
                particles.push(new Particle(this.x + (Math.random()*16-8), this.y, '#fff', 0.9, true));
        }
        this.draw();
    }

    shoot() {
        if (this.ammo <= 0) { this.isReloading = true; this.reloadTimer = this.reloadDuration; return; }
        this.shootCooldown = this.shootCooldownDuration; this.ammo--; updateHUD();

        const sb = this.projectileSpeedBonus;
        if (this.powerUpType === 'double') {
            projectiles.push(new Projectile(this.x - 10, this.y - this.radius, -90, '#ff7777', false, false, sb));
            projectiles.push(new Projectile(this.x + 10, this.y - this.radius, -90, '#ff7777', false, false, sb));
        } else if (this.powerUpType === 'spread') {
            projectiles.push(new Projectile(this.x, this.y - this.radius, -90,  '#ff7777', false, false, sb));
            projectiles.push(new Projectile(this.x, this.y - this.radius, -102, '#ff7777', false, false, sb));
            projectiles.push(new Projectile(this.x, this.y - this.radius, -78,  '#ff7777', false, false, sb));
        } else if (this.powerUpType === 'laser') {
            // ✅ جديد: قوة الليزر
            for (let dx = -1; dx <= 1; dx++) {
                projectiles.push(new Projectile(this.x + dx*8, this.y - this.radius, -90, '#ff4444', false, false, sb + 4));
            }
        } else {
            projectiles.push(new Projectile(this.x, this.y - this.radius, -90, '#cc4444', false, false, sb));
        }
    }

    takeDamage(amount) {
        if (this.invincibleTimer > 0 || this.powerUpType === 'shield') {
            if (this.powerUpType === 'shield') { this.powerUpType = 'normal'; this.powerUpTimer = 0; }
            return false;
        }
        this.health = Math.max(0, this.health - amount);
        this.invincibleTimer = 25; // لحظات قصيرة من اللامسيسة
        updateHUD();
        return this.health <= 0;
    }
}

class Projectile {
    constructor(x, y, angleDeg, color = '#cc4444', isEnemy = false, isRPG = false, speedBonus = 0) {
        this.x = x; this.y = y; this.isEnemy = isEnemy; this.isRPG = isRPG;
        this.radius = isRPG ? 7 : (isEnemy ? 5 : 3.5);
        this.speed  = isRPG ? 4.5 : (isEnemy ? 5.0 : 12 + speedBonus);
        this.color  = color;
        const rad = angleDeg * Math.PI / 180;
        this.velocity = { x: Math.cos(rad) * this.speed, y: Math.sin(rad) * this.speed };
    }
    draw() {
        ctx.beginPath();
        if (this.isRPG) {
            ctx.save(); ctx.translate(this.x, this.y);
            ctx.fillStyle = this.color; ctx.shadowBlur = 10; ctx.shadowColor = '#ff0000';
            ctx.fillRect(-2, -9, 4, 18);
            ctx.beginPath(); ctx.moveTo(-4,9); ctx.lineTo(4,9); ctx.lineTo(0,15);
            ctx.closePath(); ctx.fillStyle = '#ff5500'; ctx.fill(); ctx.restore();
        } else {
            ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2);
            ctx.shadowBlur = 8; ctx.shadowColor = this.color;
            ctx.fillStyle = this.color; ctx.fill();
        }
    }
    update() {
        this.x += this.velocity.x; this.y += this.velocity.y;
        if (this.isRPG && frames % 4 === 0 && !isShopOpen)
            particles.push(new Particle(this.x, this.y - 5, 'rgba(60,60,65,0.4)', 0.98));
        this.draw();
    }
}

class BigBoss {
    constructor(lvl) {
        this.x = canvas.width / 2; this.y = -130; this.targetY = 140; this.radius = 65;
        this.isAllyShip = (lvl === 4);
        this.color      = this.isAllyShip ? '#4466aa' : this._bossColor(lvl);
        // ✅ إصلاح #9: بوس أقوى مع صحة أكثر لمراحل أطول ولكن متدرجة
        this.maxHealth  = 40 + (lvl * 18);
        this.health     = this.maxHealth;
        this.speedX     = (lvl === 6 || lvl >= 7) ? 5.5 : 2.0 + (lvl * 0.35);
        this.direction  = 1; this.shootCooldown = 0;
        this.points     = 150 + lvl * 20;
        this.phase      = 1; // مرحلة القتال (تزيد الصعوبة عند نصف الصحة)
    }
    _bossColor(lvl) {
        const cols = { 1:'#880000', 2:'#1a551a', 3:'#773c00', 5:'#991100', 6:'#555500' };
        return cols[lvl] || '#ffffff';
    }
    draw() {
        ctx.save(); ctx.translate(this.x, this.y); ctx.beginPath();
        if (this.isAllyShip) {
            ctx.moveTo(0, this.radius); ctx.lineTo(this.radius, -this.radius);
            ctx.lineTo(this.radius*0.4, -this.radius*0.5); ctx.lineTo(0, -this.radius*1.1);
            ctx.lineTo(-this.radius*0.4, -this.radius*0.5); ctx.lineTo(-this.radius, -this.radius);
        } else {
            ctx.moveTo(0, -this.radius); ctx.lineTo(this.radius, -this.radius/2);
            ctx.lineTo(this.radius*1.1, this.radius/3); ctx.lineTo(this.radius/2, this.radius);
            ctx.lineTo(-this.radius/2, this.radius); ctx.lineTo(-this.radius*1.1, this.radius/3);
            ctx.lineTo(-this.radius, -this.radius/2);
        }
        ctx.closePath();
        ctx.shadowBlur = 20; ctx.shadowColor = this.color;
        ctx.fillStyle = '#070305'; ctx.fill();
        ctx.strokeStyle = this.color; ctx.lineWidth = 3; ctx.stroke();

        // وميض المرحلة الثانية
        if (this.phase === 2 && frames % 20 < 10) {
            ctx.strokeStyle = '#ff2200'; ctx.lineWidth = 1.5;
            ctx.beginPath(); ctx.arc(0, 0, this.radius + 12, 0, Math.PI*2); ctx.stroke();
        }
        ctx.restore();

        // شريط صحة البوس
        const bw = 400; const bx = canvas.width/2 - bw/2;
        ctx.fillStyle = 'rgba(20,5,5,0.5)'; ctx.fillRect(bx, 35, bw, 8);
        const healthColor = this.phase === 2 ? '#ff2200' : this.color;
        ctx.fillStyle = healthColor;
        ctx.fillRect(bx, 35, bw*(this.health/this.maxHealth), 8);
        // إطار
        ctx.strokeStyle = 'rgba(255,255,255,0.1)'; ctx.lineWidth = 1;
        ctx.strokeRect(bx, 35, bw, 8);
    }
    update() {
        // تفعيل المرحلة الثانية عند نصف الصحة
        if (this.health <= this.maxHealth / 2 && this.phase === 1) {
            this.phase = 2;
            this.speedX *= 1.4;
            triggerCameraShake(20);
            showFlash('⚠️ البوس دخل المرحلة الثانية!', 120);
        }

        if (this.y < this.targetY) { this.y += 2; }
        else {
            this.x += this.speedX * this.direction;
            if (this.x + this.radius > canvas.width || this.x - this.radius < 0) this.direction *= -1;
            this.shootCooldown--;
            if (this.shootCooldown <= 0) {
                this.shoot();
                const baseCooldown = Math.max(14, 55 - level*3);
                this.shootCooldown = this.phase === 2 ? Math.floor(baseCooldown * 0.65) : baseCooldown;
            }
        }
        this.draw();
    }
    shoot() {
        const ang = Math.atan2(player.y - this.y, player.x - this.x) * 180 / Math.PI;
        if (this.isAllyShip) {
            for (let a = 0; a < 360; a += 45)
                enemyProjectiles.push(new Projectile(this.x, this.y+10, a, 'rgba(0,255,100,0.75)', true));
        } else if (level === 1) {
            enemyProjectiles.push(new Projectile(this.x, this.y+25, 90,  this.color, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, 75,  this.color, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, 105, this.color, true));
        } else if (level <= 3) {
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang, this.color, true));
            if (this.phase === 2) {
                enemyProjectiles.push(new Projectile(this.x, this.y+25, ang-15, this.color, true));
                enemyProjectiles.push(new Projectile(this.x, this.y+25, ang+15, this.color, true));
            }
        } else if (level === 6) {
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang,    this.color, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang-12, this.color, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang+12, this.color, true));
            if (this.phase === 2) {
                enemyProjectiles.push(new Projectile(this.x, this.y+25, ang-24, this.color, true));
                enemyProjectiles.push(new Projectile(this.x, this.y+25, ang+24, this.color, true));
            }
        } else {
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang,    '#cc0000', true, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang-18, this.color, true));
            enemyProjectiles.push(new Projectile(this.x, this.y+25, ang+18, this.color, true));
        }
    }
}

class Enemy {
    constructor(x, y) {
        this.x = x; this.y = y;
        this.shootCooldown = Math.random() * 80 + 60;
        const rand = Math.random();
        // ✅ إصلاح #2 و #8: أعداء بصحة أكثر ومتنوعة
        if (rand < 0.25 && level > 2) {
            this.type = 'fast'; this.radius = 14; this.color = '#aaaa22';
            this.health = 1; this.speed = 2.8 + level*0.2; this.points = 20;
        } else if (rand < 0.45 && level > 3) {
            this.type = 'tank'; this.radius = 32; this.color = '#881111';
            this.health = 5 + Math.floor(level/2); this.speed = 0.9 + level*0.08; this.points = 50;
        } else if (rand < 0.6 && level > 5) {
            this.type = 'sniper'; this.radius = 18; this.color = '#2255aa';
            this.health = 2; this.speed = 1.2; this.points = 35;
            this.shootCooldown = 30; // يطلق نار كثير
        } else {
            this.type = 'normal'; this.radius = 22; this.color = '#2d552d';
            this.health = 2 + Math.floor(level/3); // ✅ صحة أعلى بالتدريج
            this.speed = 1.4 + level*0.12; this.points = 10;
        }
    }
    draw() {
        ctx.beginPath();
        const sides = this.type === 'tank' ? 6 : (this.type === 'fast' ? 3 : (this.type === 'sniper' ? 4 : 5));
        for (let i = 0; i < sides; i++) {
            const angle = (i * 2 * Math.PI / sides) - Math.PI/2;
            const px = this.x + Math.cos(angle)*this.radius;
            const py = this.y + Math.sin(angle)*this.radius;
            if (i === 0) ctx.moveTo(px, py); else ctx.lineTo(px, py);
        }
        ctx.closePath();
        ctx.shadowBlur = 10; ctx.shadowColor = this.color;
        ctx.fillStyle = '#050507'; ctx.fill();
        ctx.strokeStyle = this.color; ctx.lineWidth = this.type === 'tank' ? 3 : 1.5; ctx.stroke();

        // شريط صحة للأعداء ذوي الصحة العالية
        if (this.health > 2) {
            const maxH = this.type === 'tank' ? (5 + Math.floor(level/2)) : (2 + Math.floor(level/3));
            const bw = this.radius * 2;
            ctx.fillStyle = 'rgba(150,0,0,0.4)';
            ctx.fillRect(this.x - this.radius, this.y - this.radius - 7, bw, 3);
            ctx.fillStyle = '#ff3333';
            ctx.fillRect(this.x - this.radius, this.y - this.radius - 7, bw * (this.health/maxH), 3);
        }
    }
    update() {
        this.y += this.speed;
        this.x += Math.sin(this.y * 0.04) * (this.type === 'fast' ? 1.8 : 0.8);

        if (isPlaying && !isShopOpen && !inCutscene) {
            if (this.type !== 'fast') {
                this.shootCooldown--;
                if (this.shootCooldown <= 0) {
                    if (this.type === 'sniper') {
                        // سنايبر: يصوّب نحو اللاعب
                        const ang = Math.atan2(player.y - this.y, player.x - this.x) * 180 / Math.PI;
                        enemyProjectiles.push(new Projectile(this.x, this.y + this.radius, ang, '#3366ff', true));
                    } else {
                        enemyProjectiles.push(new Projectile(this.x, this.y + this.radius, 90, '#cc1100', true));
                    }
                    this.shootCooldown = this.type === 'sniper'
                        ? Math.random() * 60 + 45
                        : Math.random() * 120 + 100;
                }
            }
        }
        this.draw();
    }
}

class Particle {
    constructor(x, y, color, friction = 0.97, isSpark = false) {
        this.x = x; this.y = y;
        this.radius = isSpark ? Math.random()*2+1 : Math.random()*3+0.5;
        this.color = color;
        const angle = Math.random() * Math.PI * 2;
        const speed = isSpark ? Math.random()*5+2 : Math.random()*3+0.5;
        this.velocity = { x: Math.cos(angle)*speed, y: Math.sin(angle)*speed };
        this.alpha = 1; this.friction = friction; this.isSpark = isSpark;
    }
    draw() {
        ctx.save(); ctx.globalAlpha = this.alpha; ctx.fillStyle = this.color;
        if (this.isSpark) { ctx.shadowBlur = 10; ctx.shadowColor = '#fff'; }
        ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2); ctx.fill(); ctx.restore();
    }
    update() {
        this.velocity.x *= this.friction; this.velocity.y *= this.friction;
        this.x += this.velocity.x; this.y += this.velocity.y;
        this.alpha -= this.isSpark ? 0.04 : 0.02;
        this.draw();
    }
}

class PowerUp {
    constructor(x, y, forcedType) {
        this.x = x; this.y = y; this.radius = 12; this.speed = 1.2;
        const types = ['double', 'spread', 'shield', 'health', 'laser'];
        this.type = forcedType || types[Math.floor(Math.random() * types.length)];
        const icons = { health: '❤', shield: '🛡', double: '❙❙', spread: '↟', laser: '≡' };
        this.icon = icons[this.type] || 'E';
    }
    draw() {
        ctx.save(); ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2);
        ctx.fillStyle   = (frames % 30 < 15) ? '#5a1111' : '#2a2a30';
        ctx.lineWidth   = 1.5; ctx.strokeStyle = '#ff3333'; ctx.fill(); ctx.stroke();
        ctx.fillStyle   = '#ddd'; ctx.font = '10px Arial'; ctx.textAlign = 'center';
        ctx.textBaseline = 'middle'; ctx.fillText(this.icon, this.x, this.y);
        ctx.restore();
    }
    update() {
        this.y += this.speed;
        if (frames % 12 === 0) particles.push(new Particle(this.x, this.y, 'rgba(100,20,20,0.3)', 0.98));
        this.draw();
    }
}

// ═══════════════════════════════════════════════════════
//  رسم الخلفيات
// ═══════════════════════════════════════════════════════
function drawDeepCosmicBackground() {
    if (inVoidEnd) return;
    nebulaAngle += 0.001; ctx.save();
    const cx = canvas.width/2 + Math.sin(nebulaAngle)*100;
    const cy = canvas.height/3 + Math.cos(nebulaAngle)*80;
    const pNebula = ctx.createRadialGradient(cx, cy, 50, cx, cy, canvas.width*0.8);
    pNebula.addColorStop(0, 'rgba(30,10,45,0.15)');
    pNebula.addColorStop(0.5, 'rgba(15,5,25,0.08)');
    pNebula.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = pNebula; ctx.fillRect(0, 0, canvas.width, canvas.height);
    const rNebula = ctx.createRadialGradient(canvas.width-150, canvas.height/2, 20, canvas.width-100, canvas.height/2, canvas.height*0.6);
    rNebula.addColorStop(0, 'rgba(50,5,10,0.12)');
    rNebula.addColorStop(0.6, 'rgba(20,2,4,0.05)');
    rNebula.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = rNebula; ctx.fillRect(0, 0, canvas.width, canvas.height); ctx.restore();
}

function drawDyingEarth() {
    if (inVoidEnd) return;
    ctx.save();
    const ex = canvas.width - 140, ey = 150, er = 75;
    let df = Math.min(1, (level-1)/6);
    if (inCutscene) df = Math.min(0.15, cutsceneTimer/400);

    ctx.beginPath(); ctx.arc(ex, ey, er, 0, Math.PI*2);
    ctx.fillStyle = '#02040c'; ctx.fill();

    ctx.save(); ctx.beginPath(); ctx.arc(ex, ey, er, 0, Math.PI*2); ctx.clip();
    ctx.fillStyle = `rgba(10,40,110,${0.4*(1-df)})`; ctx.fillRect(ex-er, ey-er, er*2, er*2);
    ctx.fillStyle = `rgba(35,75,40,${0.45*(1-df)})`;
    ctx.beginPath();
    ctx.moveTo(ex-25, ey-5); ctx.lineTo(ex+5, ey-5); ctx.lineTo(ex+15, ey+20);
    ctx.lineTo(ex-5, ey+50); ctx.lineTo(ex-20, ey+45); ctx.lineTo(ex-30, ey+15);
    ctx.closePath(); ctx.fill();
    ctx.beginPath();
    ctx.moveTo(ex+5, ey-12); ctx.lineTo(ex+22, ey-8); ctx.lineTo(ex+26, ey+5);
    ctx.lineTo(ex+12, ey+12); ctx.lineTo(ex+4, ey+2); ctx.closePath(); ctx.fill();

    if (df < 0.7) {
        ctx.fillStyle = `rgba(255,215,0,${0.7*(1-df*1.4)})`;
        for (let lt of [{dx:-15,dy:-5},{dx:-5,dy:10},{dx:12,dy:-4},{dx:18,dy:3}])
            ctx.fillRect(ex+lt.dx, ey+lt.dy, 1.5, 1.5);
    }
    if (df > 0) {
        ctx.fillStyle = `rgba(18,18,20,${df*0.98})`;
        ctx.fillRect(ex-er, ey-er, er*2, er*2);
    }
    ctx.restore();
    ctx.strokeStyle = 'rgba(60,60,65,0.15)'; ctx.stroke(); ctx.restore();
}

// ═══════════════════════════════════════════════════════
//  إدارة النجوم والحطام
// ═══════════════════════════════════════════════════════
function initStars() {
    stars = [];
    // ✅ إصلاح #11: عدد نجوم أكبر
    for (let i = 0; i < 160; i++) stars.push(new Star(true));
}

function handleStars() {
    for (let i = stars.length - 1; i >= 0; i--) {
        const s = stars[i]; s.update();
        if (s.y > canvas.height) { stars.splice(i, 1); stars.push(new Star(false)); }
    }
}

function handleDebris() {
    if (Math.random() < 0.005 && debris.length < 5 && isPlaying && !isShopOpen && !inCutscene && !inVoidEnd)
        debris.push(new Debris());
    for (let i = debris.length - 1; i >= 0; i--) {
        const d = debris[i]; d.update();
        if (d.y > canvas.height) debris.splice(i, 1);
    }
}

// ═══════════════════════════════════════════════════════
//  نصوص القصة
// ═══════════════════════════════════════════════════════
function getStoryLine(lvl) {
    const logs = {
        1: "[الذكاء الاصطناعي]: تم تحييد الرائد الأول... رصدنا وميضاً حرارياً هائلاً من جهة الأرض. الغلاف الجوي يحترق... لقد بدأ هجومهم الشامل على موطننا. نحن متأخرون جداً.",
        2: "[قائد القاعدة الأرضية -- صوت مشوش]: أيها الطيار... النصف الشرقي خرج عن الخدمة بالكامل... انهار خط الدفاع الجوي الأرضي... لا تحاول العودة.",
        3: "[الذكاء الاصطناعي]: انقطع اتصال القاعدة المركزية بالأرض تماماً. الإشارات الحيوية للبشرية تنخفض بنسبة 74%. لم نعد نحارب من أجل الإنقاذ... نحن نحارب من أجل الثأر.",
        4: "[إشعار طوارئ]: رصد سفينة القيادة الحليفة U.S.S Vanguard! الأنظمة لا تستجيب... تم تعديلها من العدو! إنها جثة ميكانيكية تهاجمنا الآن!",
        5: "[صوت طفلة مجهول]: هل هناك أحد في السماء؟ السماء هنا سوداء جداً... أبي نام ولم يستيقظ... [الذكاء الاصطناعي]: تم حظر التردد. استعد للزعيم السادس.",
        6: "[الذكاء الاصطناعي]: العداد البشري: صفر. تم تأكيد انقراض الجنس البشري بالكامل. أنت الكائن الحي الأخير. الوحش النهائي 'أوميجا الأبيض' يقف أمامك الآن.",
    };
    return logs[lvl] || "[الذكاء الاصطناعي]: صمت كوني مطبق... لا توجد أي إشارات في المدى العام.";
}

// ═══════════════════════════════════════════════════════
//  تأثيرات واجهة المستخدم
// ═══════════════════════════════════════════════════════
function runTypewriterEffect(fullText, element, speed = 35, callback = null) {
    clearInterval(textTypeInterval); element.innerHTML = ""; let index = 0;
    textTypeInterval = setInterval(() => {
        if (index < fullText.length) {
            element.innerHTML = fullText.substring(0, index+1) + '<span class="cursor-blink"></span>';
            index++;
        } else {
            element.innerHTML = fullText; clearInterval(textTypeInterval);
            if (callback) callback();
        }
    }, speed);
}

function triggerCameraShake(duration) { shakeTimer = duration; }

function processCameraShake() {
    if (shakeTimer > 0) {
        shakeTimer--;
        const sx = Math.random()*10-5, sy = Math.random()*10-5;
        gameContainer.style.transform = `translate(${sx}px,${sy}px)`;
        if (!inVoidEnd) { hudLeft.classList.add('hud-glitch'); hudRight.classList.add('hud-glitch'); }
    } else {
        gameContainer.style.transform = 'translate(0px,0px)';
        hudLeft.classList.remove('hud-glitch'); hudRight.classList.remove('hud-glitch');
    }
}

function updateHUD() {
    scoreEl.textContent = score;
    popEl.textContent   = population.toLocaleString();
    levelEl.textContent = level;
    // تحديث شريط التقدم
    const pct = Math.min(100, Math.floor((score / scoreForNextLevel) * 100));
    progressFill.style.width = pct + '%';
    progressLabel.textContent = boss ? '⚠️ معركة البوس' : `تقدم المرحلة ${pct}%`;
}

// ═══════════════════════════════════════════════════════
//  المتجر
// ═══════════════════════════════════════════════════════
function openShop() {
    isShopOpen = true; hud.classList.add('hidden');
    shopScreen.classList.remove('hidden'); mouse.isDown = false;
    runTypewriterEffect(getStoryLine(level), storyLog);

    const choices = [
        { text: `🔴 مكثف النبض (+15 ذخيرة قصوى، +5 ذخيرة حالية) ⚡ العيب: سرعة حركة -10%`,
          action: () => { player.maxAmmo += 15; player.ammo = Math.min(player.maxAmmo, player.ammo+5); player.speed = Math.max(0.04, player.speed-0.013); } },
        { text: `🔴 خلايا نانوية (إصلاح ذاتي كل 1.5 ثانية +2 نقطة) ⚡ العيب: سرعة الرصاص -15%`,
          action: () => { player.autoHeal = true; player.projectileSpeedBonus -= 1.5; player.health = player.maxHealth; } },
        { text: `🔴 تدريع التيتانيوم (+35 هيكل أقصى + إصلاح فوري) ⚡ العيب: زمن التعشيق +20 فريم`,
          action: () => { player.maxHealth += 35; player.health = player.maxHealth; player.reloadDuration += 20; } },
        { text: `🔴 مكثف الدفع الناري (رصاص أسرع +بدء المرحلة التالية بدرع) ⚡ العيب: لا مغناطيس قوة`,
          action: () => { player.shootCooldownDuration = Math.max(4, player.shootCooldownDuration-2); player.projectileSpeedBonus += 3; player.magnetRadius = 0; player.powerUpType = 'shield'; player.powerUpTimer = 400; } },
        { text: `🔴 نظام الانتشار المزدوج (فتح قوة الليزر كقوة دائمة) ⚡ العيب: ذخيرة قصوى -5`,
          action: () => { player.powerUpType = 'laser'; player.powerUpTimer = 9999; player.maxAmmo = Math.max(10, player.maxAmmo-5); player.ammo = Math.min(player.ammo, player.maxAmmo); } },
    ];

    // اختيار خيارَين عشوائيَّين مختلفَين
    let c1 = choices[Math.floor(Math.random()*choices.length)];
    let c2 = choices[Math.floor(Math.random()*choices.length)];
    while (c2.text === c1.text) c2 = choices[Math.floor(Math.random()*choices.length)];

    shopOpt1.textContent = c1.text; shopOpt1.onclick = () => { c1.action(); closeShop(); };
    shopOpt2.textContent = c2.text; shopOpt2.onclick = () => { c2.action(); closeShop(); };
}

function closeShop() {
    clearInterval(textTypeInterval); shopScreen.classList.add('hidden');
    // ✅ إصلاح #5: 7 مراحل حقيقية (level 7 آخر مرحلة)
    if (level >= 7) { triggerWhiteVoidEnd(); return; }
    hud.classList.remove('hidden'); isShopOpen = false;
    level++;
    // ✅ إصلاح #4/#15: عتبة المراحل تزيد بشكل تدريجي وليس ثابتاً
    scoreForNextLevel += 400 + (level * 80);
    updateHUD();
    comboCount = 1; comboTimer = 0; // إعادة ضبط الكومبو بين المراحل
}

// ═══════════════════════════════════════════════════════
//  نهاية الفراغ الأبيض
// ═══════════════════════════════════════════════════════
function triggerWhiteVoidEnd() {
    inVoidEnd = true; isShopOpen = false; hud.classList.add('hidden');
    projectiles = []; enemyProjectiles = []; enemies = []; boss = null;
    whiteVoidScreen.classList.remove('hidden'); restartFromVoidBtn.classList.add('hidden');
    const voidMessage = "لقد دمرت 'الأوميجا الأبيض'.. انطفأت الترددات المعادية. لكن لم يعد هناك قاعدة أرضية لتستقبلك.. ولم يعد هناك بشر ليروا هذا الانتصار الكوني. العداد البشري: صفر. الأكسجين المتبقي في مقصورتك: 0.02%. جاري إغلاق أنظمة الدعم الحيوية بشكل كامل.. شكراً لخدماتك أيها الطيار الأخير.";
    runTypewriterEffect(voidMessage, voidLog, 50, () => { restartFromVoidBtn.classList.remove('hidden'); });
}

// ═══════════════════════════════════════════════════════
//  المقطع السينمائي
// ═══════════════════════════════════════════════════════
function handleCutscene() {
    cutsceneTimer++;
    if (cutsceneTimer === 80 || cutsceneTimer === 160 || cutsceneTimer === 240) triggerCameraShake(20);
    if (cutsceneTimer >= 300) { inCutscene = false; hud.classList.remove('hidden'); updateHUD(); }
}

// ═══════════════════════════════════════════════════════
//  حلقة الرسوميات الرئيسية
// ═══════════════════════════════════════════════════════
function animate() {
    if (!isPlaying) return;
    animationId = requestAnimationFrame(animate);

    // مسح الشاشة
    if (inVoidEnd) {
        ctx.fillStyle = '#ffffff'; ctx.fillRect(0, 0, canvas.width, canvas.height);
    } else {
        ctx.fillStyle = 'rgba(2,2,6,0.42)'; ctx.fillRect(0, 0, canvas.width, canvas.height);
    }

    drawDeepCosmicBackground();
    handleStars();
    drawDyingEarth();
    handleDebris();
    updateCombo();

    // ─── مقطع سينمائي ───
    if (inCutscene) {
        handleCutscene(); player.update(); processCameraShake();
        for (let i = particles.length-1; i >= 0; i--) {
            const p = particles[i]; if (p.alpha <= 0) particles.splice(i, 1); else p.update();
        }
        return;
    }

    if (inVoidEnd) { frames++; player.update(); processCameraShake(); return; }
    if (isShopOpen) return;

    frames++;
    player.update();
    processCameraShake();

    // ─── ظهور بوس عند الوصول للعتبة ───
    if (score >= scoreForNextLevel && !boss) {
        boss = new BigBoss(level); enemies = [];
        triggerCameraShake(30);
        showFlash(`⚠️ تحذير: ظهر زعيم المرحلة ${level}!`, 150);
    }

    // ─── تحديث البوس ───
    if (boss) {
        boss.update();
        const d = Math.hypot(player.x - boss.x, player.y - boss.y);
        if (d - player.radius - boss.radius < 0) {
            const died = player.takeDamage(2.5);
            triggerCameraShake(6); updateHUD();
            if (died) { endGame(); return; }
        }
    }

    // ─── توليد الأعداء ───
    // ✅ إصلاح #2: جميع المراحل تولّد أعداء، بعضها أكثر إثناء البوس
    const shouldSpawn = !boss || [2, 3, 5, 6, 7].includes(level) || (boss && level >= 4);
    if (shouldSpawn) {
        const rateDivider   = boss ? 30 : 18;
        const currentRate   = Math.max(380, 1400 - level * 80);
        if (frames % Math.floor(currentRate / rateDivider) === 0) {
            const r = 25;
            enemies.push(new Enemy(Math.random()*(canvas.width - r*2) + r, -r));
        }
    }

    // ─── جزيئات ───
    for (let i = particles.length-1; i >= 0; i--) {
        const p = particles[i]; if (p.alpha <= 0) particles.splice(i, 1); else p.update();
    }

    // ─── رصاص اللاعب ───
    for (let i = projectiles.length-1; i >= 0; i--) {
        const proj = projectiles[i]; proj.update();
        if (proj.y + proj.radius < 0 || proj.x < 0 || proj.x > canvas.width) {
            projectiles.splice(i, 1); continue;
        }
        // اصطدام بالبوس
        if (boss) {
            const d = Math.hypot(proj.x - boss.x, proj.y - boss.y);
            if (d - proj.radius - boss.radius < 0) {
                projectiles.splice(i, 1); boss.health--;
                createExplosion(proj.x, proj.y, boss.color);
                if (boss.health <= 0) {
                    score += boss.points * comboCount;
                    createExplosion(boss.x, boss.y, '#ffffff', true);
                    triggerCameraShake(40);
                    boss = null; enemyProjectiles = []; updateHUD(); openShop();
                }
                continue;
            }
        }
    }

    // ─── رصاص الأعداء ───
    for (let i = enemyProjectiles.length-1; i >= 0; i--) {
        const ep = enemyProjectiles[i]; ep.update();
        if (ep.y > canvas.height || ep.y < -20 || ep.x < -20 || ep.x > canvas.width+20) {
            enemyProjectiles.splice(i, 1); continue;
        }
        const d = Math.hypot(player.x - ep.x, player.y - ep.y);
        if (d - player.radius - ep.radius < 0) {
            const dmg = ep.isRPG ? 22 : 12; // ✅ تسهيل: ضرر أقل
            enemyProjectiles.splice(i, 1);
            const died = player.takeDamage(dmg);
            triggerCameraShake(ep.isRPG ? 12 : 5);
            createExplosion(ep.x, ep.y, ep.color);
            if (died) { endGame(); return; }
        }
    }

    // ─── قوى التعزيز ───
    for (let i = powerUps.length-1; i >= 0; i--) {
        const pu = powerUps[i];
        const d  = Math.hypot(player.x - pu.x, player.y - pu.y);

        // ✅ إصلاح #3 و#14: رسم PowerUp دائماً، سواء كان يُسحب أم لا
        if (player.magnetRadius > 0 && d < player.magnetRadius * 3) {
            const ang = Math.atan2(player.y - pu.y, player.x - pu.x);
            pu.x += Math.cos(ang) * 5; pu.y += Math.sin(ang) * 5;
            pu.draw(); // رسم صريح عند السحب
        } else {
            pu.update(); // update تشمل draw
        }

        if (d - player.radius - pu.radius < 0) {
            if (pu.type === 'health') { player.health = Math.min(player.maxHealth, player.health + 35); }
            else { player.powerUpType = pu.type; player.powerUpTimer = 650; }
            updateHUD(); powerUps.splice(i, 1); continue;
        }
        if (pu.y > canvas.height) powerUps.splice(i, 1);
    }

    // ─── الأعداء ───
    for (let i = enemies.length-1; i >= 0; i--) {
        const enemy = enemies[i]; enemy.update();

        // اصطدام بالرصاص
        let hitByProjectile = false;
        for (let j = projectiles.length-1; j >= 0; j--) {
            const proj = projectiles[j];
            if (!enemies[i]) break; // ✅ حراسة: العدو قد يكون حُذف
            const dp = Math.hypot(proj.x - enemy.x, proj.y - enemy.y);
            if (dp - proj.radius - enemy.radius < 0) {
                projectiles.splice(j, 1); enemy.health--;
                createExplosion(proj.x, proj.y, enemy.color);
                if (enemy.health <= 0) {
                    score += enemy.points * comboCount;
                    addKill();
                    createExplosion(enemy.x, enemy.y, '#ffffff');
                    // ✅ تحسين: احتمال إسقاط قوة تعزيز أكبر
                    const dropRoll = Math.random();
                    if (dropRoll < 0.14) powerUps.push(new PowerUp(enemy.x, enemy.y));
                    else if (dropRoll < 0.16 && player.health < player.maxHealth * 0.4)
                        powerUps.push(new PowerUp(enemy.x, enemy.y, 'health')); // ❤ ضمان عند خطر
                    enemies.splice(i, 1); updateHUD();
                    hitByProjectile = true;
                }
                break;
            }
        }
        if (hitByProjectile) continue;
        if (!enemies[i]) continue;

        // اصطدام باللاعب
        const dp2 = Math.hypot(player.x - enemy.x, player.y - enemy.y);
        if (dp2 - player.radius - enemy.radius < 0) {
            const dmg = enemy.type === 'tank' ? 25 : 12; // ✅ تسهيل: ضرر أقل
            const died = player.takeDamage(dmg);
            triggerCameraShake(15);
            createExplosion(enemy.x, enemy.y, enemy.color);
            enemies.splice(i, 1); updateHUD();
            if (died) { endGame(); return; }
            continue;
        }

        // خروج من الشاشة
        if (enemy.y - enemy.radius > canvas.height) {
            enemies.splice(i, 1);
            const casualties = Math.floor(Math.random()*25000 + 20000);
            population = Math.max(0, population - casualties);
            triggerCameraShake(5); updateHUD();
        }
    }
}

// ═══════════════════════════════════════════════════════
//  بدء اللعبة وإنهاؤها
// ═══════════════════════════════════════════════════════
function startCutsceneSequence() {
    clearInterval(textTypeInterval);
    cancelAnimationFrame(animationId);
    [startScreen, gameOverScreen, shopScreen, whiteVoidScreen].forEach(s => s.classList.add('hidden'));

    player = new Player();
    projectiles = []; enemyProjectiles = []; enemies = [];
    particles = []; powerUps = []; debris = [];
    score = 0; population = 2_400_000; level = 1; frames = 0;
    scoreForNextLevel = 600; boss = null;
    comboCount = 1; comboTimer = 0;

    isPlaying = true; isShopOpen = false; inCutscene = true; inVoidEnd = false; cutsceneTimer = 0;
    initStars(); animate();
}

function endGame() {
    isPlaying = false; clearInterval(textTypeInterval); cancelAnimationFrame(animationId);
    finalScoreEl.textContent = score; hud.classList.add('hidden');
    const txt = population <= 0
        ? "تحطمت سفينتك.. لكن كوكب الأرض مات قبلك بالفعل. لقد انقرض الجنس البشري بسبب عجزك عن حمايتهم."
        : "تحطمت السفينة وتحول جسدك إلى غبار كوني.. مات الطيار الأخير، وسقطت الأرض بعدك بلحظات.";
    document.getElementById('gameOverText').textContent = txt;
    gameOverScreen.classList.remove('hidden');
}

// ═══════════════════════════════════════════════════════
//  ربط الأزرار
// ═══════════════════════════════════════════════════════
startBtn.addEventListener('click', startCutsceneSequence);
restartBtn.addEventListener('click', startCutsceneSequence);
restartFromVoidBtn.addEventListener('click', startCutsceneSequence);

// ─── تهيئة أولية ───
initStars();
ctx.fillStyle = '#020205'; ctx.fillRect(0, 0, canvas.width, canvas.height);
for (const s of stars) s.draw();
</script>
</body>
</html>

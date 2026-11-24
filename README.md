# -<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>星語占卜｜每日星座運勢查詢</title>
    <!-- 引入 Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- 引入 Google Fonts: Noto Serif TC 與 Noto Sans TC 營造神秘典雅感 -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@500;700&family=Noto+Sans+TC:wght@300;400;500;700&family=Noto+Serif+TC:wght@500;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Noto Sans TC"', 'sans-serif'],
                        serif: ['"Noto Serif TC"', 'serif'],
                        display: ['"Cinzel"', '"Noto Serif TC"', 'serif'],
                    },
                    colors: {
                        cosmos: {
                            950: '#05040d',
                            900: '#0b091e',
                            800: '#151236',
                            700: '#231d54',
                        },
                        starlight: {
                            gold: '#f5d061',
                            amber: '#e0a938',
                            soft: '#fdf3d1',
                        }
                    },
                    animation: {
                        'float': 'float 6s ease-in-out infinite',
                        'pulse-slow': 'pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'spin-slow': 'spin 24s linear infinite',
                    },
                    keyframes: {
                        float: {
                            '0%, 100%': { transform: 'translateY(0px)' },
                            '50%': { transform: 'translateY(-8px)' },
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-gradient-to-b from-cosmos-950 via-cosmos-900 to-cosmos-800 text-slate-100 min-h-screen relative overflow-x-hidden selection:bg-amber-400/30 selection:text-amber-200">

    <!-- 動態星空背景 Canvas -->
    <canvas id="starfield" class="fixed inset-0 w-full h-full pointer-events-none z-0"></canvas>

    <!-- 背景神秘星雲光暈裝飾 -->
    <div class="fixed top-[-10%] left-[-10%] w-[50vw] h-[50vw] rounded-full bg-indigo-900/20 blur-[120px] pointer-events-none z-0"></div>
    <div class="fixed bottom-[-10%] right-[-10%] w-[55vw] h-[55vw] rounded-full bg-purple-900/20 blur-[130px] pointer-events-none z-0"></div>

    <!-- 右上角固定顯示「41陳」專屬標記 -->
    <header class="fixed top-4 right-4 sm:top-6 sm:right-6 z-50">
        <div class="group relative flex items-center gap-2 px-4 py-2 rounded-full bg-slate-900/70 backdrop-blur-md border border-amber-400/40 shadow-[0_0_20px_rgba(245,208,97,0.25)] transition-all duration-300 hover:border-amber-300 hover:shadow-[0_0_28px_rgba(245,208,97,0.45)]">
            <span class="relative flex h-2.5 w-2.5">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-amber-300 opacity-75"></span>
                <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-amber-400"></span>
            </span>
            <span class="font-serif font-bold tracking-widest text-sm sm:text-base bg-gradient-to-r from-amber-200 via-yellow-300 to-amber-400 bg-clip-text text-transparent">
                41陳
            </span>
        </div>
    </header>

    <!-- 主內容區塊 -->
    <main class="relative z-10 max-w-4xl mx-auto px-4 sm:px-6 pt-16 pb-20 sm:pt-20">
        
        <!-- 網站標題與星象裝飾 -->
        <section class="text-center mb-10 sm:mb-12">
            <div class="inline-flex items-center justify-center w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-indigo-950/60 border border-amber-400/30 shadow-[0_0_30px_rgba(129,140,248,0.25)] mb-4 animate-float">
                <!-- 星盤 SVG 圖示 -->
                <svg class="w-9 h-9 sm:w-11 sm:h-11 text-amber-300 animate-spin-slow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.3">
                    <circle cx="12" cy="12" r="9" stroke-dasharray="2 2"/>
                    <circle cx="12" cy="12" r="5"/>
                    <path d="M12 1v2M12 21v2M1 12h2M21 12h2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"/>
                    <polygon points="12,7 13.5,10.5 17,12 13.5,13.5 12,17 10.5,13.5 7,12 10.5,10.5" fill="currentColor" fill-opacity="0.3"/>
                </svg>
            </div>
            
            <p class="text-amber-300/80 tracking-[0.25em] text-xs sm:text-sm uppercase font-display mb-2">
                CELESTIAL ASTROLOGY ORACLE
            </p>
            <h1 class="font-serif text-3xl sm:text-5xl font-bold tracking-wider bg-gradient-to-b from-white via-amber-100 to-amber-300/90 bg-clip-text text-transparent drop-shadow-sm mb-3">
                星語秘境・每日星座運勢
            </h1>
            <p id="today-date-display" class="text-indigo-200/80 text-sm sm:text-base font-light tracking-wide">
                正在同步星象曆法...
            </p>
        </section>

        <!-- 生日輸入玻璃擬態卡片 -->
        <section class="rounded-3xl bg-white/[0.06] backdrop-blur-xl border border-white/15 shadow-[0_8px_32px_0_rgba(0,0,0,0.45)] p-6 sm:p-8 mb-8 transition-all duration-500">
            <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                
                <!-- 提示文字 -->
                <div class="text-center md:text-left md:w-5/12">
                    <h2 class="font-serif text-xl sm:text-2xl font-semibold text-amber-200 mb-1.5 flex items-center justify-center md:justify-start gap-2">
                        <span>✦</span> 探索您的專屬星盤
                    </h2>
                    <p class="text-slate-300/80 text-sm leading-relaxed">
                        請選擇您的出生月日，星象儀將為您解析今日宇宙能量與幸運指引。
                    </p>
                </div>

                <!-- 生日下拉選單與查詢按鈕 -->
                <div class="w-full md:w-7/12 flex flex-col sm:flex-row items-stretch sm:items-center gap-3">
                    <div class="grid grid-cols-3 gap-2 flex-1">
                        <!-- 出生年份 (選填/輔助個人化能量) -->
                        <div class="relative">
                            <select id="birth-year" aria-label="出生年份" class="w-full appearance-none rounded-2xl bg-cosmos-950/75 border border-indigo-400/30 text-slate-100 px-3 py-3.5 text-sm sm:text-base focus:outline-none focus:border-amber-400 focus:ring-2 focus:ring-amber-400/20 transition cursor-pointer">
                                <!-- JS 動態填入年份 -->
                            </select>
                            <span class="pointer-events-none absolute right-2.5 top-1/2 -translate-y-1/2 text-amber-300/70 text-xs">▼</span>
                        </div>

                        <!-- 出生月份 -->
                        <div class="relative">
                            <select id="birth-month" aria-label="出生月份" class="w-full appearance-none rounded-2xl bg-cosmos-950/75 border border-indigo-400/30 text-slate-100 px-3 py-3.5 text-sm sm:text-base focus:outline-none focus:border-amber-400 focus:ring-2 focus:ring-amber-400/20 transition cursor-pointer">
                                <option value="" disabled selected>月份</option>
                            </select>
                            <span class="pointer-events-none absolute right-2.5 top-1/2 -translate-y-1/2 text-amber-300/70 text-xs">▼</span>
                        </div>

                        <!-- 出生日期 -->
                        <div class="relative">
                            <select id="birth-day" aria-label="出生日期" class="w-full appearance-none rounded-2xl bg-cosmos-950/75 border border-indigo-400/30 text-slate-100 px-3 py-3.5 text-sm sm:text-base focus:outline-none focus:border-amber-400 focus:ring-2 focus:ring-amber-400/20 transition cursor-pointer">
                                <option value="" disabled selected>日期</option>
                            </select>
                            <span class="pointer-events-none absolute right-2.5 top-1/2 -translate-y-1/2 text-amber-300/70 text-xs">▼</span>
                        </div>
                    </div>

                    <!-- 查詢按鈕 -->
                    <button id="predict-btn" type="button" class="rounded-2xl px-6 py-3.5 font-medium text-cosmos-950 bg-gradient-to-r from-amber-300 via-yellow-400 to-amber-500 hover:from-amber-200 hover:to-amber-400 active:scale-[0.98] shadow-[0_0_20px_rgba(245,208,97,0.35)] hover:shadow-[0_0_28px_rgba(245,208,97,0.55)] transition-all duration-200 whitespace-nowrap flex items-center justify-center gap-2 cursor-pointer">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"/>
                        </svg>
                        <span>查詢今日運勢</span>
                    </button>
                </div>
            </div>

            <!-- 內建柔和提示訊息框 (取代 alert) -->
            <div id="message-box" class="hidden mt-4 rounded-xl bg-rose-500/15 border border-rose-400/40 px-4 py-2.5 text-rose-200 text-sm text-center transition-all">
                請先選擇完整的出生月份與日期喔！
            </div>
        </section>

        <!-- 運勢結果顯示區 (預設隱藏，查詢後平滑展現) -->
        <section id="result-section" class="hidden opacity-0 translate-y-4 transition-all duration-700 space-y-6">
            
            <!-- 1. 星座主視覺與整體運勢卡片 -->
            <div class="rounded-3xl bg-gradient-to-br from-white/[0.09] to-indigo-950/40 backdrop-blur-xl border border-amber-300/25 shadow-[0_12px_40px_rgba(0,0,0,0.5)] p-6 sm:p-8 relative overflow-hidden">
                <!-- 右側背景巨大星座符號浮水印 -->
                <div id="zodiac-watermark" class="pointer-events-none select-none absolute -right-4 -bottom-8 text-[140px] sm:text-[180px] leading-none text-amber-300/[0.06] font-serif">
                    ♈
                </div>

                <div class="relative z-10 flex flex-col sm:flex-row items-center sm:items-start gap-6">
                    <!-- 星座徽章 -->
                    <div class="flex-shrink-0 w-24 h-24 sm:w-28 sm:h-28 rounded-3xl bg-gradient-to-b from-amber-400/20 to-indigo-900/50 border border-amber-300/40 shadow-[0_0_25px_rgba(245,208,97,0.2)] flex flex-col items-center justify-center">
                        <span id="zodiac-symbol" class="text-4xl sm:text-5xl text-amber-300 drop-shadow-[0_0_10px_rgba(245,208,97,0.5)]">♈</span>
                        <span id="zodiac-en" class="text-[10px] tracking-widest uppercase text-amber-200/70 mt-1 font-display">ARIES</span>
                    </div>

                    <!-- 星座名稱、日期、元素與整體星等 -->
                    <div class="flex-1 text-center sm:text-left">
                        <div class="flex flex-wrap items-center justify-center sm:justify-start gap-2.5 mb-2">
                            <h3 id="zodiac-name" class="font-serif text-2xl sm:text-3xl font-bold text-white tracking-wide">
                                牡羊座
                            </h3>
                            <span id="zodiac-date-range" class="px-3 py-1 rounded-full text-xs bg-white/10 text-indigo-200 border border-white/10">
                                03/21 - 04/19
                            </span>
                            <span id="zodiac-element" class="px-3 py-1 rounded-full text-xs font-medium bg-amber-400/15 text-amber-300 border border-amber-400/30">
                                🔥 火象星座
                            </span>
                        </div>

                        <!-- 整體運勢星級 -->
                        <div class="flex items-center justify-center sm:justify-start gap-3 mb-4">
                            <span class="text-sm text-slate-300">今日整體運勢：</span>
                            <div id="overall-stars" class="flex items-center gap-1 text-amber-400 text-lg">
                                <!-- JS 動態產生星星 -->
                            </div>
                            <span id="overall-score-badge" class="text-xs font-bold px-2 py-0.5 rounded-md bg-amber-400/20 text-amber-200">
                                92分
                            </span>
                        </div>

                        <!-- 整體總評 -->
                        <p id="overall-summary" class="text-slate-200/95 text-sm sm:text-base leading-relaxed bg-cosmos-950/40 rounded-2xl p-4 border border-white/10">
                            今日星象能量順暢，您的直覺與行動力達到絕佳平衡。適合主動推進心中的計畫，真誠的態度將為您吸引意想不到的好運與支持。
                        </p>
                    </div>
                </div>
            </div>

            <!-- 2. 三大維度運勢卡片 (愛情 / 事業學業 / 財富) -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 sm:gap-5">
                <!-- 愛情運勢 -->
                <div class="rounded-3xl bg-white/[0.06] backdrop-blur-xl border border-white/15 p-5 sm:p-6 flex flex-col justify-between hover:border-pink-400/30 transition-colors">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <div class="flex items-center gap-2">
                                <span class="w-8 h-8 rounded-xl bg-pink-500/20 border border-pink-400/30 flex items-center justify-center text-pink-300">♥</span>
                                <h4 class="font-serif font-semibold text-lg text-pink-200">愛情運勢</h4>
                            </div>
                            <span id="love-score-text" class="text-sm font-bold text-pink-300">85%</span>
                        </div>
                        <!-- 進度條 -->
                        <div class="w-full h-2 rounded-full bg-slate-800/80 overflow-hidden mb-4">
                            <div id="love-bar" class="h-full rounded-full bg-gradient-to-r from-pink-500 to-rose-300 transition-all duration-1000" style="width: 0%"></div>
                        </div>
                        <p id="love-desc" class="text-slate-300 text-sm leading-relaxed">
                            運勢解析載入中...
                        </p>
                    </div>
                </div>

                <!-- 事業與學業運勢 -->
                <div class="rounded-3xl bg-white/[0.06] backdrop-blur-xl border border-white/15 p-5 sm:p-6 flex flex-col justify-between hover:border-sky-400/30 transition-colors">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <div class="flex items-center gap-2">
                                <span class="w-8 h-8 rounded-xl bg-sky-500/20 border border-sky-400/30 flex items-center justify-center text-sky-300">✦</span>
                                <h4 class="font-serif font-semibold text-lg text-sky-200">事業學業</h4>
                            </div>
                            <span id="career-score-text" class="text-sm font-bold text-sky-300">88%</span>
                        </div>
                        <!-- 進度條 -->
                        <div class="w-full h-2 rounded-full bg-slate-800/80 overflow-hidden mb-4">
                            <div id="career-bar" class="h-full rounded-full bg-gradient-to-r from-sky-500 to-indigo-300 transition-all duration-1000" style="width: 0%"></div>
                        </div>
                        <p id="career-desc" class="text-slate-300 text-sm leading-relaxed">
                            運勢解析載入中...
                        </p>
                    </div>
                </div>

                <!-- 財富運勢 -->
                <div class="rounded-3xl bg-white/[0.06] backdrop-blur-xl border border-white/15 p-5 sm:p-6 flex flex-col justify-between hover:border-amber-400/30 transition-colors">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <div class="flex items-center gap-2">
                                <span class="w-8 h-8 rounded-xl bg-amber-500/20 border border-amber-400/30 flex items-center justify-center text-amber-300">◈</span>
                                <h4 class="font-serif font-semibold text-lg text-amber-200">財富運勢</h4>
                            </div>
                            <span id="wealth-score-text" class="text-sm font-bold text-amber-300">80%</span>
                        </div>
                        <!-- 進度條 -->
                        <div class="w-full h-2 rounded-full bg-slate-800/80 overflow-hidden mb-4">
                            <div id="wealth-bar" class="h-full rounded-full bg-gradient-to-r from-amber-500 to-yellow-300 transition-all duration-1000" style="width: 0%"></div>
                        </div>
                        <p id="wealth-desc" class="text-slate-300 text-sm leading-relaxed">
                            運勢解析載入中...
                        </p>
                    </div>
                </div>
            </div>

            <!-- 3. 今日幸運元素與開運小叮嚀 -->
            <div class="rounded-3xl bg-white/[0.06] backdrop-blur-xl border border-white/15 p-6 sm:p-7">
                <h4 class="font-serif text-lg font-semibold text-amber-200 mb-4 flex items-center gap-2">
                    <span>✧</span> 今日星象幸運指引
                </h4>

                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-5">
                    <!-- 幸運色 -->
                    <div class="rounded-2xl bg-cosmos-950/50 border border-white/10 p-4 flex items-center gap-3.5">
                        <span id="lucky-color-dot" class="w-10 h-10 rounded-full border-2 border-white/30 shadow-inner flex-shrink-0" style="background-color: #f59e0b;"></span>
                        <div>
                            <div class="text-xs text-slate-400">今日幸運色</div>
                            <div id="lucky-color-name" class="font-medium text-slate-100 text-base mt-0.5">琥珀暖金</div>
                        </div>
                    </div>

                    <!-- 幸運數字 -->
                    <div class="rounded-2xl bg-cosmos-950/50 border border-white/10 p-4 flex items-center gap-3.5">
                        <div class="w-10 h-10 rounded-full bg-indigo-500/20 border border-indigo-400/30 flex items-center justify-center font-display font-bold text-amber-300 text-lg flex-shrink-0">
                            #
                        </div>
                        <div>
                            <div class="text-xs text-slate-400">今日幸運數字</div>
                            <div id="lucky-number" class="font-display font-bold text-slate-100 text-base mt-0.5">7、19</div>
                        </div>
                    </div>

                    <!-- 貴人星座 -->
                    <div class="rounded-2xl bg-cosmos-950/50 border border-white/10 p-4 flex items-center gap-3.5">
                        <div id="benefactor-symbol" class="w-10 h-10 rounded-full bg-purple-500/20 border border-purple-400/30 flex items-center justify-center text-purple-200 text-xl flex-shrink-0">
                            ♐
                        </div>
                        <div>
                            <div class="text-xs text-slate-400">今日貴人星座</div>
                            <div id="benefactor-name" class="font-medium text-slate-100 text-base mt-0.5">射手座</div>
                        </div>
                    </div>
                </div>

                <!-- 開運小叮嚀 -->
                <div class="rounded-2xl bg-gradient-to-r from-amber-500/10 via-purple-500/10 to-indigo-500/10 border border-amber-300/25 p-4 flex items-start gap-3">
                    <span class="text-amber-300 text-xl leading-none mt-0.5">💡</span>
                    <div>
                        <span class="text-xs font-semibold uppercase tracking-wider text-amber-300 block mb-1">今日開運小叮嚀</span>
                        <p id="daily-advice" class="text-slate-200 text-sm leading-relaxed">
                            睡前花五分鐘靜心整理思緒，喝一杯溫熱的花草茶，有助於釋放白天的緊繃並迎來好夢。
                        </p>
                    </div>
                </div>
            </div>

        </section>

        <!-- 頁尾版權 -->
        <footer class="mt-14 text-center text-xs text-slate-400/70">
            <p>✦ 星象運勢僅供心靈啟發與日常參考，願星光指引您前行的每一步 ✦</p>
        </footer>
    </main>

    <script>
        /**
         * ==========================================
         * 1. 動態閃爍星空粒子背景 (Canvas)
         * ==========================================
         */
        function initStarfield() {
            const canvas = document.getElementById('starfield');
            const ctx = canvas.getContext('2d');
            let width, height, stars = [], shootingStars = [];

            function resize() {
                width = canvas.width = window.innerWidth;
                height = canvas.height = window.innerHeight;
                createStars();
            }

            function createStars() {
                stars = [];
                // 依據螢幕面積動態調整星星數量，兼顧效能與美感
                const count = Math.min(Math.floor((width * height) / 7500), 220);
                for (let i = 0; i < count; i++) {
                    stars.push({
                        x: Math.random() * width,
                        y: Math.random() * height,
                        radius: Math.random() * 1.6 + 0.3,
                        alpha: Math.random(),
                        alphaChange: (Math.random() * 0.015 + 0.004) * (Math.random() < 0.5 ? 1 : -1),
                        tint: Math.random() > 0.8 ? '#fde68a' : (Math.random() > 0.5 ? '#c7d2fe' : '#ffffff')
                    });
                }
            }

            function spawnShootingStar() {
                if (shootingStars.length < 2 && Math.random() < 0.008) {
                    shootingStars.push({
                        x: Math.random() * width * 0.8,
                        y: Math.random() * height * 0.4,
                        len: Math.random() * 80 + 50,
                        speed: Math.random() * 8 + 6,
                        alpha: 1
                    });
                }
            }

            function animate() {
                ctx.clearRect(0, 0, width, height);

                // 繪製閃爍恆星
                for (let s of stars) {
                    s.alpha += s.alphaChange;
                    if (s.alpha <= 0.15 || s.alpha >= 1) {
                        s.alphaChange = -s.alphaChange;
                    }
                    ctx.save();
                    ctx.globalAlpha = Math.max(0.1, Math.min(1, s.alpha));
                    ctx.fillStyle = s.tint;
                    ctx.beginPath();
                    ctx.arc(s.x, s.y, s.radius, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                }

                // 偶爾出現的流星
                spawnShootingStar();
                for (let i = shootingStars.length - 1; i >= 0; i--) {
                    let ss = shootingStars[i];
                    ss.x += ss.speed;
                    ss.y += ss.speed * 0.55;
                    ss.alpha -= 0.018;

                    if (ss.alpha <= 0) {
                        shootingStars.splice(i, 1);
                        continue;
                    }

                    const grad = ctx.createLinearGradient(ss.x, ss.y, ss.x - ss.len, ss.y - ss.len * 0.55);
                    grad.addColorStop(0, `rgba(253, 230, 138, ${ss.alpha})`);
                    grad.addColorStop(1, 'rgba(253, 230, 138, 0)');

                    ctx.strokeStyle = grad;
                    ctx.lineWidth = 1.5;
                    ctx.beginPath();
                    ctx.moveTo(ss.x, ss.y);
                    ctx.lineTo(ss.x - ss.len, ss.y - ss.len * 0.55);
                    ctx.stroke();
                }

                requestAnimationFrame(animate);
            }

            window.addEventListener('resize', resize);
            resize();
            animate();
        }

        /**
         * ==========================================
         * 2. 12 星座資料庫與運勢語料庫
         * ==========================================
         */
        const ZODIAC_SIGNS = [
            { id: 'capricorn', name: '摩羯座', en: 'CAPRICORN', symbol: '♑', range: '12/22 - 01/19', element: '⛰️ 土象星座', start: [12, 22], end: [1, 19] },
            { id: 'aquarius',  name: '水瓶座', en: 'AQUARIUS',  symbol: '♒', range: '01/20 - 02/18', element: '🌬️ 風象星座', start: [1, 20],  end: [2, 18] },
            { id: 'pisces',    name: '雙魚座', en: 'PISCES',    symbol: '♓', range: '02/19 - 03/20', element: '💧 水象星座', start: [2, 19],  end: [3, 20] },
            { id: 'aries',     name: '牡羊座', en: 'ARIES',     symbol: '♈', range: '03/21 - 04/19', element: '🔥 火象星座', start: [3, 21],  end: [4, 19] },
            { id: 'taurus',    name: '金牛座', en: 'TAURUS',    symbol: '♉', range: '04/20 - 05/20', element: '⛰️ 土象星座', start: [4, 20],  end: [5, 20] },
            { id: 'gemini',    name: '雙子座', en: 'GEMINI',    symbol: '♊', range: '05/21 - 06/21', element: '🌬️ 風象星座', start: [5, 21],  end: [6, 21] },
            { id: 'cancer',    name: '巨蟹座', en: 'CANCER',    symbol: '♋', range: '06/22 - 07/22', element: '💧 水象星座', start: [6, 22],  end: [7, 22] },
            { id: 'leo',       name: '獅子座', en: 'LEO',       symbol: '♌', range: '07/23 - 08/22', element: '🔥 火象星座', start: [7, 23],  end: [8, 22] },
            { id: 'virgo',     name: '處女座', en: 'VIRGO',     symbol: '♍', range: '08/23 - 09/22', element: '⛰️ 土象星座', start: [8, 23],  end: [9, 22] },
            { id: 'libra',     name: '天秤座', en: 'LIBRA',     symbol: '♎', range: '09/23 - 10/23', element: '🌬️ 風象星座', start: [9, 23],  end: [10, 23] },
            { id: 'scorpio',   name: '天蠍座', en: 'SCORPIO',   symbol: '♏', range: '10/24 - 11/22', element: '💧 水象星座', start: [10, 24], end: [11, 22] },
            { id: 'sagittarius', name: '射手座', en: 'SAGITTARIUS', symbol: '♐', range: '11/23 - 12/21', element: '🔥 火象星座', start: [11, 23], end: [12, 21] }
        ];

        const HOROSCOPE_POOL = {
            overall: [
                "今日星象呈現和諧三分相，您的思緒格外清晰，適合整理近期目標並勇敢邁出第一步，周遭環境也將給予正向回饋。",
                "宇宙能量溫柔地包覆著您，今天適合放慢腳步傾聽內心聲音。許多看似複雜的難題，只要換個角度就能迎刃而解。",
                "行動力與靈感同步提升的一天！您展現出的自信與真誠極具感染力，容易在團體中成為焦點，把握機會展現專長吧。",
                "星辰帶來穩健而踏實的磁場，適合處理累積已久的瑣事或深入研究感興趣的領域，您的細心將為自己贏得高度信任。",
                "人際互動充滿驚喜的一天，一通久違的訊息或偶遇的對話，都可能為您開啟全新的視野與合作契機。",
                "直覺力相當敏銳，面對選擇時不妨相信第一直覺。保持開放與彈性的心態，好運正悄悄在轉角處等著您。"
            ],
            love: [
                "單身者有機會在日常社交或學習場合遇見聊得來的對象；有伴侶者適合安排一場輕鬆的晚餐約會，分享彼此近況能讓感情升溫。",
                "情感表達宜溫柔而坦誠，適時給予對方讚美與傾聽，能化解先前的小誤會。單身者不妨多展現微笑，桃花運正在悄悄醞釀。",
                "浪漫指數攀升！無論是主動傳送關心訊息，還是準備一份小驚喜，都能讓心儀對象或伴侶感受到您滿滿的心意。",
                "今天在感情上更重視心靈契合與安全感，適合與伴侶聊聊未來的共同規劃；單身者先把重心放回愛自己，魅力自然散發。",
                "人際魅力十足，容易吸引欣賞您特質的人靠近。保持自然不造作的相處模式，緣分會在最不經意的時刻悄然降臨。"
            ],
            career: [
                "工作與學習效率極佳，適合集中火力攻克具挑戰性的任務。與夥伴保持良好溝通，團隊協作將帶來事半功倍的成果。",
                "創意靈感源源不絕，提案或報告時能提出令人眼睛一亮的觀點。記得把突發奇想記錄下來，未來將派上大用場。",
                "穩紮穩打是今日的致勝關鍵，按部就班完成進度並仔細核對細節，您的專業態度將獲得師長或主管的肯定。",
                "適合學習新技能或吸收跨領域知識的一天，保持好奇心並勇於發問，將能突破目前的瓶頸，看見更寬廣的發展路徑。",
                "面對臨時變動時能展現出色的應變能力，冷靜分析優先順序後逐一擊破，您將發現自己的潛力比想像中更強大。"
            ],
            wealth: [
                "正財運勢穩健，過去的努力逐漸轉化為實質回報。今日適合檢視日常收支與儲蓄計畫，為下一階段累積豐厚底氣。",
                "偏財與小確幸運勢不錯，購物時容易遇到優惠或買到物超所值的好物，但仍建議保持理性消費，避免一時衝動。",
                "理財直覺敏銳，適合花點時間研究長期穩健的資產配置或閱讀財經資訊，吸收新知將有助於提升未來的財務格局。",
                "人際往來可能帶來些許交際花費，視為建立良好關係的投資即可；同時也有機會從朋友口中獲得實用的省錢或理財情報。",
                "收支平衡的一天，把錢花在提升自我成長或健康生活品質上最為值得，務實規劃能讓您對未來更有安全感。"
            ],
            colors: [
                { name: "星夜靛藍", hex: "#4338ca" },
                { name: "琥珀暖金", hex: "#f59e0b" },
                { name: "月光銀白", hex: "#e2e8f0" },
                { name: "薔薇粉晶", hex: "#f472b6" },
                { name: "翡翠森綠", hex: "#10b981" },
                { name: "紫晶星雲", hex: "#a855f7" },
                { name: "晴空蔚藍", hex: "#38bdf8" },
                { name: "珊瑚暖橘", hex: "#fb923c" }
            ],
            advices: [
                "出門前整理一下隨身包包或桌面，清空雜物能為您騰出更多迎接好運的空間。",
                "午後不妨泡一杯溫熱的花草茶或黑咖啡，給自己十分鐘遠離螢幕，讓眼睛與心靈深呼吸。",
                "今天遇到任何幫助您的人，別忘了真誠地說聲謝謝，善意的循環會為您帶來整天的好心情。",
                "睡前聽一段輕柔的純音樂或冥想引導，有助於釋放日常壓力，讓星光伴您一夜好眠。",
                "嘗試換一條沒走過的散步路線回家，微小的生活變化能有效喚醒您的靈感與活力。",
                "把今天完成的三件小事寫在筆記本上，肯定自己的每一步累積，自信心將會倍增。"
            ]
        };

        /**
         * ==========================================
         * 3. 日期初始化與生日判斷邏輯
         * ==========================================
         */
        function getZodiacByDate(month, day) {
            if ((month === 12 && day >= 22) || (month === 1 && day <= 19)) return ZODIAC_SIGNS[0]; // 摩羯
            if ((month === 1 && day >= 20) || (month === 2 && day <= 18)) return ZODIAC_SIGNS[1];  // 水瓶
            if ((month === 2 && day >= 19) || (month === 3 && day <= 20)) return ZODIAC_SIGNS[2];  // 雙魚
            if ((month === 3 && day >= 21) || (month === 4 && day <= 19)) return ZODIAC_SIGNS[3];  // 牡羊
            if ((month === 4 && day >= 20) || (month === 5 && day <= 20)) return ZODIAC_SIGNS[4];  // 金牛
            if ((month === 5 && day >= 21) || (month === 6 && day <= 21)) return ZODIAC_SIGNS[5];  // 雙子
            if ((month === 6 && day >= 22) || (month === 7 && day <= 22)) return ZODIAC_SIGNS[6];  // 巨蟹
            if ((month === 7 && day >= 23) || (month === 8 && day <= 22)) return ZODIAC_SIGNS[7];  // 獅子
            if ((month === 8 && day >= 23) || (month === 9 && day <= 22)) return ZODIAC_SIGNS[8];  // 處女
            if ((month === 9 && day >= 23) || (month === 10 && day <= 23)) return ZODIAC_SIGNS[9]; // 天秤
            if ((month === 10 && day >= 24) || (month === 11 && day <= 22)) return ZODIAC_SIGNS[10]; // 天蠍
            return ZODIAC_SIGNS[11]; // 射手
        }

        // 確定性偽隨機數產生器 (結合今日日期 + 生日，確保同一天查詢結果穩定且合理)
        function createSeededRandom(seedStr) {
            let h = 2166136261 >>> 0;
            for (let i = 0; i < seedStr.length; i++) {
                h ^= seedStr.charCodeAt(i);
                h = Math.imul(h, 16777619);
            }
            return function() {
                h += h << 13; h ^= h >>> 7;
                h += h << 3;  h ^= h >>> 17;
                h += h << 5;
                return (h >>> 0) / 4294967296;
            };
        }

        function initSelectors() {
            const yearSelect = document.getElementById('birth-year');
            const monthSelect = document.getElementById('birth-month');
            const daySelect = document.getElementById('birth-day');
            const now = new Date();

            // 顯示今日日期
            const weekdays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
            document.getElementById('today-date-display').textContent = 
                `西元 ${now.getFullYear()} 年 ${now.getMonth() + 1} 月 ${now.getDate()} 日・${weekdays[now.getDay()]}`;

            // 填入年份 (1950 ~ 今年)
            const currentYear = now.getFullYear();
            const defaultYearOption = document.createElement('option');
            defaultYearOption.value = "";
            defaultYearOption.textContent = "年份(選填)";
            yearSelect.appendChild(defaultYearOption);

            for (let y = currentYear; y >= 1950; y--) {
                const opt = document.createElement('option');
                opt.value = y;
                opt.textContent = `${y} 年`;
                yearSelect.appendChild(opt);
            }

            // 填入月份 (1 ~ 12)
            for (let m = 1; m <= 12; m++) {
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = `${m} 月`;
                monthSelect.appendChild(opt);
            }

            // 動態更新日期天數 (支援大小月與閏年)
            function updateDays() {
                const selectedYear = parseInt(yearSelect.value) || 2000; // 預設閏年 2000 讓 2/29 可選
                const selectedMonth = parseInt(monthSelect.value) || 1;
                const previousDay = parseInt(daySelect.value);

                const daysInMonth = new Date(selectedYear, selectedMonth, 0).getDate();
                daySelect.innerHTML = '<option value="" disabled selected>日期</option>';

                for (let d = 1; d <= daysInMonth; d++) {
                    const opt = document.createElement('option');
                    opt.value = d;
                    opt.textContent = `${d} 日`;
                    if (d === previousDay) opt.selected = true;
                    daySelect.appendChild(opt);
                }
            }

            yearSelect.addEventListener('change', updateDays);
            monthSelect.addEventListener('change', updateDays);
            updateDays();
        }

        /**
         * ==========================================
         * 4. 產生並渲染今日星座運勢
         * ==========================================
         */
        function generateAndRenderHoroscope() {
            const yearVal = document.getElementById('birth-year').value;
            const monthVal = parseInt(document.getElementById('birth-month').value);
            const dayVal = parseInt(document.getElementById('birth-day').value);
            const msgBox = document.getElementById('message-box');

            if (!monthVal || !dayVal) {
                msgBox.classList.remove('hidden');
                return;
            }
            msgBox.classList.add('hidden');

            const zodiac = getZodiacByDate(monthVal, dayVal);
            const now = new Date();
            const todayKey = `${now.getFullYear()}-${now.getMonth() + 1}-${now.getDate()}`;
            // 結合今日日期、星座與出生年份作為種子
            const seed = `${todayKey}_${zodiac.id}_${yearVal || 'default'}`;
            const rand = createSeededRandom(seed);

            // 計算各項分數 (72 ~ 98 之間，給予積極正向體驗)
            const loveScore = Math.floor(rand() * 26) + 73;
            const careerScore = Math.floor(rand() * 26) + 73;
            const wealthScore = Math.floor(rand() * 26) + 72;
            const overallScore = Math.round((loveScore + careerScore + wealthScore) / 3);
            const starCount = overallScore >= 90 ? 5 : (overallScore >= 80 ? 4 : 3);

            // 抽選文案與幸運元素
            const overallText = HOROSCOPE_POOL.overall[Math.floor(rand() * HOROSCOPE_POOL.overall.length)];
            const loveText = HOROSCOPE_POOL.love[Math.floor(rand() * HOROSCOPE_POOL.love.length)];
            const careerText = HOROSCOPE_POOL.career[Math.floor(rand() * HOROSCOPE_POOL.career.length)];
            const wealthText = HOROSCOPE_POOL.wealth[Math.floor(rand() * HOROSCOPE_POOL.wealth.length)];
            const luckyColor = HOROSCOPE_POOL.colors[Math.floor(rand() * HOROSCOPE_POOL.colors.length)];
            const dailyAdvice = HOROSCOPE_POOL.advices[Math.floor(rand() * HOROSCOPE_POOL.advices.length)];

            // 幸運數字 (兩個不重複數字 1~36)
            const num1 = Math.floor(rand() * 18) + 1;
            const num2 = Math.floor(rand() * 18) + 19;

            // 貴人星座 (排除自己)
            const otherZodiacs = ZODIAC_SIGNS.filter(z => z.id !== zodiac.id);
            const benefactor = otherZodiacs[Math.floor(rand() * otherZodiacs.length)];

            // 更新 DOM 內容
            document.getElementById('zodiac-watermark').textContent = zodiac.symbol;
            document.getElementById('zodiac-symbol').textContent = zodiac.symbol;
            document.getElementById('zodiac-en').textContent = zodiac.en;
            document.getElementById('zodiac-name').textContent = zodiac.name;
            document.getElementById('zodiac-date-range').textContent = zodiac.range;
            document.getElementById('zodiac-element').textContent = zodiac.element;

            // 星級渲染
            const starsContainer = document.getElementById('overall-stars');
            starsContainer.innerHTML = '';
            for (let i = 1; i <= 5; i++) {
                const starSpan = document.createElement('span');
                starSpan.textContent = i <= starCount ? '★' : '☆';
                starSpan.className = i <= starCount ? 'text-amber-400 drop-shadow-[0_0_6px_rgba(251,191,36,0.6)]' : 'text-slate-600';
                starsContainer.appendChild(starSpan);
            }
            document.getElementById('overall-score-badge').textContent = `${overallScore} 分`;
            document.getElementById('overall-summary').textContent = overallText;

            // 三大運勢更新
            document.getElementById('love-score-text').textContent = `${loveScore}%`;
            document.getElementById('love-desc').textContent = loveText;

            document.getElementById('career-score-text').textContent = `${careerScore}%`;
            document.getElementById('career-desc').textContent = careerText;

            document.getElementById('wealth-score-text').textContent = `${wealthScore}%`;
            document.getElementById('wealth-desc').textContent = wealthText;

            // 幸運元素更新
            document.getElementById('lucky-color-dot').style.backgroundColor = luckyColor.hex;
            document.getElementById('lucky-color-name').textContent = luckyColor.name;
            document.getElementById('lucky-number').textContent = `${num1}、${num2}`;
            document.getElementById('benefactor-symbol').textContent = benefactor.symbol;
            document.getElementById('benefactor-name').textContent = benefactor.name;
            document.getElementById('daily-advice').textContent = dailyAdvice;

            // 顯示結果區塊並觸發進度條動畫
            const resultSection = document.getElementById('result-section');
            resultSection.classList.remove('hidden');

            // 先歸零進度條再延遲展開以觸發平滑過渡
            document.getElementById('love-bar').style.width = '0%';
            document.getElementById('career-bar').style.width = '0%';
            document.getElementById('wealth-bar').style.width = '0%';

            requestAnimationFrame(() => {
                resultSection.classList.remove('opacity-0', 'translate-y-4');
                setTimeout(() => {
                    document.getElementById('love-bar').style.width = `${loveScore}%`;
                    document.getElementById('career-bar').style.width = `${careerScore}%`;
                    document.getElementById('wealth-bar').style.width = `${wealthScore}%`;
                }, 120);
            });

            // 平滑捲動至結果區域
            resultSection.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
        }

        // 頁面載入後初始化
        window.onload = function() {
            initStarfield();
            initSelectors();
            document.getElementById('predict-btn').addEventListener('click', generateAndRenderHoroscope);
        };
    </script>
</body>
</html>

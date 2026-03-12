<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>命理大师 · 八字紫微算命</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700&display=swap');
        
        body {
            font-family: 'Noto Serif SC', serif;
        }
        .mystical-bg {
            background: linear-gradient(135deg, #1a0f2e 0%, #2a1f3f 100%);
        }
        .card {
            background: rgba(30, 20, 50, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid #d4af77;
        }
        .pillar {
            transition: all 0.3s ease;
        }
        .pillar:hover {
            transform: scale(1.05) rotate(2deg);
            box-shadow: 0 0 25px #ffd700;
        }
        .result-section {
            animation: fadeIn 1s ease forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .spinner {
            border: 4px solid #4b2e83;
            border-top: 4px solid #d4af77;
        }
    </style>
</head>
<body class="mystical-bg text-amber-100 min-h-screen">
    <div class="max-w-4xl mx-auto px-4 py-8">
        <!-- 头部 -->
        <div class="text-center mb-10">
            <div class="flex items-center justify-center gap-4 mb-4">
                <i class="fa-solid fa-dragon text-5xl text-amber-400"></i>
                <h1 class="text-5xl font-bold tracking-widest text-transparent bg-clip-text bg-gradient-to-r from-amber-300 to-yellow-200">命理大师</h1>
                <i class="fa-solid fa-dragon text-5xl text-amber-400"></i>
            </div>
            <p class="text-amber-300 text-xl">八字 · 紫微斗数 · 一生运势全解</p>
            <div class="h-1 w-32 mx-auto bg-gradient-to-r from-transparent via-amber-400 to-transparent mt-4"></div>
        </div>

        <!-- 输入表单 -->
        <div id="formCard" class="card rounded-3xl p-8 shadow-2xl">
            <form id="fortuneForm" class="space-y-8">
                <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
                    <!-- 年 -->
                    <div>
                        <label class="block text-amber-400 text-sm mb-2 font-medium">出生年份</label>
                        <select id="year" class="w-full bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4 focus:outline-none focus:border-amber-400 text-lg">
                            <!-- 1900-2026 -->
                        </select>
                    </div>
                    <!-- 月 -->
                    <div>
                        <label class="block text-amber-400 text-sm mb-2 font-medium">出生月份</label>
                        <select id="month" class="w-full bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4 focus:outline-none focus:border-amber-400 text-lg">
                            <!-- 1-12 -->
                        </select>
                    </div>
                    <!-- 日 -->
                    <div>
                        <label class="block text-amber-400 text-sm mb-2 font-medium">出生日期</label>
                        <select id="day" class="w-full bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4 focus:outline-none focus:border-amber-400 text-lg">
                            <!-- 1-31 -->
                        </select>
                    </div>
                    <!-- 时间 -->
                    <div>
                        <label class="block text-amber-400 text-sm mb-2 font-medium">出生时间（现代时）</label>
                        <input type="time" id="time" value="12:00" 
                               class="w-full bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4 focus:outline-none focus:border-amber-400 text-lg">
                        <div id="shichenDisplay" class="mt-2 text-xs text-amber-400 text-center font-medium"></div>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- 出生地 -->
                    <div class="md:col-span-2">
                        <label class="block text-amber-400 text-sm mb-2 font-medium">出生地（城市）</label>
                        <input type="text" id="birthplace" placeholder="例如：北京" 
                               class="w-full bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4 focus:outline-none focus:border-amber-400 text-lg">
                    </div>
                    <!-- 性别 -->
                    <div>
                        <label class="block text-amber-400 text-sm mb-2 font-medium">性别</label>
                        <div class="flex gap-6 bg-zinc-900 border border-amber-700 rounded-2xl px-5 py-4">
                            <label class="flex items-center gap-2 cursor-pointer">
                                <input type="radio" name="gender" value="男" checked class="accent-amber-400">
                                <span class="text-lg">男</span>
                            </label>
                            <label class="flex items-center gap-2 cursor-pointer">
                                <input type="radio" name="gender" value="女" class="accent-amber-400">
                                <span class="text-lg">女</span>
                            </label>
                        </div>
                    </div>
                </div>

                <button type="button" onclick="startFortune()"
                        class="w-full bg-gradient-to-r from-amber-500 to-yellow-600 hover:from-amber-400 hover:to-yellow-500 text-zinc-950 font-bold text-2xl py-6 rounded-3xl transition-all duration-300 flex items-center justify-center gap-3 shadow-lg">
                    <i class="fa-solid fa-crystal-ball"></i>
                    开始算命
                </button>
            </form>
        </div>

        <!-- 加载动画 -->
        <div id="loading" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-50">
            <div class="text-center">
                <div class="spinner mx-auto w-16 h-16 rounded-full animate-spin"></div>
                <p class="mt-6 text-amber-300 text-2xl tracking-widest">天机正在推演...</p>
                <p class="text-amber-500 text-sm mt-2">请稍候，八字与紫微斗数排盘中</p>
            </div>
        </div>

        <!-- 结果区域 -->
        <div id="resultCard" class="hidden card rounded-3xl p-8 mt-8 shadow-2xl">
            <div class="flex justify-between items-center mb-8 border-b border-amber-700 pb-6">
                <h2 class="text-3xl font-bold text-amber-300">您的命理报告</h2>
                <button onclick="resetForm()" 
                        class="px-6 py-2 bg-zinc-800 hover:bg-zinc-700 rounded-2xl text-sm flex items-center gap-2">
                    <i class="fa-solid fa-rotate"></i> 重新算命
                </button>
            </div>

            <!-- 基本信息 -->
            <div id="basicInfo" class="mb-10 bg-zinc-950/70 rounded-2xl p-6 text-sm"></div>

            <!-- 八字排盘 -->
            <div class="mb-10">
                <h3 class="text-amber-400 text-xl mb-4 flex items-center gap-2">
                    <i class="fa-solid fa-scroll"></i> 生辰八字
                </h3>
                <div id="baziGrid" class="grid grid-cols-4 gap-4"></div>
            </div>

            <!-- 紫微斗数 -->
            <div class="mb-12">
                <h3 class="text-amber-400 text-xl mb-4 flex items-center gap-2">
                    <i class="fa-solid fa-star"></i> 紫微斗数命盘
                </h3>
                <div id="ziweiInfo" class="bg-zinc-950/70 rounded-2xl p-6 text-lg"></div>
            </div>

            <!-- 详细分析 -->
            <div class="space-y-12">
                <!-- 财运 -->
                <div class="result-section">
                    <h3 class="text-2xl font-bold mb-4 text-amber-300 border-l-4 border-amber-400 pl-4">财运详解</h3>
                    <div id="wealthText" class="text-amber-100 leading-relaxed text-lg"></div>
                </div>

                <!-- 整体命运 -->
                <div class="result-section">
                    <h3 class="text-2xl font-bold mb-4 text-amber-300 border-l-4 border-amber-400 pl-4">一生命运</h3>
                    <div id="fateText" class="text-amber-100 leading-relaxed text-lg"></div>
                </div>

                <!-- 桃花运 -->
                <div class="result-section">
                    <h3 class="text-2xl font-bold mb-4 text-amber-300 border-l-4 border-amber-400 pl-4">桃花姻缘</h3>
                    <div id="peachText" class="text-amber-100 leading-relaxed text-lg"></div>
                </div>

                <!-- 年龄段运势 -->
                <div class="result-section">
                    <h3 class="text-2xl font-bold mb-6 text-amber-300 border-l-4 border-amber-400 pl-4">各年龄段运势与转运</h3>
                    <div id="ageSections" class="space-y-8"></div>
                </div>
            </div>

            <!-- 免责 -->
            <div class="mt-12 text-center text-amber-500 text-xs leading-relaxed border-t border-amber-800 pt-6">
                本结果为娱乐性质模拟推演，仅供参考。<br>
                命运掌握在自己手中，行善积德、努力奋斗方能改运！
            </div>
        </div>
    </div>

    <script>
        // 天干地支
        const heavenlyStems = ["甲", "乙", "丙", "丁", "戊", "己", "庚", "辛", "壬", "癸"];
        const earthlyBranches = ["子", "丑", "寅", "卯", "辰", "巳", "午", "未", "申", "酉", "戌", "亥"];

        // 紫微主星备选（简化）
        const ziweiStarsMale = ["紫微星", "天机星", "太阳星", "武曲星", "天同星", "廉贞星", "天府星"];
        const ziweiStarsFemale = ["太阴星", "贪狼星", "巨门星", "天相星", "天梁星", "七杀星", "破军星"];

        // 时辰映射（现代小时 → 地支）
        function getShichen(hour) {
            if (hour >= 23 || hour < 1) return "子时";
            if (hour >= 1 && hour < 3) return "丑时";
            if (hour >= 3 && hour < 5) return "寅时";
            if (hour >= 5 && hour < 7) return "卯时";
            if (hour >= 7 && hour < 9) return "辰时";
            if (hour >= 9 && hour < 11) return "巳时";
            if (hour >= 11 && hour < 13) return "午时";
            if (hour >= 13 && hour < 15) return "未时";
            if (hour >= 15 && hour < 17) return "申时";
            if (hour >= 17 && hour < 19) return "酉时";
            if (hour >= 19 && hour < 21) return "戌时";
            return "亥时";
        }

        // 初始化年月日下拉
        function initSelects() {
            const yearSelect = document.getElementById('year');
            for (let y = 1900; y <= 2026; y++) {
                const opt = document.createElement('option');
                opt.value = y;
                opt.textContent = y + "年";
                if (y === 1990) opt.selected = true;
                yearSelect.appendChild(opt);
            }

            const monthSelect = document.getElementById('month');
            for (let m = 1; m <= 12; m++) {
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = m + "月";
                monthSelect.appendChild(opt);
            }

            const daySelect = document.getElementById('day');
            for (let d = 1; d <= 31; d++) {
                const opt = document.createElement('option');
                opt.value = d;
                opt.textContent = d + "日";
                daySelect.appendChild(opt);
            }
        }

        // 更新时辰显示
        function updateShichen() {
            const timeInput = document.getElementById('time');
            const display = document.getElementById('shichenDisplay');
            timeInput.addEventListener('change', () => {
                const [h] = timeInput.value.split(':').map(Number);
                display.textContent = `对应时辰：${getShichen(h)}`;
            });
        }

        // 计算八字（娱乐版模拟）
        function calculateBazi(year, month, day, hour) {
            const yearStemIdx = (year - 4) % 10;
            const yearBranchIdx = (year - 4) % 12;
            const yearPillar = heavenlyStems[yearStemIdx < 0 ? yearStemIdx + 10 : yearStemIdx] + 
                              earthlyBranches[yearBranchIdx < 0 ? yearBranchIdx + 12 : yearBranchIdx];

            // 月柱简化模拟
            const monthStemIdx = (yearStemIdx + month * 2) % 10;
            const monthBranchIdx = (month + 1) % 12;
            const monthPillar = heavenlyStems[monthStemIdx] + earthlyBranches[monthBranchIdx];

            // 日柱简化模拟
            const dayStemIdx = (day + year) % 10;
            const dayBranchIdx = (day + 3) % 12;
            const dayPillar = heavenlyStems[dayStemIdx] + earthlyBranches[dayBranchIdx];

            // 时柱
            const hourBranch = earthlyBranches[Math.floor((hour + 1) % 24 / 2)];
            const hourStemIdx = (dayStemIdx * 2 + Math.floor(hour / 2)) % 10;
            const hourPillar = heavenlyStems[hourStemIdx] + hourBranch;

            return {
                year: yearPillar,
                month: monthPillar,
                day: dayPillar,
                hour: hourPillar
            };
        }

        // 生成结果HTML
        function generateResult(year, month, day, hour, minute, birthplace, gender) {
            const timeStr = `${hour.toString().padStart(2, '0')}:${minute.toString().padStart(2, '0')}`;
            const shichen = getShichen(hour);
            const bazi = calculateBazi(year, month, day, hour);
            const mainStar = gender === "男" 
                ? ziweiStarsMale[year % ziweiStarsMale.length] 
                : ziweiStarsFemale[year % ziweiStarsFemale.length];

            // 基本信息
            document.getElementById('basicInfo').innerHTML = `
                <div class="flex flex-wrap gap-6 text-amber-200">
                    <div><span class="text-amber-400">出生信息：</span>${year}年${month}月${day}日 ${timeStr}（${shichen}）</div>
                    <div><span class="text-amber-400">出生地：</span>${birthplace || "未知"}</div>
                    <div><span class="text-amber-400">性别：</span>${gender}</div>
                </div>
            `;

            // 八字网格
            const baziHTML = `
                <div class="pillar bg-zinc-900 rounded-2xl p-5 text-center border border-amber-600">
                    <div class="text-amber-400 text-xs">年柱</div>
                    <div class="text-3xl font-bold mt-1">${bazi.year}</div>
                </div>
                <div class="pillar bg-zinc-900 rounded-2xl p-5 text-center border border-amber-600">
                    <div class="text-amber-400 text-xs">月柱</div>
                    <div class="text-3xl font-bold mt-1">${bazi.month}</div>
                </div>
                <div class="pillar bg-zinc-900 rounded-2xl p-5 text-center border border-amber-600">
                    <div class="text-amber-400 text-xs">日柱</div>
                    <div class="text-3xl font-bold mt-1">${bazi.day}</div>
                </div>
                <div class="pillar bg-zinc-900 rounded-2xl p-5 text-center border border-amber-600">
                    <div class="text-amber-400 text-xs">时柱</div>
                    <div class="text-3xl font-bold mt-1">${bazi.hour}</div>
                </div>
            `;
            document.getElementById('baziGrid').innerHTML = baziHTML;

            // 紫微
            document.getElementById('ziweiInfo').innerHTML = `
                <div class="flex items-center gap-8">
                    <div class="flex-shrink-0">
                        <div class="w-28 h-28 bg-gradient-to-br from-purple-900 to-amber-900 rounded-3xl flex items-center justify-center text-6xl shadow-inner">⭐</div>
                    </div>
                    <div>
                        <div class="text-amber-400 text-xl">命宫主星：<span class="text-yellow-300 font-bold">${mainStar}</span></div>
                        <div class="mt-3 text-base leading-relaxed">
                            您的紫微命盘显示${mainStar}坐守命宫，${gender === "男" ? "格局稳重，事业心强" : "格局柔美，智慧过人"}。<br>
                            辅星加持，贵人运极强，一生多得长辈或贵人提携。
                        </div>
                    </div>
                </div>
            `;

            // 财运
            document.getElementById('wealthText').innerHTML = `
                <p>您的财运整体属于中上格局，日主${bazi.day[0]}属${bazi.day[1]}，正财与偏财皆有来源。</p>
                <p class="mt-4">早年（20岁前）财运平平，主要依靠父母或稳定工资；30岁后进入财运上升期，正财稳定，适合从事金融、科技、贸易或创意行业。40岁前后会有一次较大投资机会，但需谨慎，避免冲动。晚年（55岁后）财富积累丰厚，可享清福。</p>
                <p class="mt-4 text-amber-400">转运建议：35岁后多参与团队合作项目，忌单独投机。每年农历三月、九月为财神月，宜布局理财。</p>
            `;

            // 命运
            document.getElementById('fateText').innerHTML = `
                <p>一生命运先苦后甜，年轻时多磨练，中年后事业有成，家庭和睦。贵人运极强，${gender === "男" ? "适合从政或管理岗位" : "适合教育、艺术或服务业"}。</p>
                <p class="mt-4">健康方面注意肝肾与呼吸系统，40岁前后为人生关键节点，需保持乐观心态。整体运势由中转上，晚年子女孝顺，生活富足安康。</p>
                <p class="mt-4 text-amber-400">改运秘诀：多行善积德，佩戴玉石或参加公益活动可增强运势。</p>
            `;

            // 桃花运
            document.getElementById('peachText').innerHTML = `
                <p>您的桃花运较为旺盛，异性缘分极佳。${gender === "男" ? "容易吸引温柔贤惠的伴侣" : "追求者众多，需谨慎选择"}。</p>
                <p class="mt-4">25-35岁为正桃花期，最易遇到正缘。婚姻运稳定，夫妻相敬如宾。但需注意33岁前后有烂桃花风险，避免暧昧关系。</p>
                <p class="mt-4 text-amber-400">建议：结婚后多陪伴家人，农历七月适合求姻缘。</p>
            `;

            // 年龄段
            const ageData = [
                {range: "0-20岁", desc: "童年运势平稳，家庭环境优越，学业优秀。18岁前后开始接触社会，奠定基础。转运点：高考或大学入学年份。"},
                {range: "21-30岁", desc: "事业起步阶段，贵人相助，财运渐开。25岁前后有一次重要机遇（跳槽或创业）。注意感情专一。"},
                {range: "31-40岁", desc: "人生巅峰期，事业大成，财富快速积累。35岁为重大转运年，建议大胆投资或拓展人脉。"},
                {range: "41-50岁", desc: "守成阶段，家庭幸福，子女优秀。需注意身体保养，避免过度操劳。"},
                {range: "51-60岁", desc: "智慧巅峰，适合传授经验或退休规划。财运稳固，可享受人生。"},
                {range: "60岁以后", desc: "晚年福寿双全，子孙满堂。建议多旅行、养生，生活安逸富足。"}
            ];

            let ageHTML = "";
            ageData.forEach(item => {
                ageHTML += `
                    <div class="border-l-2 border-amber-500 pl-6">
                        <div class="flex items-center gap-3">
                            <span class="text-amber-400 font-bold text-xl">${item.range}</span>
                        </div>
                        <p class="mt-2 text-amber-100">${item.desc}</p>
                    </div>
                `;
            });
            document.getElementById('ageSections').innerHTML = ageHTML;
        }

        // 开始算命
        function startFortune() {
            const year = parseInt(document.getElementById('year').value);
            const month = parseInt(document.getElementById('month').value);
            const day = parseInt(document.getElementById('day').value);
            const timeVal = document.getElementById('time').value;
            const birthplace = document.getElementById('birthplace').value.trim();
            const gender = document.querySelector('input[name="gender"]:checked').value;

            if (!timeVal) {
                alert("请选择出生时间！");
                return;
            }

            const [hour, minute] = timeVal.split(':').map(Number);

            // 显示加载
            document.getElementById('loading').classList.remove('hidden');
            document.getElementById('formCard').classList.add('opacity-30');

            setTimeout(() => {
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('formCard').classList.add('hidden');
                document.getElementById('resultCard').classList.remove('hidden');

                generateResult(year, month, day, hour, minute, birthplace, gender);
            }, 2800);
        }

        // 重置
        function resetForm() {
            document.getElementById('resultCard').classList.add('hidden');
            document.getElementById('formCard').classList.remove('hidden');
            document.getElementById('formCard').classList.remove('opacity-30');
        }

        // 初始化
        window.onload = function() {
            initSelects();
            updateShichen();
            
            // Tailwind script already loaded
            console.log('%c命理大师网页已就绪 ✨', 'color:#d4af77; font-size:12px');
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>八字紫微命盘</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;700&amp;display=swap');
        
        body {
            font-family: 'Noto Sans SC', system-ui, sans-serif;
        }
        
        .pillar {
            background: linear-gradient(135deg, #f8e5c8, #f5d0a0);
            border: 3px solid #d97706;
        }
        
        .result-card {
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }
        
        .nav-tab {
            transition: all 0.3s ease;
        }
        
        .nav-tab.active {
            border-bottom: 3px solid #d97706;
            color: #d97706;
            font-weight: 700;
        }
        
        .mobile-container {
            max-width: 480px;
            margin: 0 auto;
            background: #fff;
            min-height: 100vh;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
        }
        
        .header-bg {
            background: linear-gradient(90deg, #78350f, #b45309);
        }
    </style>
</head>
<body class="bg-amber-50">
    <div class="mobile-container mx-auto overflow-hidden">
        <!-- 顶部导航栏 -->
        <header class="header-bg text-white py-4 px-6 flex items-center justify-between sticky top-0 z-50">
            <div class="flex items-center gap-3">
                <i class="fa-solid fa-yin-yang text-3xl"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-wider">八字紫微</h1>
                    <p class="text-xs opacity-75 -mt-1">精准排盘 · 庸善传媒</p>
                </div>
            </div>
            <button onclick="resetApp()" 
                    class="bg-white/20 hover:bg-white/30 px-4 py-2 rounded-2xl text-sm flex items-center gap-2 transition">
                <i class="fa-solid fa-rotate"></i>
                重算
            </button>
        </header>

        <!-- 输入界面 -->
        <div id="input-screen" class="p-6">
            <div class="text-center mb-8">
                <div class="inline-flex items-center gap-2 bg-amber-100 text-amber-800 px-6 py-2 rounded-3xl text-sm font-medium">
                    <i class="fa-solid fa-star"></i>
                    请输入出生信息
                </div>
            </div>

            <form id="birth-form" onsubmit="calculateFate(event)" class="space-y-6">
                <!-- 性别 -->
                <div>
                    <label class="block text-sm font-medium text-amber-900 mb-2">性别</label>
                    <div class="grid grid-cols-2 gap-4">
                        <label class="flex items-center justify-center bg-white border-2 border-transparent has-[:checked]:border-amber-600 rounded-3xl py-4 cursor-pointer transition">
                            <input type="radio" name="gender" value="男" class="hidden" checked>
                            <span class="flex items-center gap-3 text-lg">
                                <i class="fa-solid fa-mars text-blue-600"></i>
                                <span class="font-medium">男</span>
                            </span>
                        </label>
                        <label class="flex items-center justify-center bg-white border-2 border-transparent has-[:checked]:border-amber-600 rounded-3xl py-4 cursor-pointer transition">
                            <input type="radio" name="gender" value="女" class="hidden">
                            <span class="flex items-center gap-3 text-lg">
                                <i class="fa-solid fa-venus text-pink-600"></i>
                                <span class="font-medium">女</span>
                            </span>
                        </label>
                    </div>
                </div>

                <!-- 出生日期 -->
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-amber-900 mb-2">出生年份</label>
                        <select id="year" required
                                class="w-full px-5 py-4 bg-white border border-amber-200 rounded-3xl focus:outline-none focus:border-amber-500 text-lg">
                            <!-- 1900-2025 年份快速填充 -->
                            <script>
                                for(let y=1900; y<=2025; y++){
                                    document.write(`<option value="${y}">${y}年</option>`);
                                }
                            </script>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-900 mb-2">出生月份</label>
                        <select id="month" required
                                class="w-full px-5 py-4 bg-white border border-amber-200 rounded-3xl focus:outline-none focus:border-amber-500 text-lg">
                            <option value="1">1月</option>
                            <option value="2">2月</option>
                            <option value="3">3月</option>
                            <option value="4">4月</option>
                            <option value="5">5月</option>
                            <option value="6">6月</option>
                            <option value="7">7月</option>
                            <option value="8">8月</option>
                            <option value="9">9月</option>
                            <option value="10">10月</option>
                            <option value="11">11月</option>
                            <option value="12">12月</option>
                        </select>
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-amber-900 mb-2">出生日期</label>
                        <select id="day" required
                                class="w-full px-5 py-4 bg-white border border-amber-200 rounded-3xl focus:outline-none focus:border-amber-500 text-lg">
                            <!-- JS 动态生成 1-31 -->
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-900 mb-2">出生时辰</label>
                        <select id="hour" required
                                class="w-full px-5 py-4 bg-white border border-amber-200 rounded-3xl focus:outline-none focus:border-amber-500 text-lg">
                            <option value="子时">子时 (23:00-01:00)</option>
                            <option value="丑时">丑时 (01:00-03:00)</option>
                            <option value="寅时">寅时 (03:00-05:00)</option>
                            <option value="卯时">卯时 (05:00-07:00)</option>
                            <option value="辰时">辰时 (07:00-09:00)</option>
                            <option value="巳时">巳时 (09:00-11:00)</option>
                            <option value="午时">午时 (11:00-13:00)</option>
                            <option value="未时">未时 (13:00-15:00)</option>
                            <option value="申时">申时 (15:00-17:00)</option>
                            <option value="酉时">酉时 (17:00-19:00)</option>
                            <option value="戌时">戌时 (19:00-21:00)</option>
                            <option value="亥时">亥时 (21:00-23:00)</option>
                        </select>
                    </div>
                </div>

                <!-- 提交按钮 -->
                <button type="submit"
                        class="w-full py-6 bg-gradient-to-r from-amber-600 to-orange-600 text-white font-bold text-xl rounded-3xl shadow-xl active:scale-95 transition flex items-center justify-center gap-3">
                    <i class="fa-solid fa-wand-magic-sparkles"></i>
                    开始排盘
                </button>
            </form>

            <div class="text-center mt-8 text-xs text-amber-600/70">
                ※ 本命盘仅供参考 · 数据来自网络
            </div>
        </div>

        <!-- 结果界面（默认隐藏） -->
        <div id="result-screen" class="hidden">
            <!-- 出生信息卡 -->
            <div class="bg-white mx-6 mt-6 rounded-3xl p-5 shadow result-card">
                <div class="flex justify-between items-center">
                    <div>
                        <span id="display-gender" class="inline-block px-3 py-1 bg-amber-100 text-amber-800 rounded-2xl text-sm"></span>
                        <span id="display-birth" class="block text-3xl font-bold text-amber-900 mt-2"></span>
                    </div>
                    <div class="text-right">
                        <div class="text-xs text-amber-500">您的八字命盘</div>
                        <div id="display-bazi-short" class="font-mono text-2xl tracking-widest text-orange-800"></div>
                    </div>
                </div>
            </div>

            <!-- 八字排盘 -->
            <div class="mx-6 mt-6">
                <div class="flex items-center gap-2 mb-4 px-1">
                    <i class="fa-solid fa-table text-amber-700"></i>
                    <h2 class="text-xl font-bold text-amber-900">八字排盘</h2>
                </div>
                <div class="grid grid-cols-4 gap-2 text-center">
                    <!-- 时柱 -->
                    <div class="pillar rounded-3xl py-6">
                        <div class="text-xs text-amber-700">时柱</div>
                        <div id="pillar-hour" class="text-4xl font-bold text-amber-950 mt-1">丙午</div>
                        <div class="text-[10px] text-amber-600 mt-2">纳音：剑锋金</div>
                    </div>
                    <!-- 日柱 -->
                    <div class="pillar rounded-3xl py-6">
                        <div class="text-xs text-amber-700">日柱</div>
                        <div id="pillar-day" class="text-4xl font-bold text-amber-950 mt-1">乙巳</div>
                        <div class="text-[10px] text-amber-600 mt-2">日主：乙木</div>
                    </div>
                    <!-- 月柱 -->
                    <div class="pillar rounded-3xl py-6">
                        <div class="text-xs text-amber-700">月柱</div>
                        <div id="pillar-month" class="text-4xl font-bold text-amber-950 mt-1">甲辰</div>
                        <div class="text-[10px] text-amber-600 mt-2">纳音：覆灯火</div>
                    </div>
                    <!-- 年柱 -->
                    <div class="pillar rounded-3xl py-6">
                        <div class="text-xs text-amber-700">年柱</div>
                        <div id="pillar-year" class="text-4xl font-bold text-amber-950 mt-1">癸丑</div>
                        <div class="text-[10px] text-amber-600 mt-2">纳音：桑柘木</div>
                    </div>
                </div>
            </div>

            <!-- 紫微斗数命宫 -->
            <div class="mx-6 mt-8 bg-white rounded-3xl p-6 result-card">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-star text-3xl text-purple-600"></i>
                    <div>
                        <div class="text-purple-800 font-medium">紫微斗数 · 命宫</div>
                        <div id="ziwei-palace" class="text-4xl font-bold text-purple-950">子宫</div>
                    </div>
                </div>
                <div class="mt-4 text-sm text-purple-700 leading-relaxed">
                    命宫坐子，紫微星入庙，格局为「紫相朝垣」，一生贵人相助，格局中上。
                </div>
            </div>

            <!-- 导航标签 -->
            <div class="mx-6 mt-8 flex border-b border-amber-200 overflow-x-auto">
                <button onclick="switchTab(0)" id="tab-0"
                        class="nav-tab active px-6 py-3 text-sm whitespace-nowrap">性格</button>
                <button onclick="switchTab(1)" id="tab-1"
                        class="nav-tab px-6 py-3 text-sm whitespace-nowrap">姻缘</button>
                <button onclick="switchTab(2)" id="tab-2"
                        class="nav-tab px-6 py-3 text-sm whitespace-nowrap">财运</button>
                <button onclick="switchTab(3)" id="tab-3"
                        class="nav-tab px-6 py-3 text-sm whitespace-nowrap">事业</button>
                <button onclick="switchTab(4)" id="tab-4"
                        class="nav-tab px-6 py-3 text-sm whitespace-nowrap">流年</button>
            </div>

            <!-- 内容区域 -->
            <div id="tab-content" class="mx-6 mt-2 pb-24">
                <!-- 性格 -->
                <div id="content-0">
                    <div class="result-card bg-white p-6 rounded-3xl">
                        <p class="text-amber-800 leading-relaxed">
                            您日主乙木，生于辰月，得时令之气。性格温和内敛，外柔内刚，富有同情心与艺术细胞。思维敏捷，善于洞察人心，但有时优柔寡断。命带华盖，喜欢独处思考，文艺气质浓厚。
                        </p>
                    </div>
                </div>

                <!-- 姻缘 -->
                <div id="content-1" class="hidden">
                    <div class="result-card bg-white p-6 rounded-3xl">
                        <p class="text-amber-800 leading-relaxed">
                            正桃花旺，配偶属相宜鼠、龙。婚姻稳定，晚婚更佳。配偶温柔贤惠，事业心强，能给您生活上的支持。感情路前期多波折，30岁后逐渐平稳。
                        </p>
                    </div>
                </div>

                <!-- 财运 -->
                <div id="content-2" class="hidden">
                    <div class="result-card bg-white p-6 rounded-3xl">
                        <p class="text-amber-800 leading-relaxed">
                            正财稳固，偏财运佳。35岁后财库开启，适合从事教育、文化、设计相关行业。投资宜稳健，房产、股票皆有收获。一生衣食无忧，中年后富足。
                        </p>
                    </div>
                </div>

                <!-- 事业 -->
                <div id="content-3" class="hidden">
                    <div class="result-card bg-white p-6 rounded-3xl">
                        <p class="text-amber-800 leading-relaxed">
                            事业宫得紫微星照，适合管理与创意工作。贵人运极强，40岁前需积累，45岁后大展宏图。适合公职、教育、科技、艺术领域，领导力突出。
                        </p>
                    </div>
                </div>

                <!-- 流年 -->
                <div id="content-4" class="hidden">
                    <div class="space-y-4">
                        <div class="bg-white rounded-3xl p-5 flex gap-4">
                            <div class="w-12 h-12 bg-green-100 rounded-2xl flex items-center justify-center text-2xl">2026</div>
                            <div class="flex-1">
                                <div class="font-medium text-green-800">大吉之年</div>
                                <div class="text-sm text-amber-700">事业升迁，财运正旺，宜把握机会</div>
                            </div>
                        </div>
                        <div class="bg-white rounded-3xl p-5 flex gap-4">
                            <div class="w-12 h-12 bg-orange-100 rounded-2xl flex items-center justify-center text-2xl">2027</div>
                            <div class="flex-1">
                                <div class="font-medium text-orange-800">平稳过渡</div>
                                <div class="text-sm text-amber-700">注意身体，家庭和睦</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 一生命理总结 -->
            <div class="mx-6 mt-6 bg-gradient-to-br from-amber-900 to-orange-900 text-white rounded-3xl p-8 result-card">
                <div class="flex items-center gap-3 mb-4">
                    <i class="fa-solid fa-scroll text-3xl"></i>
                    <h3 class="text-xl font-bold">一生命理总结</h3>
                </div>
                <div id="life-summary" class="text-amber-100 leading-relaxed text-[15px]">
                    命局中和，日主得地，格局为「伤官格」。一生贵人多，波折少，中年后富贵双全。健康注意肝胆与情绪调节。总体而言，上等命格，福寿双全。
                </div>
            </div>

            <div class="h-24"></div>
        </div>
    </div>

    <script>
        // Tailwind 初始化
        function initTailwind() {
            // 已通过 CDN 加载，无需额外操作
        }

        // 动态生成日期选项
        function generateDayOptions() {
            const daySelect = document.getElementById('day');
            daySelect.innerHTML = '';
            for(let d = 1; d <= 31; d++) {
                const option = document.createElement('option');
                option.value = d;
                option.textContent = d + '日';
                daySelect.appendChild(option);
            }
        }

        // 切换标签页
        function switchTab(tabIndex) {
            // 移除所有 active
            for(let i = 0; i < 5; i++) {
                const tab = document.getElementById('tab-' + i);
                const content = document.getElementById('content-' + i);
                if(tab) tab.classList.remove('active');
                if(content) content.classList.add('hidden');
            }
            // 添加 active
            document.getElementById('tab-' + tabIndex).classList.add('active');
            document.getElementById('content-' + tabIndex).classList.remove('hidden');
        }

        // 计算命盘（模拟算法）
        function calculateFate(e) {
            e.preventDefault();
            
            // 获取输入
            const year = document.getElementById('year').value;
            const month = document.getElementById('month').value;
            const day = document.getElementById('day').value;
            const hour = document.getElementById('hour').value;
            const genderRadios = document.getElementsByName('gender');
            let gender = '男';
            for(let radio of genderRadios) {
                if(radio.checked) gender = radio.value;
            }
            
            // 显示结果界面
            document.getElementById('input-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            
            // 显示出生信息
            document.getElementById('display-gender').textContent = gender;
            document.getElementById('display-birth').innerHTML = 
                `${year}年 ${month}月 ${day}日 ${hour}`;
            
            // 模拟八字（根据年份简单映射，实际应使用专业算法）
            const stems = ['甲','乙','丙','丁','戊','己','庚','辛','壬','癸'];
            const branches = ['子','丑','寅','卯','辰','巳','午','未','申','酉','戌','亥'];
            
            const yearStem = stems[(parseInt(year) - 4) % 10];
            const yearBranch = branches[(parseInt(year) - 4) % 12];
            
            document.getElementById('pillar-year').textContent = yearStem + yearBranch;
            document.getElementById('pillar-month').textContent = '甲辰';
            document.getElementById('pillar-day').textContent = '乙巳';
            document.getElementById('pillar-hour').textContent = '丙午';
            
            document.getElementById('display-bazi-short').textContent = 
                `${yearStem}${yearBranch} 甲辰 乙巳 丙午`;
            
            // 紫微命宫（模拟）
            const palaces = ['子宫','丑宫','寅宫','卯宫','辰宫','巳宫','午宫','未宫','申宫','酉宫','戌宫','亥宫'];
            const randomPalace = palaces[parseInt(year) % 12];
            document.getElementById('ziwei-palace').textContent = randomPalace;
            
            // 默认显示性格标签
            switchTab(0);
        }

        // 重置应用
        function resetApp() {
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('input-screen').classList.remove('hidden');
            
            // 清空表单（可选）
            document.getElementById('birth-form').reset();
        }

        // 初始化
        window.onload = function() {
            initTailwind();
            generateDayOptions();
            
            // 默认选中第一个标签（虽然隐藏）
            console.log('%c八字紫微手机版已就绪 ✨', 'color:#d97706; font-weight:bold');
        };
    </script>
</body>
</html>

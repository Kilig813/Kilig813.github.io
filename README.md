[回忆录.html](https://github.com/user-attachments/files/23789230/default.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>孟州一中回忆录 | 青春碎片集</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "宋体", "微软雅黑", cursive;
        }

        body {
            background-color: #f8f5f0;
            background-image: url("https://via.placeholder.com/1920x1080?text=旧笔记本纹理");
            background-size: cover;
            background-attachment: fixed;
            color: #333;
            line-height: 1.8;
            height: 100vh;
            overflow: hidden;
            position: relative;
        }

        /* 左侧主题侧边栏：半隐藏+悬浮展开 */
        .theme-sidebar {
            position: fixed;
            top: 0;
            left: 0;
            width: 90px; /* 默认半隐藏宽度 */
            height: 100vh;
            background: rgba(255,255,255,0.92);
            box-shadow: 3px 0 15px rgba(0,0,0,0.1);
            z-index: 30;
            transition: width 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
            overflow: hidden;
            padding: 40px 0;
        }
        .theme-sidebar:hover {
            width: 320px; /* 鼠标靠近展开宽度 */
        }

        /* 侧边栏标题 */
        .sidebar-header {
            padding: 0 20px 30px;
            border-bottom: 1px dashed #d4b898;
            margin-bottom: 20px;
            transform: translateX(10px);
            transition: transform 0.5s ease 0.1s;
        }
        .theme-sidebar:hover .sidebar-header {
            transform: translateX(0);
        }
        .sidebar-header h1 {
            font-size: 1.8rem;
            color: #8b5a2b;
            transform: rotate(-1deg);
            white-space: nowrap;
        }
        .sidebar-header p {
            font-size: 0.9rem;
            color: #666;
            font-style: italic;
            margin-top: 5px;
            white-space: nowrap;
        }

        /* 侧边栏主题列表：垂直排列 */
        .theme-list {
            height: calc(100vh - 150px);
            overflow-y: auto;
            padding: 0 10px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .theme-card {
            background: rgba(255,255,255,0.85);
            border-radius: 6px;
            padding: 15px 20px;
            box-shadow: 2px 2px 8px rgba(0,0,0,0.08);
            border-left: 3px solid #d4b898;
            cursor: pointer;
            transition: all 0.3s ease;
            transform: translateX(15px);
            opacity: 0.7;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .theme-sidebar:hover .theme-card {
            transform: translateX(0);
            opacity: 1;
        }
        .theme-card:hover {
            transform: translateX(5px) scale(1.02);
            box-shadow: 3px 3px 10px rgba(0,0,0,0.12);
            background: #fff;
        }
        .theme-card h2 {
            font-size: 1.1rem;
            color: #8b5a2b;
            margin-bottom: 5px;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .theme-card p {
            font-size: 0.85rem;
            color: #555;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* 主内容区：详情页容器 */
        .main-content {
            margin-left: 80px; /* 适配侧边栏默认宽度 */
            height: 100vh;
            transition: margin-left 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
            position: relative;
        }
        .theme-sidebar:hover + .main-content {
            margin-left: 320px; /* 侧边栏展开时同步偏移 */
        }

        /* 详情页：不对称分栏+回忆碎片拼贴 */
        .page-detail {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            padding: 50px;
            display: grid;
            grid-template-columns: 3fr 1fr;
            gap: 30px;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.5s ease;
            pointer-events: none;
            overflow-y: auto;
        }
        .page-detail.active {
            opacity: 1;
            transform: translateY(0);
            pointer-events: all;
        }

        /* 详情页主内容区 */
        .detail-main {
            background: rgba(255,255,255,0.95);
            border-radius: 8px;
            padding: 40px;
            box-shadow: 4px 4px 12px rgba(0,0,0,0.1);
            position: relative;
        }
        .detail-main::before {
            content: "";
            position: absolute;
            top: 20px;
            left: 20px;
            width: 100px;
            height: 3px;
            background: #d4b898;
        }
        .detail-title {
            font-size: 2.5rem;
            color: #8b5a2b;
            margin-bottom: 20px;
            padding-left: 20px;
            border-left: 4px solid #d4b898;
            transform: rotate(-0.3deg);
            display: inline-block;
        }
        .detail-brief {
            font-style: italic;
            color: #666;
            font-size: 1.1rem;
            margin-bottom: 30px;
            padding-left: 24px;
        }
        .detail-content {
            line-height: 2;
            color: #444;
        }
        .detail-content p {
            margin-bottom: 25px;
            text-indent: 2em;
            position: relative;
        }
        .detail-content p::first-letter {
            font-size: 1.5rem;
            color: #8b5a2b;
            font-weight: bold;
        }

        /* 详情页侧边栏：碎片拼贴区 */
        .detail-sidebar {
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        .sidebar-item {
            background: rgba(255,255,255,0.9);
            border-radius: 6px;
            padding: 20px;
            box-shadow: 3px 3px 8px rgba(0,0,0,0.08);
            transform: rotate(1deg);
            transition: all 0.3s ease;
        }
        .sidebar-item:nth-child(2) { transform: rotate(-1.5deg); }
        .sidebar-item:nth-child(3) { transform: rotate(0.8deg); }
        .sidebar-item:hover {
            transform: rotate(0deg) scale(1.03);
            box-shadow: 4px 4px 10px rgba(0,0,0,0.12);
        }
        .sidebar-item h3 {
            font-size: 1.2rem;
            color: #8b5a2b;
            margin-bottom: 10px;
            border-bottom: 1px dashed #d4b898;
            padding-bottom: 5px;
        }
        .sidebar-item p {
            color: #555;
            font-size: 0.9rem;
            line-height: 1.6;
        }

        /* 初始引导页 */
        .guide-page {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: rgba(255,255,255,0.85);
            z-index: 20;
            text-align: center;
            padding: 40px;
        }
        .guide-page h1 {
            font-size: 3rem;
            color: #8b5a2b;
            margin-bottom: 20px;
            text-shadow: 2px 2px 0 rgba(212,184,152,0.2);
        }
        .guide-page p {
            font-size: 1.2rem;
            color: #666;
            margin-bottom: 40px;
        }
        .guide-page .tip {
            font-size: 1rem;
            color: #8b5a2b;
            font-style: italic;
            animation: blink 2s infinite;
        }
        @keyframes blink {
            0%, 100% {opacity: 1;}
            50% {opacity: 0.5;}
        }

        /* 响应式适配 */
        @media (max-width: 1200px) {
            .page-detail {
                grid-template-columns: 1fr;
                grid-template-rows: auto auto;
                padding: 30px;
            }
            .detail-sidebar {
                flex-direction: row;
                flex-wrap: wrap;
                justify-content: center;
            }
            .sidebar-item {
                width: 45%;
            }
            .theme-sidebar {
                width: 60px;
            }
            .theme-sidebar:hover {
                width: 280px;
            }
            .main-content {
                margin-left: 60px;
            }
            .theme-sidebar:hover + .main-content {
                margin-left: 280px;
            }
        }
        @media (max-width: 768px) {
            .theme-sidebar {
                width: 50px;
            }
            .theme-sidebar:hover {
                width: 250px;
            }
            .main-content {
                margin-left: 50px;
            }
            .theme-sidebar:hover + .main-content {
                margin-left: 250px;
            }
            .detail-title {
                font-size: 2rem;
            }
            .sidebar-item {
                width: 100%;
            }
            .page-detail {
                padding: 20px;
            }
            .detail-main {
                padding: 25px;
            }
            .guide-page h1 {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>
    <!-- 左侧主题侧边栏（半隐藏+悬浮展开） -->
    <div class="theme-sidebar">
        <div class="sidebar-header">
            <h1>孟州一中回忆录</h1>
            <p>青春碎片集</p>
        </div>
        <div class="theme-list">
            <div class="theme-card" data-page="1">
                <h2>主题1（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="2">
                <h2>主题2（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="3">
                <h2>主题3（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="4">
                <h2>主题4（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="5">
                <h2>主题5（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="6">
                <h2>主题6（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="7">
                <h2>主题7（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="8">
                <h2>主题8（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="9">
                <h2>主题9（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="10">
                <h2>主题10（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="11">
                <h2>主题11（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="12">
                <h2>主题12（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="13">
                <h2>主题13（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="14">
                <h2>主题14（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="15">
                <h2>主题15（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
            <div class="theme-card" data-page="16">
                <h2>主题16（待填）</h2>
                <p>简短简介（待填）</p>
            </div>
        </div>
    </div>

    <!-- 主内容区（详情页容器） -->
    <div class="main-content">
        <!-- 初始引导页 -->
        <div class="guide-page" id="guidePage">
            <h1>欢迎来到高中回忆录</h1>
            <p>将鼠标移至左侧边缘，展开主题列表</p>
            <div class="tip">点击主题卡片查看详细回忆 ←</div>
        </div>

        <!-- 主题1详情页 -->
        <div class="page-detail" id="page1">
            <div class="detail-main">
                <h2 class="detail-title">主题1（待填）</h2>
                <p class="detail-brief">主题简介（待填）</p>
                <div class="detail-content">
                    <p>详细回忆内容（待填）：比如高一军训的某个瞬间，阳光把迷彩服晒得发烫，教官喊着口号时喉结滚动，身边同学悄悄递来的薄荷糖，融化在舌尖的清凉和彼此交换的眼神，成了盛夏最清晰的印记。</p>
                    <p>可加入更多感官细节：比如操场边的白杨树沙沙作响，食堂飘来的馒头香气，夜晚宿舍里偷偷亮起的手电筒光，这些碎片拼凑起来，就是最鲜活的青春。</p>
                    <p>段落间用自然过渡，每段聚焦一个场景或情绪，让读者跟着文字重回当时的时光。</p>
                </div>
            </div>
            <div class="detail-sidebar">
                <div class="sidebar-item">
                    <h3>关键场景</h3>
                    <p>（待填）：比如“军训汇演那天的暴雨”“宿舍第一次卧谈会”</p>
                </div>
                <div class="sidebar-item">
                    <h3>难忘瞬间</h3>
                    <p>（待填）：比如“同学帮我背沉重的行李”“教官偷偷给我们买冰饮”</p>
                </div>
                <div class="sidebar-item">
                    <h3>青春印记</h3>
                    <p>（待填）：比如“晒黑的皮肤”“磨破的军训鞋”“写满祝福的笔记本”</p>
                </div>
            </div>
        </div>

        <!-- 主题2详情页 -->
        <div class="page-detail" id="page2">
            <div class="detail-main">
                <h2 class="detail-title">主题2（待填）</h2>
                <p class="detail-brief">主题简介（待填）</p>
                <div class="detail-content">
                    <p>详细回忆内容（待填）：比如高二运动会的800米跑，站在起跑线时的心跳声，耳边同学的呐喊像潮水般涌来，跑到后半程时的体力不支，同桌突然从跑道边冲出来陪跑，那句“坚持住，我陪你”成了至今想起仍会发热的话语。</p>
                    <p>可补充场景细节：比如跑道旁的加油牌上歪歪扭扭的字迹，冲过终点后递来的温水和毛巾，全班同学围过来欢呼的热闹，这些画面构成了青春里最热血的篇章。</p>
                </div>
            </div>
            <div class="detail-sidebar">
                <div class="sidebar-item">
                    <h3>关键场景</h3>
                    <p>（待填）：比如“运动会开幕式的方阵”“冲刺终点的瞬间”</p>
                </div>
                <div class="sidebar-item">
                    <h3>难忘瞬间</h3>
                    <p>（待填）：比如“全班一起制作加油牌”“同学受伤仍坚持比赛”</p>
                </div>
                <div class="sidebar-item">
                    <h3>青春印记</h3>
                    <p>（待填）：比如“沾满汗水的运动服”“获奖的集体奖状”“合影里的笑脸”</p>
                </div>
            </div>
        </div>

        <!-- 主题3-16详情页可复制主题2结构，修改id为page3-page16即可 -->
    </div>

    <script>
        // 核心元素获取
        const themeCards = document.querySelectorAll('.theme-card');
        const detailPages = document.querySelectorAll('.page-detail');
        const guidePage = document.getElementById('guidePage');
        let currentPage = null;

        // 隐藏引导页（首次点击主题后）
        function hideGuidePage() {
            guidePage.style.opacity = 0;
            guidePage.style.pointerEvents = 'none';
            setTimeout(() => {
                guidePage.style.display = 'none';
            }, 500);
        }

        // 主题卡片点击切换详情页
        themeCards.forEach(card => {
            card.addEventListener('click', () => {
                // 首次点击隐藏引导页
                if (guidePage.style.display !== 'none') {
                    hideGuidePage();
                }

                // 获取目标页面ID
                const targetPageId = card.getAttribute('data-page');
                const targetPage = document.getElementById(`page${targetPageId}`);

                // 隐藏当前页
                if (currentPage) {
                    currentPage.classList.remove('active');
                }

                // 显示目标页
                targetPage.classList.add('active');
                currentPage = targetPage;

                // 滚动到详情页顶部
                targetPage.scrollTop = 0;
            });
        });

        // 侧边栏悬浮优化（移动端适配）
        if (window.innerWidth < 768) {
            const sidebar = document.querySelector('.theme-sidebar');
            sidebar.addEventListener('touchstart', () => {
                sidebar.style.width = '250px';
                document.querySelector('.main-content').style.marginLeft = '250px';
            });
            sidebar.addEventListener('touchend', () => {
                setTimeout(() => {
                    sidebar.style.width = '50px';
                    document.querySelector('.main-content').style.marginLeft = '50px';
                }, 3000);
            });
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Activity App | Find Your Next Fun Thing</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            background-color: #f0f4f8;
            color: #2d3748;
            line-height: 1.6;
            transition: background 0.3s, color 0.3s;
        }
        /* 暗黑模式样式 */
        body.dark{
            background:#1a202c;
            color:#e2e8f0;
        }
        body.dark .card{
            background:#2d3748;
        }
        body.dark .activity-item{
            background:#4a5568;
            color:#fff;
        }
        body.dark .activity-item:hover{
            background:#60a5fa;
        }

        .container {
            max-width: 1000px;
            margin: 2rem auto;
            padding: 0 1.5rem;
        }

        header {
            text-align: center;
            margin-bottom: 2.5rem;
            position:relative;
        }
        /* 暗黑切换按钮 */
        .dark-toggle{
            position:absolute;
            right:0;top:0;
            padding:6px 12px;
            border-radius:6px;
            border:none;
            background:#718096;
            color:#fff;
            cursor:pointer;
        }

        h1 {
            color: #4a5568;
            font-size: 2.2rem;
            margin-bottom: 0.5rem;
        }
        body.dark h1{color:#e2e8f0;}

        .subtitle {
            color: #718096;
            font-size: 1.1rem;
        }

        /* Main Sections */
        .app-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
            margin-bottom: 2rem;
        }

        .card {
            background: white;
            padding: 1.8rem;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom:1.5rem;
            transition:background 0.3s;
        }

        .card h2 {
            color: #2b6cb0;
            margin-bottom: 1.2rem;
            font-size: 1.5rem;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 0.5rem;
        }
        body.dark .card h2{color:#90cdf4;border-color:#4a5568;}

        /* Buttons */
        .btn {
            padding: 0.8rem 1.5rem;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.3s;
            margin: 0.5rem;
        }

        .btn-random {
            background-color: #38a169;
            color: white;
            font-weight: bold;
        }

        .btn-random:hover {
            background-color: #2f855a;
        }

        .btn-category {
            background-color: #e2e8f0;
            color: #2d3748;
        }
        body.dark .btn-category{background:#4a5568;color:#fff;}

        .btn-category:hover {
            background-color: #cbd5e0;
        }
        body.dark .btn-category:hover{background:#718096;}

        /* Activity Display */
        .activity-display {
            min-height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.3rem;
            font-weight: bold;
            color: #2d3748;
            text-align: center;
            padding: 1rem;
            border: 2px dashed #cbd5e0;
            border-radius: 8px;
            margin: 1rem 0;
        }
        body.dark .activity-display{border-color:#4a5568;color:#e2e8f0;}

        /* Activity Grid (Manual Selection) */
        .activity-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 0.8rem;
            margin-top: 1rem;
            max-height: 300px;
            overflow-y: auto;
            padding-right: 0.5rem;
        }

        .activity-item {
            background: #f7fafc;
            padding: 0.7rem;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.2s;
            text-align: center;
            position:relative;
        }

        .activity-item:hover {
            background: #bee3f8;
        }
        /* 难度小标签 */
        .diff-tag{
            font-size:10px;
            padding:2px 5px;
            border-radius:4px;
            position:absolute;
            top:3px;right:3px;
            background:#ddd;
        }
        .diff-tag.easy{background:#9ae6b4;}
        .diff-tag.mid{background:#f6e05e;}
        .diff-tag.hard{background:#fc8181;color:#fff;}

        /* Smart Recommendations */
        .recommendation-form {
            margin: 1rem 0;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: bold;
        }

        select {
            width: 100%;
            padding: 0.8rem;
            border-radius: 8px;
            border: 1px solid #cbd5e0;
            font-size: 1rem;
        }
        body.dark select{background:#4a5568;color:#fff;border-color:#718096;}

        .btn-recommend {
            background-color: #805ad5;
            color: white;
            width: 100%;
        }

        .btn-recommend:hover {
            background-color:#6b46c1;
        }

        /* 新增收藏&历史卡片 */
        .flex-row{
            display:grid;
            grid-template-columns:1fr 1fr;
            gap:2rem;
        }
        .small-card-title{
            font-size:18px;
            color:#2b6cb0;
            margin-bottom:10px;
            padding-bottom:6px;
            border-bottom:2px solid #eee;
        }
        body.dark .small-card-title{color:#90cdf4;border-color:#4a5568;}
        .list-wrap{
            max-height:220px;
            overflow-y:auto;
        }
        .mini-item{
            padding:6px;
            border-bottom:1px solid #eee;
            display:flex;
            justify-content:space-between;
        }
        body.dark .mini-item{border-color:#4a5568;}
        .del-btn{
            color:red;
            cursor:pointer;
            border:none;background:transparent;
        }

        /* 弹窗样式 */
        .modal-mask{
            position:fixed;
            inset:0;
            background:rgba(0,0,0,0.5);
            display:none;
            align-items:center;
            justify-content:center;
            z-index:999;
        }
        .modal-box{
            background:#fff;
            padding:24px;
            border-radius:12px;
            width:90%;max-width:500px;
        }
        body.dark .modal-box{background:#2d3748;color:#fff;}
        .modal-close{
            float:right;
            border:none;background:#e53e3e;color:#fff;
            padding:4px 10px;border-radius:4px;cursor:pointer;
        }
        .timer-box{
            margin-top:10px;
            text-align:center;
            font-size:24px;
            color:#805ad5;
        }
        body.dark .timer-box{color:#b794f4;}

        /* Responsive Design */
        @media (max-width: 768px) {
            .app-grid {
                grid-template-columns: 1fr;
            }
            .activity-grid {
                grid-template-columns: 1fr;
            }
            .flex-row{grid-template-columns:1fr;}
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <button class="dark-toggle" id="darkBtn">🌓 深色模式</button>
            <h1>🎯 Activity App</h1>
            <p class="subtitle">Find fun, random, or personalized activities in one click!</p>
        </header>

        <!-- Random Activity Generator -->
        <div class="app-grid">
            <div class="card">
                <h2>🎲 Random Activity Generator</h2>
                <p>Click below for a surprise activity!</p>
                <button class="btn btn-random" onclick="generateRandomActivity()">Generate Random Activity</button>
                <div id="randomActivity" class="activity-display">Your random activity will appear here</div>
                <button class="btn btn-category" onclick="saveCurrentToFav()">⭐收藏本次活动</button>
            </div>

            <!-- Smart Recommendations -->
            <div class="card">
                <h2>🧠 Smart Recommendations</h2>
                <div class="recommendation-form">
                    <div class="form-group">
                        <label>Your Mood:</label>
                        <select id="mood">
                            <option value="happy">Happy & Energetic</option>
                            <option value="chill">Chill & Relaxed</option>
                            <option value="creative">Creative & Inspired</option>
                            <option value="social">Social & Outgoing</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Energy Level:</label>
                        <select id="energy">
                            <option value="high">High (Active)</option>
                            <option value="medium">Medium (Balanced)</option>
                            <option value="low">Low (Low Effort)</option>
                        </select>
                    </div>
                </div>
                <button class="btn btn-recommend" onclick="getSmartRecommendation()">Get My Recommendation</button>
                <div id="recommendedActivity" class="activity-display">Your personalized activity will appear here</div>
                <button class="btn btn-category" onclick="saveRecToFav()">⭐收藏推荐活动</button>
            </div>
        </div>

        <!-- Choose Activity Manually -->
        <div class="card">
            <h2>📋 Choose Any Activity</h2>
            <div>
                <button class="btn btn-category" onclick="filterActivities('all')">All</button>
                <button class="btn btn-category" onclick="filterActivities('outdoor')">Outdoor</button>
                <button class="btn btn-category" onclick="filterActivities('indoor')">Indoor</button>
                <button class="btn btn-category" onclick="filterActivities('creative')">Creative</button>
                <button class="btn btn-category" onclick="filterActivities('relax')">Relax</button>
            </div>
            <div id="activityList" class="activity-grid"></div>
        </div>

        <!-- 新增：收藏夹 + 历史记录 双卡片 -->
        <div class="flex-row">
            <div class="card">
                <h3 class="small-card-title">❤️ My Favorite Activities</h3>
                <div class="list-wrap" id="favList"></div>
                <button class="btn btn-category" onclick="clearAllFav()">清空收藏</button>
            </div>
            <div class="card">
                <h3 class="small-card-title">📜 History Record</h3>
                <div class="list-wrap" id="historyList"></div>
                <button class="btn btn-category" onclick="clearAllHistory()">清空历史</button>
            </div>
        </div>
    </div>

    <!-- 活动详情弹窗 -->
    <div class="modal-mask" id="detailModal">
        <div class="modal-box">
            <button class="modal-close" onclick="closeModal()">X</button>
            <h3 id="modalTitle">活动名称</h3>
            <p id="modalDesc" style="margin:12px 0;">活动介绍</p>
            <p>预估耗时：<span id="modalTime"></span></p>
            <div class="timer-box">
                倒计时：<span id="countTimer">00:00</span>
            </div>
            <div style="margin-top:10px;text-align:center;">
                <button class="btn btn-random" onclick="startTimer()">开始计时</button>
                <button class="btn btn-category" onclick="stopTimer()">暂停计时</button>
            </div>
        </div>
    </div>

    <script>
        // ==========【升级：扩充海量活动库，附带详情、耗时、难度】==========
        const activities = {
            outdoor: [
                {name:"Hike a trail",desc:"沿着山间步道徒步，亲近自然，欣赏山林风景",time:"90分钟",diff:"mid"},
                {name:"Ride a bike",desc:"城市或郊野骑行，锻炼身体，打卡路边小店",time:"60分钟",diff:"mid"},
                {name:"Go for a walk",desc:"饭后短途散步，舒缓身心，放松压力",time:"30分钟",diff:"easy"},
                {name:"Play frisbee",desc:"和好友户外飞盘团建，趣味运动互动",time:"70分钟",diff:"mid"},
                {name:"Visit a park",desc:"公园闲逛、喂小动物、晒太阳放空",time:"45分钟",diff:"easy"},
                {name:"Go camping",desc:"野外露营，搭建帐篷、野餐看星空",time:"全天",diff:"hard"},
                {name:"Have a picnic",desc:"准备零食饮品，草坪野餐休闲",time:"120分钟",diff:"easy"},
                {name:"Rock climbing",desc:"户外岩壁攀岩，挑战身体耐力与胆量",time:"150分钟",diff:"hard"},
                {name:"Fishing by lake",desc:"湖边垂钓，静心等待收获",time:"180分钟",diff:"easy"},
                {name:"Outdoor jogging",desc:"晨跑夜跑，有氧运动减脂",time:"40分钟",diff:"mid"},
                {name:"Pick wild fruit",desc:"果园采摘时令水果",time:"100分钟",diff:"easy"},
                {name:"Skateboarding",desc:"平地练习滑板技巧",time:"80分钟",diff:"mid"},
                {name:"Kayak boating",desc:"湖面皮划艇玩水",time:"110分钟",diff:"mid"},
                {name:"Outdoor yoga",desc:"草坪晨间瑜伽舒展身体",time:"50分钟",diff:"easy"},
                {name:"Bird watching",desc:"携带望远镜户外观鸟",time:"130分钟",diff:"easy"},
                {name:"Drive road trip",desc:"短途自驾周边游玩",time:"半天",diff:"mid"},
                {name:"Fly kites",desc:"空旷场地放风筝",time:"60分钟",diff:"easy"},
                {name:"Beach collecting shells",desc:"海边散步捡贝壳",time:"90分钟",diff:"easy"},
                {name:"Outdoor barbecue",desc:"户外炭火烧烤聚餐",time:"150分钟",diff:"mid"},
                {name:"Mountain climbing",desc:"攀登中小型山峰",time:"240分钟",diff:"hard"}
            ],
            indoor: [
                {name:"Watch a movie",desc:"窝沙发看高分电影，搭配零食饮料",time:"120分钟",diff:"easy"},
                {name:"Cook a meal",desc:"研究新菜谱，亲手制作一顿正餐",time:"90分钟",diff:"mid"},
                {name:"Play board games",desc:"桌游聚会，狼人杀、大富翁等",time:"150分钟",diff:"easy"},
                {name:"Do a puzzle",desc:"拼装拼图，锻炼专注力",time:"200分钟",diff:"mid"},
                {name:"Read a book",desc:"沉浸式阅读小说或科普书籍",time:"70分钟",diff:"easy"},
                {name:"Clean your space",desc:"全屋收纳整理、断舍离杂物",time:"110分钟",diff:"mid"},
                {name:"Listen to music",desc:"沉浸式听歌，整理喜欢的歌单",time:"40分钟",diff:"easy"},
                {name:"Learn handmade coffee",desc:"手冲咖啡自学入门",time:"60分钟",diff:"mid"},
                {name:"Indoor workout",desc:"居家无氧健身训练",time:"50分钟",diff:"mid"},
                {name:"Watch documentary",desc:"科普纪录片拓展知识面",time:"95分钟",diff:"easy"},
                {name:"Organize photo album",desc:"整理手机相册打印照片",time:"80分钟",diff:"easy"},
                {name:"Make milk tea",desc:"自制奶茶甜品小食",time:"45分钟",diff:"easy"},
                {name:"Virtual museum tour",desc:"线上云逛全球博物馆",time:"75分钟",diff:"easy"},
                {name:"Learn short video edit",desc:"简单剪辑日常短视频",time:"120分钟",diff:"mid"},
                {name:"Poker games",desc:"休闲纸牌小游戏",time:"85分钟",diff:"easy"},
                {name:"Home spa",desc:"居家护肤泡澡护理",time:"65分钟",diff:"easy"},
                {name:"Build model",desc:"拼装高达/建筑模型",time:"180分钟",diff:"hard"},
                {name:"Try new snack recipe",desc:"自制饼干、小点心",time:"90分钟",diff:"mid"},
                {name:"Online chat party",desc:"线上和朋友连麦闲聊",time:"100分钟",diff:"easy"},
                {name:"Learn sign language",desc:"入门手语小课堂",time:"70分钟",diff:"mid"}
            ],
            creative: [
                {name:"Draw a picture",desc:"水彩/素描自由创作画作",time:"130分钟",diff:"mid"},
                {name:"Write a story",desc:"构思短篇小故事，锻炼文笔",time:"150分钟",diff:"mid"},
                {name:"Do origami",desc:"折纸小动物、立体摆件",time:"55分钟",diff:"easy"},
                {name:"Paint",desc:"丙烯颜料画布创作",time:"180分钟",diff:"hard"},
                {name:"Learn a song",desc:"自学弹唱一首喜欢的歌曲",time:"120分钟",diff:"mid"},
                {name:"Make a craft",desc:"废旧物品改造手工艺品",time:"140分钟",diff:"mid"},
                {name:"Photograph things",desc:"居家静物摄影练习构图",time:"90分钟",diff:"easy"},
                {name:"Handmade candle",desc:"融化蜡油自制香薰蜡烛",time:"80分钟",diff:"mid"},
                {name:"Write poetry",desc:"随笔写诗抒发心情",time:"60分钟",diff:"easy"},
                {name:"Design wallpaper",desc:"手机壁纸原创设计",time:"75分钟",diff:"easy"},
                {name:"Clothes refit",desc:"改造旧衣服款式",time:"110分钟",diff:"mid"},
                {name:"Pottery making",desc:"软陶捏制小摆件",time:"160分钟",diff:"hard"},
                {name:"Comic sketch",desc:"手绘短篇四格漫画",time:"190分钟",diff:"hard"},
                {name:"DIY notebook",desc:"手工装订手账本",time:"100分钟",diff:"mid"},
                {name:"Create playlist",desc:"按情绪分类定制歌单",time:"40分钟",diff:"easy"},
                {name:"Calligraphy practice",desc:"硬笔/软笔练字",time:"65分钟",diff:"easy"},
                {name:"Make greeting card",desc:"手绘节日贺卡",time:"50分钟",diff:"easy"},
                {name:"Mix essential oil",desc:"调配自用香薰精油",time:"70分钟",diff:"mid"},
                {name:"Write travel plan",desc:"原创旅行攻略文案",time:"85分钟",diff:"easy"},
                {name:"Digital drawing",desc:"平板数位板画画",time:"170分钟",diff:"hard"}
            ],
            relax: [
                {name:"Meditate",desc:"静坐冥想放空大脑，缓解焦虑",time:"30分钟",diff:"easy"},
                {name:"Take a bath",desc:"香氛泡澡放松身体疲惫",time:"45分钟",diff:"easy"},
                {name:"Nap",desc:"午后小憩恢复精力",time:"25分钟",diff:"easy"},
                {name:"Stretch",desc:"全身拉伸放松肌肉酸痛",time:"20分钟",diff:"easy"},
                {name:"Sip tea",desc:"慢品茶看书放空",time:"35分钟",diff:"easy"},
                {name:"Daydream",desc:"窝着发呆畅想趣事",time:"任意",diff:"easy"},
                {name:"Do yoga",desc:"舒缓阴瑜伽放松身心",time:"50分钟",diff:"mid"},
                {name:"Aromatherapy rest",desc:"香薰陪伴闭目静养",time:"40分钟",diff:"easy"},
                {name:"Listen ASMR",desc:"助眠白噪音放松神经",time:"60分钟",diff:"easy"},
                {name:"Foot soak",desc:"泡脚养生舒缓疲劳",time:"30分钟",diff:"easy"},
                {name:"Slow breathing exercise",desc:"深呼吸减压练习",time:"15分钟",diff:"easy"},
                {name:"Plant care",desc:"给绿植浇水修剪",time:"25分钟",diff:"easy"},
                {name:"Cloud watching",desc:"窗边躺着看云变化",time:"任意",diff:"easy"},
                {name:"Warm drink making",desc:"煮热可可、花果茶",time:"20分钟",diff:"easy"},
                {name:"Silent reading",desc:"不带手机安静看书",time:"55分钟",diff:"easy"},
                {name:"Eye care rest",desc:"热敷眼睛缓解用眼疲劳",time:"15分钟",diff:"easy"},
                {name:"Slow stroll indoor",desc:"家中缓步放松走动",time:"20分钟",diff:"easy"},
                {name:"Soft music rest",desc:"轻音乐陪伴瘫坐休息",time:"70分钟",diff:"easy"},
                {name:"Herbal tea brewing",desc:"搭配花草养生茶",time:"30分钟",diff:"easy"},
                {name:"Guided sleep meditation",desc:"跟随引导音频睡前放松",time:"45分钟",diff:"easy"}
            ],
            all: []
        };

        // 合并所有活动进all数组
        activities.all = [
            ...activities.outdoor,
            ...activities.indoor,
            ...activities.creative,
            ...activities.relax
        ];

        // ==========全局变量：收藏、历史、计时器、当前选中活动==========
        let favArr = JSON.parse(localStorage.getItem('activityFav')) || [];
        let historyArr = JSON.parse(localStorage.getItem('activityHistory')) || [];
        let currentPickAct = null;
        let timerInterval = null;
        let countSec = 0;

        // 页面初始化
        window.onload = function() {
            filterActivities('all');
            renderFav();
            renderHistory();
        };

        // 1.随机生成活动
        function generateRandomActivity() {
            const randomIndex = Math.floor(Math.random() * activities.all.length);
            const randomAct = activities.all[randomIndex];
            currentPickAct = randomAct;
            document.getElementById("randomActivity").textContent = `✅ ${randomAct.name}`;
            saveToHistory(randomAct.name);
        }

        // 2.分类筛选渲染活动列表
        function filterActivities(category) {
            const list = document.getElementById("activityList");
            list.innerHTML = "";
            const arr = activities[category];
            arr.forEach(item => {
                const div = document.createElement("div");
                div.className = "activity-item";
                // 难度标签
                let diffClass = '';
                let diffText = '';
                if(item.diff==='easy'){diffClass='easy';diffText='简单'}
                else if(item.diff==='mid'){diffClass='mid';diffText='中等'}
                else{diffClass='hard';diffText='困难'}
                div.innerHTML = `${item.name}<span class="diff-tag ${diffClass}">${diffText}</span>`;
                // 点击弹窗看详情
                div.onclick = ()=>{
                    currentPickAct = item;
                    openModal(item);
                };
                list.appendChild(div);
            });
        }

        // 3.智能推荐
        function getSmartRecommendation() {
            const mood = document.getElementById("mood").value;
            const energy = document.getElementById("energy").value;
            let pool = [];
            if (mood === "happy" && energy === "high") pool = activities.outdoor;
            else if (mood === "chill" && energy === "low") pool = activities.relax;
            else if (mood === "creative" && energy === "medium") pool = activities.creative;
            else if (mood === "social" && energy === "high") {
                const custom = {name:"Hang out with friends / Play a group game",desc:"线下和好友聚会玩耍，桌游聚餐",time:"半天",diff:"mid"};
                currentPickAct = custom;
                document.getElementById("recommendedActivity").textContent = `✨ ${custom.name}`;
                saveToHistory(custom.name);
                return;
            } else if (energy === "low") pool = [...activities.relax, ...activities.indoor];
            else pool = activities.all;

            const res = pickRandom(pool);
            currentPickAct = res;
            document.getElementById("recommendedActivity").textContent = `✨ ${res.name}`;
            saveToHistory(res.name);
        }
        function pickRandom(arr){
            return arr[Math.floor(Math.random()*arr.length)];
        }

        // ==========收藏相关函数==========
        function saveCurrentToFav(){
            if(!currentPickAct) return alert("暂无选中活动！");
            saveFav(currentPickAct.name);
        }
        function saveRecToFav(){
            if(!currentPickAct) return alert("暂无推荐活动！");
            saveFav(currentPickAct.name);
        }
        function saveFav(name){
            if(favArr.includes(name)) return alert("已经收藏过啦！");
            favArr.push(name);
            localStorage.setItem('activityFav',JSON.stringify(favArr));
            renderFav();
            alert("收藏成功");
        }
        function renderFav(){
            const wrap = document.getElementById('favList');
            wrap.innerHTML = '';
            favArr.forEach((item,idx)=>{
                const div = document.createElement('div');
                div.className='mini-item';
                div.innerHTML = `${item}<button class="del-btn" onclick="delFav(${idx})">×</button>`;
                wrap.appendChild(div);
            })
        }
        function delFav(idx){
            favArr.splice(idx,1);
            localStorage.setItem('activityFav',JSON.stringify(favArr));
            renderFav();
        }
        function clearAllFav(){
            favArr=[];
            localStorage.removeItem('activityFav');
            renderFav();
        }

        // ==========历史记录==========
        function saveToHistory(name){
            historyArr.unshift(name);
            if(historyArr.length>20) historyArr.pop(); //最多存20条
            localStorage.setItem('activityHistory',JSON.stringify(historyArr));
            renderHistory();
        }
        function renderHistory(){
            const wrap = document.getElementById('historyList');
            wrap.innerHTML = '';
            historyArr.forEach((item,idx)=>{
                const div = document.createElement('div');
                div.className='mini-item';
                div.innerHTML = `${item}<button class="del-btn" onclick="delHis(${idx})">×</button>`;
                wrap.appendChild(div);
            })
        }
        function delHis(idx){
            historyArr.splice(idx,1);
            localStorage.setItem('activityHistory',JSON.stringify(historyArr));
            renderHistory();
        }
        function clearAllHistory(){
            historyArr=[];
            localStorage.removeItem('activityHistory');
            renderHistory();
        }

        // ==========弹窗&计时器==========
        function openModal(act){
            document.getElementById('modalTitle').innerText = act.name;
            document.getElementById('modalDesc').innerText = act.desc;
            document.getElementById('modalTime').innerText = act.time;
            document.getElementById('detailModal').style.display='flex';
            //重置计时
            stopTimer();
            countSec=0;
            updateTimerView();
        }
        function closeModal(){
            document.getElementById('detailModal').style.display='none';
            stopTimer();
        }
        function startTimer(){
            stopTimer();
            timerInterval = setInterval(()=>{
                countSec++;
                updateTimerView();
            },1000)
        }
        function stopTimer(){
            clearInterval(timerInterval);
        }
        function updateTimerView(){
            let m = Math.floor(countSec/60).toString().padStart(2,'0');
            let s = (countSec%60).toString().padStart(2,'0');
            document.getElementById('countTimer').innerText = `${m}:${s}`;
        }

        // ==========暗黑模式切换==========
        document.getElementById('darkBtn').onclick = function(){
            document.body.classList.toggle('dark');
            this.innerText = document.body.classList.contains('dark') ? "☀️浅色模式":"🌓深色模式";
        }
    </script>
</body>
</html>

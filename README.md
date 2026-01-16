[ai_studio_code (2).html](https://github.com/user-attachments/files/24657999/ai_studio_code.2.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>市场营销期中考试（带跳转功能版）</title>
    <style>
        :root {
            --primary-blue: #4a90e2;
            --light-bg: #f5f7fa;
            --text-color: #333;
            --correct-color: #2ecc71;
            --wrong-color: #e74c3c;
            --option-bg: #fff;
            --option-hover: #f0f4f8;
        }

        body {
            font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
            background-color: var(--light-bg);
            margin: 0;
            padding: 20px;
            color: var(--text-color);
            display: flex;
            justify-content: center;
        }

        .quiz-container {
            width: 100%;
            max-width: 800px;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            padding: 30px;
            position: relative;
        }

        .header {
            border-bottom: 2px solid #f0f0f0;
            padding-bottom: 20px;
            margin-bottom: 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 20px;
            margin: 0;
            color: var(--primary-blue);
        }

        /* 进度条改为可点击样式 */
        .progress-box {
            font-size: 14px;
            color: #fff;
            background: var(--primary-blue);
            padding: 6px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: opacity 0.2s;
            display: flex;
            align-items: center;
            gap: 5px;
            box-shadow: 0 2px 5px rgba(74, 144, 226, 0.3);
        }
        .progress-box:hover {
            opacity: 0.9;
        }
        .progress-box::after {
            content: "▼";
            font-size: 10px;
        }

        /* 题目区域 */
        .question-card {
            margin-bottom: 20px;
            min-height: 200px;
        }

        .question-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 25px;
            line-height: 1.6;
        }

        .tag {
            background-color: var(--primary-blue);
            color: white;
            font-size: 12px;
            padding: 3px 8px;
            border-radius: 4px;
            margin-right: 10px;
            vertical-align: 2px;
        }

        /* 选项列表 */
        .options {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .option-btn {
            background: var(--option-bg);
            border: 2px solid #eef2f7;
            padding: 15px 20px;
            border-radius: 8px;
            text-align: left;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.2s;
            display: flex;
            align-items: center;
        }

        .option-btn:hover:not(.disabled) {
            background-color: var(--option-hover);
            border-color: var(--primary-blue);
        }

        .option-letter {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-weight: bold;
            color: #666;
            flex-shrink: 0;
        }

        /* 答案状态 */
        .option-btn.correct {
            background-color: #eafaf1;
            border-color: var(--correct-color);
            color: #155724;
        }
        .option-btn.correct .option-letter {
            background-color: var(--correct-color);
            color: white;
        }

        .option-btn.wrong {
            background-color: #fdeaea;
            border-color: var(--wrong-color);
            color: #721c24;
        }
        .option-btn.wrong .option-letter {
            background-color: var(--wrong-color);
            color: white;
        }

        .disabled {
            pointer-events: none;
        }

        /* 解析栏 */
        .explanation {
            margin-top: 20px;
            padding: 15px;
            background-color: #fff8e1;
            border-left: 4px solid #ffc107;
            border-radius: 4px;
            display: none;
            animation: fadeIn 0.3s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 底部导航 */
        .footer {
            margin-top: 40px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid #f0f0f0;
            padding-top: 20px;
        }

        .nav-btn {
            padding: 12px 30px;
            border-radius: 25px;
            border: none;
            cursor: pointer;
            font-size: 15px;
            font-weight: bold;
            transition: opacity 0.2s;
        }

        .btn-prev {
            background-color: #e0e0e0;
            color: #555;
        }
        .btn-prev:hover { background-color: #d0d0d0; }
        .btn-prev:disabled { opacity: 0.5; cursor: not-allowed; }

        .btn-next {
            background-color: var(--primary-blue);
            color: white;
        }
        .btn-next:hover { opacity: 0.9; }

        /* 结算页面 */
        #result-page {
            display: none;
            text-align: center;
            padding: 50px 20px;
        }
        .score-circle {
            width: 150px;
            height: 150px;
            border-radius: 50%;
            background: var(--primary-blue);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            font-weight: bold;
            margin: 0 auto 30px;
            box-shadow: 0 10px 25px rgba(74, 144, 226, 0.3);
        }
        
        /* === 弹窗样式 === */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background-color: white;
            padding: 25px;
            border-radius: 12px;
            width: 90%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
            position: relative;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .modal-header h3 { margin: 0; }
        .close-btn {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: #666;
        }

        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(45px, 1fr));
            gap: 10px;
        }

        .grid-item {
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #f0f0f0;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            color: #666;
            border: 2px solid transparent;
        }

        .grid-item:hover { background-color: #e0e0e0; }
        
        /* 题号状态颜色 */
        .grid-item.current {
            border-color: var(--primary-blue);
            color: var(--primary-blue);
            background-color: #fff;
        }
        .grid-item.answered-correct {
            background-color: var(--correct-color);
            color: white;
        }
        .grid-item.answered-wrong {
            background-color: var(--wrong-color);
            color: white;
        }
    </style>
</head>
<body>

    <div class="quiz-container" id="quiz-page">
        <div class="header">
            <h1>市场营销期中考试</h1>
            <!-- 点击此处打开题号板 -->
            <div class="progress-box" id="progress-text" onclick="openModal()" title="点击跳转题目">
                1 / 100
            </div>
        </div>

        <div id="question-container">
            <!-- 题目内容由JS加载 -->
        </div>

        <div class="explanation" id="explanation">
            <strong>✅ 正确答案：<span id="correct-answer-text" style="color:#2ecc71; font-size:18px;"></span></strong>
        </div>

        <div class="footer">
            <button class="nav-btn btn-prev" id="prev-btn" onclick="prevQuestion()" disabled>上一题</button>
            <div style="flex-grow:1"></div>
            <button class="nav-btn btn-next" id="next-btn" onclick="nextQuestion()">下一题 ➜</button>
        </div>
    </div>

    <!-- 结果页 -->
    <div class="quiz-container" id="result-page">
        <div class="score-circle" id="final-score">0</div>
        <h2>考试结束！</h2>
        <p style="color:#666; margin-bottom:30px">您可以重新开始或关闭页面。</p>
        <button class="nav-btn btn-next" style="display:inline-block" onclick="location.reload()">再做一次</button>
    </div>

    <!-- 选题弹窗 -->
    <div id="jump-modal" class="modal" onclick="closeModal(event)">
        <div class="modal-content">
            <div class="modal-header">
                <h3>跳转至题目</h3>
                <button class="close-btn" onclick="closeModalDirect()">&times;</button>
            </div>
            <div class="grid-container" id="question-grid">
                <!-- JS生成格子 -->
            </div>
        </div>
    </div>

    <script>
        // ==========================================
        //  完整100题题库数据
        // ==========================================
        const questions = [
             // --- 单选题 (1-44) ---
            {id:1, type:1, q:"制造商品牌又称( )", opts:["私人品牌","商店品牌","全国品牌","分销商品牌"], c:2},
            {id:2, type:1, q:"市场营销学产生于( )", opts:["二战末期","二十世纪五十年代","二十世纪初","十九世纪末"], c:2},
            {id:3, type:1, q:"根据马斯洛的需要层次理论( )", opts:["尊重的需要是最高层次的需要","层次最高的需要最先需要","需要的层次越高越不可缺少","需要的层次越低越重要"], c:3},
            {id:4, type:1, q:"企业奉行“消费中心论”是贯彻( )", opts:["市场营销观念","推销观念","产品观念","生产观念"], c:0},
            {id:5, type:1, q:"自古至今许多经营者奉行“酒好不怕巷子深”的经商之道,这种市场营销管理哲学属于( )", opts:["生产观念","市场营销观念","推销观念","产品观念"], c:3},
            {id:6, type:1, q:"文化具有以下几种特点,其中错误的是( )", opts:["社会文化的共享性,规范性","文化的影响是有形的","社会文化既有稳定性,又有可变性","文化是后天习得的"], c:1},
            {id:7, type:1, q:"目标市场营销的步骤:一是市场细分,二是选择目标市场,三是进行( )", opts:["推销","做广告","竞争","市场定位"], c:3},
            {id:8, type:1, q:"AIDMA模型中的“I”是指( )", opts:["interest","impact","important","input"], c:0},
            {id:9, type:1, q:"对不愿接受访问的对象最适宜采用的调查方式是( )", opts:["上门调查","邮寄问卷","电话访问","人员访问"], c:1},
            {id:10, type:1, q:"推销观念和营销观念最本质的区别在于( )", opts:["推销观念考虑如何把产品变成现金...","推销观念产生于卖方市场...","推销观念已经不适用于当今市场...","推销观念注重卖方需要,而营销观念则注重买方需要"], c:3},
            {id:11, type:1, q:"以下各项中,属于影响消费者需求变化的最活跃因素的是( )", opts:["个人可支配收入","个人可随意支配收入","人均国民生产总值","个人收入"], c:1},
            {id:12, type:1, q:"企业对市场上已有的某种畅销产品稍做部分改动,标出新品牌的产品称为( )", opts:["新产品线","新产品组合","仿制新产品","全新产品"], c:2},
            {id:13, type:1, q:"维护性营销是针对( )状态实行的", opts:["潜在需求","过度需求","饱和需求","无需求"], c:2},
            {id:14, type:1, q:"人们有能力支付并愿意购买某个具体产品的欲望叫作( )", opts:["满足","需求","欲望","需要"], c:1},
            {id:15, type:1, q:"当企业生产经营多种产品或多个品牌时,应采取( )型管理组织结构", opts:["地区","市场","职能","产品"], c:3},
            {id:16, type:1, q:"品牌中可以用语言称呼、表达的部分是( )", opts:["品牌名称","品牌标识","品牌","商标"], c:0},
            {id:17, type:1, q:"市场营销观念的突出特征是( )", opts:["以产品价格为中心","以产品质量为中心","以消费者需求为中心","以产品产量为中心"], c:2},
            {id:18, type:1, q:"制约顾客购买行为的最基本因素是( )", opts:["文化因素","个人因素","经济因素","社会因素"], c:1},
            {id:19, type:1, q:"新产品导入期的策略是( )", opts:["调整价格","重新定位","产品改良","快速渗透"], c:3},
            {id:20, type:1, q:"构成现实市场的基本条件包括消费者(用户)、生产者(供给者)和( )", opts:["行业","信息","服务者","促成双方达成交易的各种条件"], c:3},
            {id:21, type:1, q:"女性购买行为的特点不包括( )", opts:["具有浓厚的情绪和情感色彩","购买行为的被动性和购买目标的明确性","注重商品的具体利益和实用价值","消费倾向的多样化和个性化"], c:1},
            {id:22, type:1, q:"从市场营销的角度看,市场就是( )", opts:["具有购买欲望和支付能力的消费者","交换过程本身","商品交换关系的总和","买卖的场所"], c:0},
            {id:23, type:1, q:"想得到基本需要的具体满足品的愿望叫作( )", opts:["需求","欲望","需要","满足"], c:1},
            {id:24, type:1, q:"代理中间商属于市场营销环境中的( )因素", opts:["竞争","渠道机构","内部环境","公众"], c:1},
            {id:25, type:1, q:"招徕定价是指( )利用部分顾客求廉的心理,特意将某几种商品的价格定得较低吸引顾客。", opts:["生产者","竞争者","批发商","零售商"], c:3},
            {id:26, type:1, q:"( )指人们对社会生活中各种事物的态度和看法。", opts:["价值观念","消费心理","社会习俗","营销道德"], c:0},
            {id:27, type:1, q:"机会水平和威胁水平都很高的企业业务属于( )", opts:["冒险业务","成熟业务","理想业务","困难业务"], c:0},
            {id:28, type:1, q:"市场营销的核心是( )。", opts:["交易","交换","管理","需求"], c:1},
            {id:29, type:1, q:"在相同的产品类型中引入其他品牌,其品牌战略是( )", opts:["品牌延伸","多品牌","新品牌","产品线扩展"], c:1},
            {id:30, type:1, q:"采用维持策略的企业处于( )。", opts:["成熟期","导入期","衰退期","成长期"], c:2},
            {id:31, type:1, q:"消费者购买行为的特点主要有( )", opts:["理性决策","需求弹性小","需求多样性","感情动机"], c:2},
            {id:32, type:1, q:"在现有产品类别中增加新的产品项目,并以同样的品牌名称推出,是( )", opts:["品牌延伸","新品牌","产品线扩展","多品牌"], c:2},
            {id:33, type:1, q:"产生于资本主义国家,处于由“卖方市场”向“买方市场”过渡阶段的营销观念是( )。", opts:["产品观念","推销观念","生产观念","市场营销观念"], c:1},
            {id:34, type:1, q:"最容易导致企业出现市场营销近视的营销观念是( )。", opts:["生产观念","市场营销观念","推销观念","产品观念"], c:3},
            {id:35, type:1, q:"生产家用电器的企业与房地产公司是( )。", opts:["一般竞争者","形式竞争者","愿望竞争者","品牌竞争者"], c:2},
            {id:36, type:1, q:"服务市场营销组合7P比传统4P增加的3P不包括( )。", opts:["有形展示","人员","过程","公共关系"], c:3},
            {id:37, type:1, q:"市场营销的核心是( )", opts:["生产活动","促销活动","销售活动","交换活动"], c:3},
            {id:38, type:1, q:"社会市场营销观念中,所强调的利益应是( )", opts:["企业、消费者与社会的整体利益","企业利益","消费者利益","社会利益"], c:0},
            {id:39, type:1, q:"传统媒体时代的购买模型是( )", opts:["AISA模型","AISAS模型","AIDMA模型","SICAS模型"], c:2},
            {id:40, type:1, q:"某服装店售货员把相同的服装以800元卖给顾客A,以600元卖给顾客B,该服装店的定价属于( )", opts:["顾客差别定价","产品部位差别定价","销售时间差别定价","产品形式差别定价"], c:0},
            {id:41, type:1, q:"按照单位成本加上一定百分比的加成来制定产品销售价格的定价方法称之为( )定价法。", opts:["诊断","成本加成","目标","认知价值"], c:1},
            {id:42, type:1, q:"无需求状态下营销管理的任务是( )", opts:["再生性营销","刺激性营销","开发性营销","维持性营销"], c:1},
            {id:43, type:1, q:"( )认为,消费者喜欢高质量、多功能和具有某种特色的产品,企业应致力于生产高附加值产品,并不断加以改进。", opts:["推销观念","营销观念","产品观念","生产观念"], c:2},
            {id:44, type:1, q:"随行就市法是( )市场的惯用定价方法。", opts:["完全竞争","异质产品","完全垄断","垄断竞争"], c:0},

            // --- 判断题 (45-100) True=0, False=1 ---
            {id:45, type:2, q:"市场营销学是建立在经济学、行为科学和现代管理理论基础上的一门应用科学。", opts:["正确","错误"], c:0},
            {id:46, type:2, q:"企业在促销活动中,如果采用“推”的策略则人员推销的作用最大;如果采用“拉”的策略,则广告的作用更大些。", opts:["正确","错误"], c:0},
            {id:47, type:2, q:"相关群体对消费者的影响因购买的产品不同而不同。对价值小和使用时不易被他人察觉的商品影响小,反之影响大。", opts:["正确","错误"], c:1},
            {id:48, type:2, q:"市场补缺者取胜的关键是专业化的生产与经营。", opts:["正确","错误"], c:0},
            {id:49, type:2, q:"在市场调查活动中,一般只有在第二手资料已经过时、不准确、不完整甚至不可靠的情况下,才花较多时间去收集第一手资料。", opts:["正确","错误"], c:0},
            {id:50, type:2, q:"肯德基的目标市场选择采取的是差异性市场选择策略。", opts:["正确","错误"], c:1},
            {id:51, type:2, q:"直接分销渠道是指生产企业直接将商品销售给消费者,不经过中间环节。", opts:["正确","错误"], c:0},
            {id:52, type:2, q:"国际市场营销与国际贸易没有什么不同。", opts:["正确","错误"], c:1},
            {id:53, type:2, q:"营业推广可以有效地吸引顾客、刺激购买欲望,短期效果显著。", opts:["正确","错误"], c:0},
            {id:54, type:2, q:"汽车和零配件是两种互补产品,对其最好的定价技巧是同高同低,即汽车价格高,零配件的价格也高,反之亦然。", opts:["正确","错误"], c:1},
            {id:55, type:2, q:"生产者市场是消费者市场的基础。", opts:["正确","错误"], c:1},
            {id:56, type:2, q:"劝说性广告主要用于推出新产品,其目的是为产品创造最初的基本需求。", opts:["正确","错误"], c:1},
            {id:57, type:2, q:"市场细分是目标市场营销的第一步。", opts:["正确","错误"], c:0},
            {id:58, type:2, q:"4C营销组合是指产品的组合、价格的组合、渠道的组合、促销的组合。", opts:["正确","错误"], c:1},
            {id:59, type:2, q:"公共关系与其它促销手段一样,其功能仅局限于促销。", opts:["正确","错误"], c:1},
            {id:60, type:2, q:"企业采取种种积极的措施在现有的市场上扩大现有产品的销售,这就是市场开发。", opts:["正确","错误"], c:1},
            {id:61, type:2, q:"营销观念产品导向中的推销观念是在企业产品供不应求的情况下的观念。", opts:["正确","错误"], c:1},
            {id:62, type:2, q:"某零售商收集其电器销售额历史数据属于实地调查的范畴。", opts:["正确","错误"], c:0},
            {id:63, type:2, q:"企业在市场定位过程中,必须要了解竞争产品的市场定位。", opts:["正确","错误"], c:0},
            {id:64, type:2, q:"营销观念产品导向中的生产观念是在企业产品供过于求的情况下的观念。", opts:["正确","错误"], c:1},
            {id:65, type:2, q:"推销员除了要负责为企业推销产品,还应该成为顾客的顾问。", opts:["正确","错误"], c:0},
            {id:66, type:2, q:"1912年,赫杰特齐教授编写的第一本市场营销学的教材的出版,标志着市场营销学已经跨出了大学讲坛,引起了整个社会的兴趣与关注。", opts:["正确","错误"], c:1},
            {id:67, type:2, q:"企业将资金投入本企业内部以产品为单位的各部门的比例就叫做产品投资组合。", opts:["正确","错误"], c:0},
            {id:68, type:2, q:"“需要层次论”的首创者是美国学者菲利普·科特勒教授。", opts:["正确","错误"], c:1},
            {id:69, type:2, q:"市场定位中迎头定位策略具有较大的风险性。", opts:["正确","错误"], c:0},
            {id:70, type:2, q:"直接出口进入国际市场方式与间接出口进入方式相比的优点在于投资少、风险小。", opts:["正确","错误"], c:1},
            {id:71, type:2, q:"目标市场选择完全市场覆盖适用于中小企业。", opts:["正确","错误"], c:1},
            {id:72, type:2, q:"市场营销观念的一个重要特征就是企业将利润作为优先考虑的事情。", opts:["正确","错误"], c:1},
            {id:73, type:2, q:"对于价格弹性小的产品,产品价格可以定低一些。", opts:["正确","错误"], c:1},
            {id:74, type:2, q:"网络营销能够为企业节省巨额促销和流通费用,从而降低了产品的成本和价格。", opts:["正确","错误"], c:0},
            {id:75, type:2, q:"市场细分是根据产品的差异对市场进行的细分。", opts:["正确","错误"], c:1},
            {id:76, type:2, q:"渗透定价策略适合于需求价格弹性较大的产品。", opts:["正确","错误"], c:0},
            {id:77, type:2, q:"一个产品即使内在质量优异,但如果没有完善的服务,实际上仍然是一个不合格的产品。", opts:["正确","错误"], c:0},
            {id:78, type:2, q:"当产品处于引入期时,采用广告和公共关系进行促销的效果最佳。", opts:["正确","错误"], c:0},
            {id:79, type:2, q:"销售渠道的中间环节越少越好。", opts:["正确","错误"], c:1},
            {id:80, type:2, q:"企业的营销控制主要有年度计划控制、赢利能力控制、效率控制与战略控制四种不同的控制方法。", opts:["正确","错误"], c:0},
            {id:81, type:2, q:"企业战略规划的第一个步骤是确定企业目标。", opts:["正确","错误"], c:1},
            {id:82, type:2, q:"职能型组织是一种最普遍的营销组织,其主要优点是可以满足不同消费者的需要。", opts:["正确","错误"], c:1},
            {id:83, type:2, q:"在确定中间商数目的三种可供选择的形式中,对所有各类产品都适用的形式是密集分销。", opts:["正确","错误"], c:1},
            {id:84, type:2, q:"当消费者购买商品以后,其购买的决策过程也随之结束。", opts:["正确","错误"], c:1},
            {id:85, type:2, q:"宝洁公司的品牌策略是多品牌策略。", opts:["正确","错误"], c:0},
            {id:86, type:2, q:"对于经营产品种类多、特点突出、技术服务要求高的企业,一般应建立以产品管理型模式为主的营销组织结构。", opts:["正确","错误"], c:0},
            {id:87, type:2, q:"生产多种产品或拥有多种形式品牌的企业,通常设置市场管理型营销组织。", opts:["正确","错误"], c:1},
            {id:88, type:2, q:"市场领先者战略的核心就是进攻。", opts:["正确","错误"], c:1},
            {id:89, type:2, q:"当消费者对商品的价格反应敏感,且市场规模大。存在着强大的竞争潜力,这时候企业就可以采用撇脂定价策略。", opts:["正确","错误"], c:1},
            {id:90, type:2, q:"盒马鲜生的营销渠道采取的是新零售方式。", opts:["正确","错误"], c:0},
            {id:91, type:2, q:"3V营销模型是指为谁服务、营销渠道和创新。", opts:["正确","错误"], c:1},
            {id:92, type:2, q:"对市场挑战者来说,防御性策略是其最理想的选择。", opts:["正确","错误"], c:1},
            {id:93, type:2, q:"人类的需求是按从较高的需求到较低的需求的先后顺序发展的。", opts:["正确","错误"], c:1},
            {id:94, type:2, q:"市场营销与推销是同一回事。", opts:["正确","错误"], c:1},
            {id:95, type:2, q:"经纪人和代理商是独立的企业,拥有经营产品的全部所有权。", opts:["正确","错误"], c:1},
            {id:96, type:2, q:"劝说性广告是企业应付竞争的有力武器。", opts:["正确","错误"], c:0},
            {id:97, type:2, q:"制定正确地价格能传递适当的信息,是一种对服务的有形展示。", opts:["正确","错误"], c:0},
            {id:98, type:2, q:"一个人的行为、兴趣和观念是由其经济状况决定的。", opts:["正确","错误"], c:1},
            {id:99, type:2, q:"日用品、工业品中的标准件,一般可以采用较长的分销渠道,而高档消费品、工业品中的专用设备或成套机组则应该采用较短的分销渠道。", opts:["正确","错误"], c:0},
            {id:100, type:2, q:"恩格尔系数越高,人们的生活水平越高,反之,恩格尔系数越小,人们的生活水平越低。", opts:["正确","错误"], c:1}
        ];

        // ==========================================
        //  逻辑代码
        // ==========================================
        let currentIndex = 0;
        let score = 0;

        const quizContainer = document.getElementById('question-container');
        const progressText = document.getElementById('progress-text');
        const nextBtn = document.getElementById('next-btn');
        const prevBtn = document.getElementById('prev-btn');
        const explainBox = document.getElementById('explanation');
        const correctAnswerText = document.getElementById('correct-answer-text');
        const modal = document.getElementById('jump-modal');
        const gridContainer = document.getElementById('question-grid');

        function loadQuestion() {
            const data = questions[currentIndex];
            const typeText = data.type === 1 ? "单选题" : "判断题";
            
            progressText.innerText = `${currentIndex + 1} / ${questions.length}`;
            prevBtn.disabled = currentIndex === 0;
            
            // 下一题按钮文字
            nextBtn.innerText = (currentIndex === questions.length - 1) ? "查看结果" : "下一题 ➜";

            let optionsHtml = '';
            const letters = data.type === 1 ? ['A', 'B', 'C', 'D'] : ['√', '×'];

            data.opts.forEach((opt, idx) => {
                const letter = letters[idx];
                optionsHtml += `
                    <div class="option-btn" id="opt-${idx}" onclick="checkAnswer(${idx}, this)">
                        <div class="option-letter">${letter}</div>
                        <div class="option-text">${opt}</div>
                    </div>
                `;
            });

            const html = `
                <div class="question-card">
                    <div class="question-title">
                        <span class="tag">${typeText}</span>
                        ${data.q}
                    </div>
                    <div class="options">
                        ${optionsHtml}
                    </div>
                </div>
            `;
            quizContainer.innerHTML = html;

            // 如果该题已经做过，恢复状态
            if (data.userChoice !== undefined) {
                restoreQuestionState(data);
            } else {
                explainBox.style.display = 'none';
                nextBtn.style.display = 'none'; // 新题隐藏下一题
            }
        }

        // 恢复已答题目的UI
        function restoreQuestionState(data) {
            const options = document.querySelectorAll('.option-btn');
            const letters = data.type === 1 ? ['A', 'B', 'C', 'D'] : ['正确', '错误'];
            
            // 显示解析
            correctAnswerText.innerText = letters[data.c];
            explainBox.style.display = 'block';
            nextBtn.style.display = 'inline-block';

            // 禁用点击
            options.forEach(opt => opt.classList.add('disabled'));

            // 恢复颜色
            const userIdx = data.userChoice;
            const correctIdx = data.c;
            
            if (userIdx === correctIdx) {
                document.getElementById(`opt-${userIdx}`).classList.add('correct');
            } else {
                document.getElementById(`opt-${userIdx}`).classList.add('wrong');
                document.getElementById(`opt-${correctIdx}`).classList.add('correct');
            }
        }

        function checkAnswer(selectedIndex, el) {
            const data = questions[currentIndex];
            // 如果已答过，不允许再点（防止分数重复计算）
            if (data.userChoice !== undefined) return;

            // 记录用户答案
            data.userChoice = selectedIndex;

            const options = document.querySelectorAll('.option-btn');
            
            // 显示解析
            const letters = data.type === 1 ? ['A', 'B', 'C', 'D'] : ['正确', '错误'];
            correctAnswerText.innerText = letters[data.c];
            explainBox.style.display = 'block';
            nextBtn.style.display = 'inline-block';

            if (selectedIndex === data.c) {
                el.classList.add('correct');
                score++;
            } else {
                el.classList.add('wrong');
                options[data.c].classList.add('correct');
            }

            options.forEach(opt => opt.classList.add('disabled'));
        }

        function nextQuestion() {
            if (currentIndex < questions.length - 1) {
                currentIndex++;
                loadQuestion();
            } else {
                showResult();
            }
        }

        function prevQuestion() {
            if (currentIndex > 0) {
                currentIndex--;
                loadQuestion();
            }
        }

        function showResult() {
            document.getElementById('quiz-page').style.display = 'none';
            document.getElementById('result-page').style.display = 'block';
            document.getElementById('final-score').innerText = score;
        }

        // === 跳转功能 ===
        function openModal() {
            renderGrid();
            modal.style.display = 'flex';
        }

        function closeModal(e) {
            if (e.target === modal) {
                modal.style.display = 'none';
            }
        }
        
        function closeModalDirect() {
            modal.style.display = 'none';
        }

        function jumpTo(index) {
            currentIndex = index;
            loadQuestion();
            modal.style.display = 'none';
        }

        function renderGrid() {
            let html = '';
            questions.forEach((q, idx) => {
                let className = 'grid-item';
                if (idx === currentIndex) className += ' current';
                
                // 判断答题状态
                if (q.userChoice !== undefined) {
                    if (q.userChoice === q.c) {
                        className += ' answered-correct';
                    } else {
                        className += ' answered-wrong';
                    }
                }

                html += `<div class="${className}" onclick="jumpTo(${idx})">${idx + 1}</div>`;
            });
            gridContainer.innerHTML = html;
        }

        // 启动
        loadQuestion();

    </script>
</body>
</html>

# SCL90-1<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <!-- 手机端适配核心标签 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SCL-90症状自评量表</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", sans-serif;
        }
        body {
            background-color: #f5f7fa;
            color: #333;
            padding: 15px;
            line-height: 1.6;
        }
        .container {
            max-width: 750px;
            margin: 0 auto;
            background: #fff;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.08);
        }
        .title {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 20px;
            color: #2d6aab;
        }
        .explain {
            font-size: 14px;
            color: #666;
            padding: 10px;
            background: #f8f9fa;
            border-radius: 8px;
            margin-bottom: 20px;
            line-height: 1.8;
        }
        .score-desc {
            font-size: 13px;
            color: #888;
            margin-bottom: 20px;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
        .score-desc span {
            width: 48%;
            margin-bottom: 5px;
        }
        .question-box {
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }
        .question {
            font-size: 15px;
            margin-bottom: 10px;
            display: flex;
            align-items: flex-start;
        }
        .question-num {
            color: #2d6aab;
            font-weight: bold;
            margin-right: 8px;
            min-width: 20px;
            text-align: center;
        }
        .options {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }
        .option {
            flex: 1;
            min-width: 80px;
        }
        input[type="radio"] {
            margin-right: 5px;
            vertical-align: middle;
        }
        label {
            font-size: 14px;
            cursor: pointer;
            color: #555;
        }
        .submit-btn {
            width: 100%;
            height: 45px;
            background: #2d6aab;
            color: #fff;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            margin: 20px 0;
            transition: background 0.3s;
        }
        .submit-btn:hover {
            background: #235796;
        }
        .result-box {
            display: none;
            margin-top: 30px;
            padding-top: 20px;
            border-top: 2px solid #f0f4f8;
        }
        .result-title {
            font-size: 18px;
            font-weight: bold;
            color: #2d6aab;
            margin-bottom: 15px;
        }
        .result-item {
            padding: 10px;
            border-radius: 6px;
            margin-bottom: 10px;
            font-size: 14px;
        }
        .total-score {
            background: #e8f4f8;
            border-left: 4px solid #2d6aab;
        }
        .factor-score {
            background: #f8f9fa;
            border-left: 4px solid #6c9ed2;
        }
        .interpret {
            margin-top: 20px;
            font-size: 14px;
            line-height: 1.8;
            color: #666;
        }
        .interpret h4 {
            font-size: 15px;
            color: #2d6aab;
            margin: 10px 0 5px;
        }
        .warn {
            color: #e63946;
            font-weight: 500;
            margin-top: 10px;
        }
        /* 滚动条优化（手机端） */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-thumb {
            border-radius: 3px;
            background: #ccc;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="title">SCL-90症状自评量表</div>
        <div class="explain">
            【测试说明】<br>
            1. 本量表包含90个项目，评估近1周内的心理状态，请根据自身实际感受选择；<br>
            2. 无需过度思考，按真实感受作答，每个项目仅选1个评分选项；<br>
            3. 结果仅为症状筛查参考，不构成专业心理诊断，分数异常建议咨询心理专业人士。
        </div>
        <div class="score-desc">
            <span>1分：无 → 完全没有该症状</span>
            <span>2分：轻度 → 症状轻微，不影响正常生活</span>
            <span>3分：中度 → 症状明显，对生活有一定干扰</span>
            <span>4分：偏重 → 症状突出，明显影响生活</span>
            <span>5分：严重 → 症状严重，无法正常生活</span>
        </div>

        <!-- 测试题容器 -->
        <div id="question-container"></div>

        <!-- 提交按钮 -->
        <button class="submit-btn" onclick="calculateScore()">提交测试，生成结果</button>

        <!-- 结果报告容器 -->
        <div class="result-box" id="result-box">
            <div class="result-title">📊 你的SCL-90测试结果报告</div>
            <div class="result-item total-score" id="total-score"></div>
            <div class="result-title" style="font-size:16px;margin-top:20px">各维度因子分（满分5分）</div>
            <div id="factor-scores"></div>
            <div class="interpret">
                <h4>📝 结果评分标准解读</h4>
                1. 总分：90-450分，总分≥160分需关注，≥200分建议进一步评估，≥250分需专业心理干预；<br>
                2. 因子分：各维度平均分，≥2分提示该维度存在症状，≥3分提示症状明显，分数越高症状越突出；<br>
                3. 10个维度说明：躯体化（身体不适）、强迫症状（反复思维/行为）、人际关系敏感（社交不适）、抑郁（情绪低落）、焦虑（紧张恐惧）、敌对（易怒冲动）、恐怖（特定恐惧）、偏执（怀疑他人）、精神病性（异常思维）、其他（睡眠/饮食等）。
                <div class="warn">⚠️ 重要提示：本测试结果仅为自我筛查参考，不能替代精神科医生、心理咨询师的专业诊断和治疗。如感到心理不适，请及时寻求专业心理帮助。</div>
            </div>
        </div>
    </div>

    <script>
        // 1. SCL-90 90道测试题
        const questions = [
            "头痛",
            "神经过敏，心中不踏实",
            "头脑中有不必要的想法或字句盘旋",
            "头昏或昏倒",
            "对异性的兴趣减退",
            "对他人责备求全",
            "感到别人能控制您的思想",
            "责怪别人制造麻烦",
            "忘性大",
            "担心自己的衣饰整齐及仪态的端正",
            "容易烦恼和激动",
            "胸痛",
            "害怕空旷的场所或街道",
            "感到自己的精力下降，活动减慢",
            "想结束自己的生命",
            "听到旁人听不到的声音",
            "发抖",
            "感到大多数人都不可信任",
            "胃口不好",
            "容易哭泣",
            "同异性相处时感到害羞不自在",
            "感到受骗、中了圈套或有人想抓住您",
            "无缘无故地突然感到害怕",
            "自己不能控制地大发脾气",
            "怕单独出门",
            "经常责怪自己",
            "腰痛",
            "感到难以完成任务",
            "感到孤独",
            "感到苦闷",
            "过分担忧",
            "对事物不感兴趣",
            "感到害怕",
            "我的感情容易受到伤害",
            "旁人能知道您的私下想法",
            "感到别人不理解您、不同情您",
            "感到人们对您不友好、不喜欢您",
            "做事必须做得很慢以保证做得正确",
            "心跳得很厉害",
            "恶心或胃部不舒服",
            "感到比不上他人",
            "肌肉酸痛",
            "感到有人在监视您、谈论您",
            "难以入睡",
            "做事必须反复检查",
            "难以做出决定",
            "怕乘电车、公共汽车、地铁或火车",
            "呼吸有困难",
            "一阵阵发冷或发热",
            "因为感到害怕而避开某些东西、场合或活动",
            "脑子变空了",
            "身体发麻或刺痛",
            "喉咙有梗塞感",
            "感到前途没有希望",
            "不能集中注意力",
            "感到身体的某一部分软弱无力",
            "感到紧张或容易紧张",
            "感到手或脚发重",
            "想到死亡的事",
            "吃得太多",
            "当别人看着您或谈论您时感到不自在",
            "有一些不属于您自己的想法",
            "有想打人或伤害他人的冲动",
            "醒得太早",
            "必须反复洗手、点数目或触摸某些东西",
            "睡得不稳不深",
            "有想摔坏或破坏东西的冲动",
            "有一些别人没有的想法或念头",
            "感到对别人神经过敏",
            "在商店或电影院等人多的地方感到不自在",
            "感到任何事情都很困难",
            "一阵阵恐惧或惊恐",
            "感到在公共场合吃东西很不舒服",
            "经常与人争论",
            "单独一人时神经很紧张",
            "别人对您的成绩没有做出恰当的评价",
            "即使和别人在一起也感到孤单",
            "感到坐立不安、心神不定",
            "感到自己没有价值",
            "感到熟悉的东西变成陌生或不像是真的",
            "大叫或摔东西",
            "害怕会在公共场合晕倒",
            "感到别人想占您的便宜",
            "为一些有关“性”的想法而很苦恼",
            "您认为应该因为自己的过错而受到惩罚",
            "感到要很快把事情做完",
            "感到自己的身体有严重问题",
            "从未感到和其他人很亲近",
            "感到自己有罪",
            "感到自己的脑子有毛病"
        ];

        // 2. 10个因子对应的题号（索引从0开始，对应90题）
        const factors = {
            躯体化: [0,3,11,26,39,41,47,48,51,52,55,57],
            强迫症状: [2,8,9,27,37,44,45,50,54,64],
            人际关系敏感: [5,20,33,35,36,40,60,68,72],
            抑郁: [4,13,14,19,21,28,29,30,31,53,70,78,84],
            焦虑: [1,16,22,32,38,56,71,77,79,85],
            敌对: [10,23,62,66,73,80],
            恐怖: [12,24,46,49,69,74,81],
            偏执: [7,17,42,67,75,82],
            精神病性: [6,15,34,61,76,83,87,88,89,58],
            其他: [18,43,58,59,63,65,86] // 睡眠、饮食等附加症状
        };

        // 3. 渲染测试题
        const questionContainer = document.getElementById('question-container');
        questions.forEach((q, index) => {
            const questionBox = document.createElement('div');
            questionBox.className = 'question-box';
            questionBox.innerHTML = `
                <div class="question">
                    <span class="question-num">${index+1}</span>
                    <span>${q}</span>
                </div>
                <div class="options">
                    <div class="option"><input type="radio" name="q${index}" value="1" id="q${index}_1"><label for="q${index}_1">1分（无）</label></div>
                    <div class="option"><input type="radio" name="q${index}" value="2" id="q${index}_2"><label for="q${index}_2">2分（轻度）</label></div>
                    <div class="option"><input type="radio" name="q${index}" value="3" id="q${index}_3"><label for="q${index}_3">3分（中度）</label></div>
                    <div class="option"><input type="radio" name="q${index}" value="4" id="q${index}_4"><label for="q${index}_4">4分（偏重）</label></div>
                    <div class="option"><input type="radio" name="q${index}" value="5" id="q${index}_5"><label for="q${index}_5">5分（严重）</label></div>
                </div>
            `;
            questionContainer.appendChild(questionBox);
        });

        // 4. 计算分数并生成结果
        function calculateScore() {
            // 验证是否答完所有题
            let isAllAnswered = true;
            let totalScore = 0;
            const answers = [];

            for (let i = 0; i < 90; i++) {
                const radio = document.querySelector(`input[name="q${i}"]:checked`);
                if (!radio) {
                    isAllAnswered = false;
                    alert(`请完成第${i+1}题的作答！`);
                    break;
                }
                const score = parseInt(radio.value);
                answers.push(score);
                totalScore += score;
            }

            if (!isAllAnswered) return;

            // 计算各因子分
            const factorScores = {};
            for (const [name, indexes] of Object.entries(factors)) {
                const sum = indexes.reduce((acc, idx) => acc + answers[idx], 0);
                factorScores[name] = (sum / indexes.length).toFixed(2); // 保留2位小数
            }

            // 渲染结果
            document.getElementById('result-box').style.display = 'block';
            // 总分
            document.getElementById('total-score').innerHTML = `
                🌟 测试总分：<b>${totalScore}</b>分（满分450分）
                <br>
                ${getTotalScoreDesc(totalScore)}
            `;
            // 各因子分
            const factorScoresEl = document.getElementById('factor-scores');
            factorScoresEl.innerHTML = '';
            for (const [name, score] of Object.entries(factorScores)) {
                const factorItem = document.createElement('div');
                factorItem.className = 'result-item factor-score';
                let desc = '';
                if (score >= 3) {
                    desc = '<span style="color:#e63946;font-weight:bold">→ 症状明显，建议关注</span>';
                } else if (score >= 2) {
                    desc = '<span style="color:#f59e0b;font-weight:bold">→ 存在轻微症状，可自我调节</span>';
                } else {
                    desc = '<span style="color:#10b981;font-weight:bold">→ 无明显症状，状态良好</span>';
                }
                factorItem.innerHTML = `${name}：<b>${score}</b>分 ${desc}`;
                factorScoresEl.appendChild(factorItem);
            }

            // 页面滚动到结果区域
            document.getElementById('result-box').scrollIntoView({ behavior: 'smooth' });
        }

        // 5. 总分解读
        function getTotalScoreDesc(score) {
            if (score < 160) {
                return '✅ 总分正常，近期心理状态良好，无明显不适症状。';
            } else if (score >= 160 && score < 200) {
                return '⚠️ 总分需关注，存在轻微心理不适，建议通过自我调节（如运动、倾诉）改善。';
            } else if (score >= 200 && score < 250) {
                return '🔴 总分偏高，心理不适症状明显，建议寻求心理咨询师专业指导。';
            } else {
                return '🟡 总分显著偏高，心理症状较严重，建议及时前往精神科/心理机构评估诊断。';
            }
        }
    </script>
</body>
</html>

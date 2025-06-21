# -<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>书法风格分辨游戏</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
      padding: 10px;
      font-family: KaiTi, Arial, sans-serif;
      text-align: center;
      background-color: #f8f8f8;
    }

    h1 {
      font-size: 28px;
      margin-bottom: 10px;
    }

    p {
      font-size: 16px;
      margin-bottom: 10px;
    }

    #score {
      font-size: 20px;
      font-weight: bold;
      margin-bottom: 20px;
    }

    #image-container {
      margin: 20px 0;
    }

    #image {
      max-width: 100%;
      height: auto;
      border: 2px solid #333;
      border-radius: 4px;
    }

    #description {
      font-size: 16px;
      color: #555;
      margin: 10px 0;
      max-width: 90%;
    }

    #options {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      margin: 20px 0;
      max-width: 300px;
    }

    button {
      padding: 10px;
      font-size: 16px;
      font-family: inherit;
      background-color: #4a90e2;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    button:hover:not(:disabled) {
      background-color: #357abd;
    }

    button:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }

    #feedback {
      font-size: 18px;
      margin: 10px 0;
    }

    #next-btn {
      margin-top: 10px;
      background-color: #2ecc71;
    }

    #next-btn:hover:not(:disabled) {
      background-color: #27ae60;
    }
  </style>
</head>
<body>
  <h1>书法风格分辨游戏</h1>
  <p>猜猜这幅书法作品是哪位书法家的风格？</p>
  <div id="score">分数: <span id="score-value">0</span></div>
  <div id="image-container">
    <img id="image" src="" alt="书法作品">
    <div id="description"></div>
  </div>
  <div id="options"></div>
  <div id="feedback"></div>
  <button id="next-btn" onclick="nextQuestion()" style="display: none;">下一题</button>

  <script>
    // 书法家数据
    const calligraphers = [
      {
        name: "王羲之",
        image: "https://upload.wikimedia.org/wikipedia/commons/3/3f/Wang_Xizhi_-_Preface_to_the_Orchid_Pavilion.jpg",
        description: "飘逸灵动，行书典范，字体优雅流畅"
      },
      {
        name: "颜真卿",
        image: "https://upload.wikimedia.org/wikipedia/commons/0/0b/Yan_Zhenqing_-_Yan_Qinli_Stele.jpg",
        description: "雄浑刚劲，楷书端庄，气势恢宏"
      },
      {
        name: "柳公权",
        image: "https://upload.wikimedia.org/wikipedia/commons/5/5c/Liu_Gongquan_-_Xuanmi_Pagoda_Stele.jpg",
        description: "骨力遒劲，结构严谨，楷书挺拔"
      },
      {
        name: "赵孟頫",
        image: "https://upload.wikimedia.org/wikipedia/commons/7/7a/Zhao_Mengfu_-_Ode_to_the_Goddess_of_Luo_River.jpg",
        description: "圆润秀美，兼具行楷，风格多变"
      },
      {
        name: "苏轼",
        image: "https://upload.wikimedia.org/wikipedia/commons/4/4f/Su_Shi_-_Cold_Food_Observance.jpg",
        description: "豪放不羁，行书奔放，文人气息浓厚"
      },
      {
        name: "米芾",
        image: "https://upload.wikimedia.org/wikipedia/commons/2/2e/Mi_Fu_-_Shu_Su_Tie.jpg",
        description: "纵横恣肆，行草狂放，笔势生动"
      },
      {
        name: "欧阳询",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/9e/Ouyang_Xun_-_Jiucheng_Palace_Inscription.jpg",
        description: "险绝峻峭，楷书严谨，结构精妙"
      },
      {
        name: "黄庭坚",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/6b/Huang_Tingjian_-_Pine_Wind_Pavilion_Poem.jpg",
        description: "纵逸雄强，行草开张，笔力遒劲"
      }
    ];

    // 初始化变量
    let score = 0;
    let currentQuestion = {};
    const imageElement = document.getElementById('image');
    const descriptionElement = document.getElementById('description');
    const optionsElement = document.getElementById('options');
    const feedbackElement = document.getElementById('feedback');
    const scoreElement = document.getElementById('score-value');
    const nextButton = document.getElementById('next-btn');

    // 启动游戏
    function startGame() {
      nextQuestion();
    }

    // 生成新问题
    function nextQuestion() {
      currentQuestion = calligraphers[Math.floor(Math.random() * calligraphers.length)];
      imageElement.src = currentQuestion.image;
      descriptionElement.textContent = `风格提示：${currentQuestion.description}`;
      optionsElement.innerHTML = '';

      const shuffledCalligraphers = shuffle([...calligraphers]).slice(0, 4);
      shuffledCalligraphers.forEach(calligrapher => {
        const button = document.createElement('button');
        button.textContent = calligrapher.name;
        button.onclick = () => checkAnswer(calligrapher.name);
        optionsElement.appendChild(button);
      });

      feedbackElement.textContent = '';
      nextButton.style.display = 'none';
    }

    // 检查答案
    function checkAnswer(selected) {
      if (selected === currentQuestion.name) {
        score += 10;
        feedbackElement.textContent = '正确！+10分';
        feedbackElement.style.color = 'green';
      } else {
        feedbackElement.textContent = `错误！正确答案是 ${currentQuestion.name}`;
        feedbackElement.style.color = 'red';
      }
      scoreElement.textContent = score;
      nextButton.style.display = 'block';
      optionsElement.querySelectorAll('button').forEach(button => {
        button.disabled = true;
      });
    }

    // 洗牌算法
    function shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }

    // 初始化
    startGame();
  </script>
</body>
</html>

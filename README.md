<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>寻宝游戏</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background-color: #f0f0f0;
    }

    #game-container {
      padding: 20px;
      max-width: 600px;
      margin: auto;
      background-color: #fff;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    #progress {
      margin: 20px 0;
    }

    .step {
      padding: 10px;
      margin: 5px 0;
      background-color: #e0e0e0;
      border-radius: 5px;
      display: inline-block;
      width: 100%;
      text-align: left;
      transition: background-color 0.3s;
    }

    .step.completed {
      background-color: #a0d468;
    }

    #message {
      margin-top: 20px;
      font-size: 1.2em;
      color: #333;
    }

    button {
      padding: 10px 20px;
      font-size: 1em;
      cursor: pointer;
    }
  </style>
</head>

<body>
  <div id="game-container">
    <h1>寻宝游戏</h1>
    <div id="progress">
      <div class="step" id="step1">图书馆</div>
      <div class="step" id="step2" style="display: none;">神庙</div>
      <div class="step" id="step3" style="display: none;">宝藏箱</div>
      <div class="step" id="step4" style="display: none;">宝藏</div>
    </div>
    <button id="start-button">开始游戏</button>
    <div id="message"></div>
  </div>

  <script>
    document.getElementById('start-button').addEventListener('click', async () => {
      const steps = [
        { id: 'step1', text: '在古老的图书馆里找到了第一个线索...', action: getInitialClue },
        { id: 'step2', text: '解码成功!宝藏在一座古老的神庙中...', action: decodeAncientScript },
        { id: 'step3', text: '找到了一个神秘的箱子...', action: searchTemple },
        { id: 'step4', text: '恭喜!你找到了传说中的宝藏!', action: openTreasureBox }
      ];

      let currentStep = 0;
      const messageDiv = document.getElementById('message');
      const progressSteps = document.querySelectorAll('.step');

      function updateProgress() {
        progressSteps.forEach((step, index) => {
          if (index <= currentStep) {
            step.classList.add('completed');
            step.style.display = 'block';
          } else {
            step.classList.remove('completed');
            // Keep subsequent steps hidden for now
            // step.style.display = 'none'; // Uncomment this if you want to hide future steps when resetting
          }
        });
      }

      async function runStep(step) {
        messageDiv.textContent = step.text;
        try {
          await step.action();
          currentStep++;
          updateProgress();
          if (currentStep < steps.length) {
            await runStep(steps[currentStep]);
          } else {
            messageDiv.textContent += ' 游戏结束！';
          }
        } catch (error) {
          messageDiv.textContent += `\n任务失败: ${error}`;
        }
      }

      function getInitialClue() {
        return new Promise((resolve) => {
          setTimeout(() => resolve(), 1000);
        });
      }

      function decodeAncientScript() {
        // Simulate a successful decode without actually needing a clue
        return new Promise((resolve) => {
          setTimeout(() => resolve(), 1500);
        });
      }

      function searchTemple() {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            const random = Math.random();
            if (random < 0.5) {
              reject('糟糕!遇到了神庙守卫!');
            } else {
              resolve();
            }
          }, 2000);
        });
      }

      function openTreasureBox() {
        return new Promise((resolve) => {
          setTimeout(() => resolve(), 1000);
        });
      }

      // Start the game
      runStep(steps[currentStep]);
    });
  </script>
</body>

</html>

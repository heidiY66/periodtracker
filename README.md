<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>姨妈周期记录</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: "Microsoft YaHei", sans-serif;
      padding: 20px;
      background-color: #fff0f5;
    }
    h1 {
      color: #d63384;
    }
    label, input, button {
      display: block;
      margin: 10px 0;
      font-size: 16px;
    }
    .history {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>🌸 姨妈周期记录</h1>

  <label for="start">月经开始日期：</label>
  <input type="date" id="start">

  <label for="end">月经结束日期：</label>
  <input type="date" id="end">

  <button onclick="saveCycle()">保存记录</button>

  <div class="history">
    <h2>📜 历史记录</h2>
    <ul id="historyList"></ul>
  </div>

  <div class="prediction">
    <h2>🔮 下次预测</h2>
    <p id="predictionText">暂无数据</p>
  </div>

  <script>
    const historyList = document.getElementById("historyList");
    const predictionText = document.getElementById("predictionText");

    function saveCycle() {
      const start = document.getElementById("start").value;
      const end = document.getElementById("end").value;
      if (!start || !end) return alert("请填写完整日期");

      const cycle = { start, end };
      let cycles = JSON.parse(localStorage.getItem("cycles") || "[]");
      cycles.push(cycle);
      localStorage.setItem("cycles", JSON.stringify(cycles));
      displayHistory();
      predictNext(cycles);
    }

    function displayHistory() {
      let cycles = JSON.parse(localStorage.getItem("cycles") || "[]");
      historyList.innerHTML = "";
      cycles.forEach((c, i) => {
        const li = document.createElement("li");
        li.textContent = `第 ${i + 1} 次：${c.start} 至 ${c.end}`;
        historyList.appendChild(li);
      });
    }

    function predictNext(cycles) {
      if (cycles.length < 2) {
        predictionText.textContent = "需要至少两次记录才能预测周期";
        return;
      }
      let totalDays = 0;
      for (let i = 1; i < cycles.length; i++) {
        const prev = new Date(cycles[i - 1].start);
        const curr = new Date(cycles[i].start);
        totalDays += (curr - prev) / (1000 * 60 * 60 * 24);
      }
      const avgCycle = Math.round(totalDays / (cycles.length - 1));
      const lastDate = new Date(cycles[cycles.length - 1].start);
      lastDate.setDate(lastDate.getDate() + avgCycle);
      predictionText.textContent = `预计下次开始日期：${lastDate.toISOString().split("T")[0]}（平均周期 ${avgCycle} 天）`;
    }

    // 初始化
    const stored = JSON.parse(localStorage.getItem("cycles") || "[]");
    if (stored.length) {
      displayHistory();
      predictNext(stored);
    }
  </script>
</body>
</html>

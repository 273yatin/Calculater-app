<!DOCTYPE html>  
<html lang="en">  
<head>  
  <meta charset="UTF-8" />  
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>  
  <title>Calculator App</title>  
  <style>  
    :root {  
      /* Dark theme (default) */  
      --bg-gradient: linear-gradient(135deg, #1e3c72, #2a5298);  
      --calc-bg: #222;  
      --display-bg: #000;  
      --display-color: #0f0;  
      --display-shadow: inset 0 0 10px rgba(0, 255, 0, 0.3);  
      --btn-number-bg: #444;  
      --btn-operator-bg: #ff9500;  
      --btn-equals-bg: #28a745;  
      --btn-clear-bg: #dc3545;  
      --text-color: white;  
      --shadow: 0 10px 30px rgba(0, 0, 0, 0.5);  
      --btn-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);  
    }  
  
    [data-theme="light"] {  
      /* Light theme */  
      --bg-gradient: linear-gradient(135deg, #f5f7fa, #c3cfe2);  
      --calc-bg: #fff;  
      --display-bg: #fff;  
      --display-color: #333;  
      --display-shadow: inset 0 0 10px rgba(0, 0, 0, 0.1);  
      --btn-number-bg: #e9ecef;  
      --btn-operator-bg: #ffc107;  
      --btn-equals-bg: #28a745;  
      --btn-clear-bg: #dc3545;  
      --text-color: #333;  
      --shadow: 0 10px 30px rgba(0, 0, 0, 0.1);  
      --btn-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);  
    }  
  
    * {  
      margin: 0;  
      padding: 0;  
      box-sizing: border-box;  
      font-family: 'Segoe UI', sans-serif;  
    }  
  
    body {  
      display: flex;  
      justify-content: center;  
      align-items: center;  
      min-height: 100vh;  
      background: var(--bg-gradient);  
      transition: background 0.3s ease;  
    }  
  
    .calculator {  
      background: var(--calc-bg);  
      padding: 20px;  
      border-radius: 20px;  
      box-shadow: var(--shadow);  
      width: 320px;  
      transition: background 0.3s ease, box-shadow 0.3s ease;  
      position: relative;  
    }  
  
    .theme-toggle {  
      position: absolute;  
      top: 10px;  
      right: 10px;  
      background: var(--btn-number-bg);  
      color: var(--text-color);  
      border: none;  
      border-radius: 50%;  
      width: 40px;  
      height: 40px;  
      cursor: pointer;  
      font-size: 1.2rem;  
      transition: all 0.3s ease;  
      box-shadow: var(--btn-shadow);  
    }  
  
    .theme-toggle:hover {  
      transform: scale(1.1);  
    }  
  
    .display {  
      width: 100%;  
      height: 80px;  
      background: var(--display-bg);  
      color: var(--display-color);  
      font-size: 2rem;  
      text-align: right;  
      padding: 0 15px;  
      border-radius: 10px;  
      margin-bottom: 15px;  
      overflow: hidden;  
      display: flex;  
      align-items: center;  
      justify-content: flex-end;  
      box-shadow: var(--display-shadow);  
      transition: background 0.3s ease, color 0.3s ease, box-shadow 0.3s ease;  
    }  
  
    .buttons {  
      display: grid;  
      grid-template-columns: repeat(4, 1fr);  
      gap: 10px;  
    }  
  
    button {  
      padding: 20px;  
      font-size: 1.5rem;  
      font-weight: bold;  
      border: none;  
      border-radius: 12px;  
      cursor: pointer;  
      transition: all 0.2s;  
      box-shadow: var(--btn-shadow);  
      color: var(--text-color);  
    }  
  
    button:active {  
      transform: translateY(2px);  
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);  
    }  
  
    .number, .decimal {  
      background: var(--btn-number-bg);  
      transition: background 0.3s ease;  
    }  
  
    .operator {  
      background: var(--btn-operator-bg);  
      transition: background 0.3s ease;  
    }  
  
    .equals {  
      background: var(--btn-equals-bg);  
      transition: background 0.3s ease;  
    }  
  
    .clear {  
      background: var(--btn-clear-bg);  
      transition: background 0.3s ease;  
    }  
  
  
    .zero {  
      grid-column: span 2;  
    }  
  
    /* Fix for equals button spanning two rows */  
    .buttons {  
      grid-template-rows: repeat(5, 1fr);  
    }  
  
    .equals {  
      grid-row: span 2;  
    }  
  </style>  
</head>  
<body>  
  
  <div class="calculator">  
    <button class="theme-toggle" onclick="toggleTheme()" id="themeToggle">🌙</button>  
    <div class="display" id="display">0</div>  
    <div class="buttons">  
      <button class="clear" onclick="clearDisplay()">C</button>  
      <button class="clear" onclick="deleteLast()">⌫</button>  
      <button class="operator" onclick="appendToDisplay('/')">/</button>  
      <button class="operator" onclick="appendToDisplay('*')">×</button>  
  
      <button class="number" onclick="appendToDisplay('7')">7</button>  
      <button class="number" onclick="appendToDisplay('8')">8</button>  
      <button class="number" onclick="appendToDisplay('9')">9</button>  
      <button class="operator" onclick="appendToDisplay('-')">-</button>  
  
      <button class="number" onclick="appendToDisplay('4')">4</button>  
      <button class="number" onclick="appendToDisplay('5')">5</button>  
      <button class="number" onclick="appendToDisplay('6')">6</button>  
      <button class="operator" onclick="appendToDisplay('+')">+</button>  
  
      <button class="number" onclick="appendToDisplay('1')">1</button>  
      <button class="number" onclick="appendToDisplay('2')">2</button>  
      <button class="number" onclick="appendToDisplay('3')">3</button>  
      <button class="equals" onclick="calculate()">=</button>  
  
      <button class="number zero" onclick="appendToDisplay('0')">0</button>  
      <button class="decimal" onclick="appendToDisplay('.')">.</button>  
    </div>  
  </div>  
  
  <script>  
    let display = document.getElementById('display');  
    let currentInput = '0';  
    let shouldResetDisplay = false;  
  
    // Theme toggle functionality  
    function toggleTheme() {  
      const body = document.body;  
      const calculator = document.querySelector('.calculator');  
      const toggleBtn = document.getElementById('themeToggle');  
        
      if (body.getAttribute('data-theme') === 'light') {  
        body.removeAttribute('data-theme');  
        toggleBtn.textContent = '🌙';  
        localStorage.setItem('theme', 'dark');  
      } else {  
        body.setAttribute('data-theme', 'light');  
        toggleBtn.textContent = '☀️';  
        localStorage.setItem('theme', 'light');  
      }  
    }  
  
    // Load saved theme on startup  
    function loadTheme() {  
      const savedTheme = localStorage.getItem('theme') || 'dark';  
      const body = document.body;  
      const toggleBtn = document.getElementById('themeToggle');  
        
      if (savedTheme === 'light') {  
        body.setAttribute('data-theme', 'light');  
        toggleBtn.textContent = '☀️';  
      } else {  
        toggleBtn.textContent = '🌙';  
      }  
    }  
  
    function updateDisplay() {  
      display.textContent = currentInput;  
    }  
  
    function appendToDisplay(value) {  
      if (shouldResetDisplay) {  
        currentInput = '0';  
        shouldResetDisplay = false;  
      }  
      if (currentInput === '0' && value !== '.') {  
        currentInput = value;  
      } else {  
        currentInput += value;  
      }  
      updateDisplay();  
    }  
  
    function clearDisplay() {  
      currentInput = '0';  
      updateDisplay();  
    }  
  
    function deleteLast() {  
      if (currentInput.length > 1) {  
        currentInput = currentInput.slice(0, -1);  
      } else {  
        currentInput = '0';  
      }  
      updateDisplay();  
    }  
  
    function calculate() {  
      try {  
        // Replace × with * for eval  
        let expression = currentInput.replace(/×/g, '*');  
        let result = eval(expression);  
  
        // Handle division by zero and invalid expressions  
        if (!isFinite(result)) {  
          currentInput = 'Error';  
        } else {  
          currentInput = parseFloat(result.toFixed(8)).toString();  
        }  
      } catch (e) {  
        currentInput = 'Error';  
      }  
      updateDisplay();  
      shouldResetDisplay = true;  
    }  
  
    // Initialize  
    loadTheme();  
    updateDisplay();  
  </script>  
  
</body>  
</html>  
Light/Dark mode button change  

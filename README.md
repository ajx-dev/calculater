# calculater[Untitled-1.html](https://github.com/user-attachments/files/24447801/Untitled-1.html)
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple Calculator</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <main class="calculator" aria-label="Simple calculator">
    <div class="display" id="display" role="textbox" aria-readonly="true" aria-label="Calculator display">0</div>

    <div class="buttons" role="group" aria-label="Calculator buttons">
      <button class="btn function" data-action="clear" aria-label="Clear (Escape)">C</button>
      <button class="btn function" data-action="backspace" aria-label="Backspace (Backspace)">⌫</button>
      <button class="btn op" data-value="/" aria-label="Divide">÷</button>

      <button class="btn" data-value="7">7</button>
      <button class="btn" data-value="8">8</button>
      <button class="btn" data-value="9">9</button>
      <button class="btn op" data-value="*" aria-label="Multiply">×</button>

      <button class="btn" data-value="4">4</button>
      <button class="btn" data-value="5">5</button>
      <button class="btn" data-value="6">6</button>
      <button class="btn op" data-value="-" aria-label="Subtract">−</button>

      <button class="btn" data-value="1">1</button>
      <button class="btn" data-value="2">2</button>
      <button class="btn" data-value="3">3</button>
      <button class="btn op" data-value="+" aria-label="Add">+</button>

      <button class="btn zero" data-value="0">0</button>
      <button class="btn" data-value=".">.</button>
      <button class="btn equals" data-action="equals" aria-label="Equals (Enter)">=</button>
    </div>
  </main>
[Untitled-2.js](https://github.com/user-attachments/files/24447814/Untitled-2.js)
// Simple calculator logic
(function () {
  const displayEl = document.getElementById('display');
  const buttons = document.querySelectorAll('.btn');

  let current = '0'; // string expression shown
  let lastWasOperator = false;

  function updateDisplay() {
    displayEl.textContent = current;
  }

  function clearAll() {
    current = '0';
    lastWasOperator = false;
    updateDisplay();
  }

  function backspace() {
    if (current.length <= 1) {
      clearAll();
      return;
    }
    current = current.slice(0, -1);
    // prevent dangling operator at end being considered last
    lastWasOperator = /[+\-*/]$/.test(current);
    updateDisplay();
  }

  function appendDigit(d) {
    if (current === '0' && d !== '.') {
      current = d;
    } else {
      // prevent more than one decimal in the current number
      if (d === '.') {
        // find last operator position
        const lastOp = Math.max(
          current.lastIndexOf('+'),
          current.lastIndexOf('-'),
          current.lastIndexOf('*'),
          current.lastIndexOf('/')
        );
        const lastNumber = current.slice(lastOp + 1);
        if (lastNumber.includes('.')) return;
      }
      current += d;
    }
    lastWasOperator = false;
    updateDisplay();
  }

  function appendOperator(op) {
    // replace trailing operator, or ignore if empty
    if (lastWasOperator) {
      current = current.slice(0, -1) + op;
    } else {
      current += op;
    }
    lastWasOperator = true;
    updateDisplay();
  }

  function evaluateExpression() {
    // Avoid evaluating if ends with operator
    if (lastWasOperator) return;
    try {
      // Use a safe Function to evaluate simple arithmetic
      // Only allow digits, operators, dots, and parentheses
      if (!/^[0-9+\-*/().\s]+$/.test(current)) {
        throw new Error('Invalid characters');
      }
      // Replace × ÷ if any (not necessary here but safe)
      const expr = current.replace(/×/g, '*').replace(/÷/g, '/');
      const result = Function('"use strict";return (' + expr + ')')();
      current = (Math.round((result + Number.EPSILON) * 1e12) / 1e12).toString();
      lastWasOperator = false;
      updateDisplay();
    } catch (e) {
      displayEl.textContent = 'Error';
      current = '0';
      lastWasOperator = false;
      setTimeout(updateDisplay, 800);
    }
  }

  // Hook up button clicks
  buttons.forEach(btn => {
    btn.addEventListener('click', () => {
      const v = btn.dataset.value;
      const action = btn.dataset.action;
      if (action === 'clear') return clearAll();
      if (action === 'backspace') return backspace();
      if (action === 'equals') return evaluateExpression();
      if (v >= '0' && v <= '9' || v === '.') return appendDigit(v);
      if (['+', '-', '*', '/'].includes(v)) return appendOperator(v);
    });
  });

  // Keyboard support
  window.addEventListener('keydown', (e) => {
    const key = e.key;
    if ((key >= '0' && key <= '9') || key === '.') {
      appendDigit(key);
      e.preventDefault();
      return;
    }
    if (['+', '-', '*', '/'].includes(key)) {
      appendOperator(key);
      e.preventDefault();
      return;
    }
    if (key === 'Enter' || key === '=') {
      evaluateExpression();
      e.preventDefault();
      return;
    }
    if (key === 'Backspace') {
      backspace();
      e.preventDefault();
      return;
    }
    if (key === 'Escape' || key.toLowerCase() === 'c') {
      clearAll();
      e.preventDefault();
      return;
    }
  });

  // initialize
  updateDisplay();
})();
  <script src="script.js"></script>
</body>
</html>[Untitled-3.css](https://github.com/user-attachments/files/24447817/Untitled-3.css)
:root{
  --bg:#f3f4f6;
  --panel:#ffffff;
  --accent:#1f2937;
  --secondary:#4b5563;
  --btn:#e5e7eb;
  --op:#f59e0b;
  --op-contrast:#ffffff;
  --shadow: 0 6px 18px rgba(31,41,55,0.08);
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}

*{box-sizing:border-box}

html,body{
  height:100%;
  margin:0;
  background:linear-gradient(180deg,var(--bg),#ffffff);
  display:flex;
  align-items:center;
  justify-content:center;
  padding:2rem;
  color:var(--accent);
}

.calculator{
  width:340px;
  max-width:95vw;
  background:var(--panel);
  border-radius:12px;
  box-shadow:var(--shadow);
  padding:16px;
}

.display{
  height:64px;
  border-radius:8px;
  background:#0f172a;
  color:#e6eef8;
  display:flex;
  align-items:center;
  justify-content:flex-end;
  font-size:1.6rem;
  padding:0.5rem 1rem;
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis;
  margin-bottom:12px;
}

.buttons{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:10px;
}

.btn{
  background:var(--btn);
  border:0;
  padding:16px;
  border-radius:8px;
  font-size:1.1rem;
  cursor:pointer;
  box-shadow:0 2px 6px rgba(15,23,42,0.06);
  transition:transform .06s ease, box-shadow .06s;
  user-select:none;
}

.btn:active{ transform: translateY(1px); }

.btn.op{
  background:var(--op);
  color:var(--op-contrast);
}

.btn.function{
  background:#eef2ff;
  color:var(--secondary);
}

.btn.equals{
  background:#111827;
  color:white;
  grid-column:4;
  grid-row:5 / span 1;
}

.btn.zero{
  grid-column:1 / span 2;
}

@media (max-width:420px){
  .display{height:56px;font-size:1.25rem}
  .btn{padding:12px;font-size:1rem}
}

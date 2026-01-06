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

  <script src="script.js"></script>
</body>
</html>

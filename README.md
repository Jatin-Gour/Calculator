<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Enhanced Calculator</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;700&display=swap');

  body {
    margin: 0;
    font-family: 'Nunito', sans-serif;
    background: linear-gradient(135deg,#f5f5f7, #d8d8db);
    color: #222;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 100vh;
    padding: 40px 20px;
  }
  body.alt-bg {
    background: linear-gradient(135deg, #e1e2e6, #fdfdfd);
  }

  .calculator-container {
    background:#363434;
    border-radius: 20px;
    width: 400px;
    box-shadow: 0 15px 40px rgba(0,0,0,0.15);
    padding: 30px 25px 50px;
    display: flex;
    flex-direction: column;
  }



  .display {
    background: #e2e3e7;
    border-radius: 15px;
    padding: 20px 15px;
    font-size: 2.75rem;
    text-align: right;
    overflow-x: auto;
    box-shadow: inset 0 5px 10px rgba(0,0,0,0.1);
    user-select: none;
    min-height: 60px;
    color: #222;
    font-weight: 600;
    font-family: monospace;
  }
  .history {
    margin-top: 15px;
    max-height: 115px;
    overflow-y: auto;
    border-radius: 12px;
    background: #f9f9fb;
    box-shadow: inset 0 3px 5px rgba(0,0,0,0.05);
    padding: 10px 15px;
    font-size: 0.9rem;
    line-height: 1.4;
    font-family: monospace;
    color: #555;
  }
  .history-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #888;
    margin-bottom: 8px;
  }
  .btn-clear-history {
    cursor: pointer;
    background: transparent;
    border: none;
    color: #d9534f;
    font-weight: 700;
    font-size: 0.85rem;
    padding: 2px 6px;
    border-radius: 10px;
    transition: background 0.2s;
  }
  .btn-clear-history:hover {
    background: #d9534f;
    color: white;
  }

  .buttons {
    margin-top: 30px;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 54px);
    gap: 12px;
  }

  button {
    font-size: 1.3rem;
    border-radius: 16px;
    border: none;
    background: #dedfe3;
    color: #333;
    box-shadow: 0 4px #b8bac0;
    cursor: pointer;
    transition: background 0.3s, transform 0.1s;
    user-select: none;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  button:active {
    transform: translateY(2px);
    box-shadow: 0 2px #b8bac0;
  }

  button.operator {
    background: #ffa267;
    box-shadow: 0 4px #d2712c;
    color: #222;
    font-weight: 700;
  }
  button.operator:active {
    background: #e67629;
    box-shadow: 0 2px #d2712c;
  }

  button.clear {
    background: #f56353;
    box-shadow: 0 4px #c01f1f;
    color: white;
    font-weight: 700;
  }
  button.clear:active {
    background: #be322d;
    box-shadow: 0 2px #c01f1f;
  }

  button.zero {
    grid-column: span 2;
  }

  button.equals {
    background: #f7a600;
    box-shadow: 0 4px #b36b00;
    color: #fff;
    font-weight: 700;
    grid-column: span 2;
  }
  button.equals:active {
    background: #bd8400;
    box-shadow: 0 2px #b36b00;
  }

  button.extended {
    background: #9db4c0;
    box-shadow: 0 4px #7b8e99;
    font-weight: 700;
    color: #222;
  }
  button.extended:active {
    background: #7b8e99;
    box-shadow: 0 2px #7b8e99;
  }

  /* Scrollbar for history */
  .history::-webkit-scrollbar {
    width: 6px;
  }
  .history::-webkit-scrollbar-thumb {
    background-color: #bbb;
    border-radius: 3px;
  }
</style>
</head>
<body>
   <div class="calculator-container" role="application" aria-label="Enhanced calculator with advanced functions">
    <div id="display" class="display" aria-live="polite" aria-atomic="true" aria-label="Calculator display">0</div>
    <div class="history" id="history" aria-label="Calculation History" tabindex="0">
      <div class="history-header">
        <span>History</span>
        <button id="clear-history" class="btn-clear-history" aria-label="Clear History">Clear</button>
      </div>
      <ul id="history-list"></ul>
    </div>
    <div class="buttons" role="group" aria-label="Calculator Buttons">
      <!-- Row 1 -->
      <button class="clear" id="clear" aria-label="Clear">C</button>
      <button class="extended" id="reciprocal" aria-label="Reciprocal">1/x</button>
      <button class="extended" id="square" aria-label="Square">x²</button>
      <button class="extended" id="sqrt" aria-label="Square Root">√x</button>
      <button class="extended" id="plus-minus" aria-label="Plus or Minus">±</button>
      <!-- Row 2 -->
      <button data-digit="7" aria-label="7">7</button>
      <button data-digit="8" aria-label="8">8</button>
      <button data-digit="9" aria-label="9">9</button>
      <button class="extended" id="factorial" aria-label="Factorial">x!</button>
      <button class="extended" id="percent" aria-label="Percent">%</button>
      <!-- Row 3 -->
      <button data-digit="4" aria-label="4">4</button>
      <button data-digit="5" aria-label="5">5</button>
      <button data-digit="6" aria-label="6">6</button>
      <button class="operator" data-operator="*" aria-label="Multiply">×</button>
      <button class="operator" data-operator="/" aria-label="Divide">÷</button>
      <!-- Row 4 -->
      <button data-digit="1" aria-label="1">1</button>
      <button data-digit="2" aria-label="2">2</button>
      <button data-digit="3" aria-label="3">3</button>
      <button class="operator" data-operator="+" aria-label="Add">+</button>
      <button class="operator" data-operator="-" aria-label="Subtract">−</button>
      <!-- Row 5 -->
      <button data-digit="0" class="zero" aria-label="0">0</button>
      <button data-digit="." class="dot" aria-label="Decimal point">.</button>
      <button id="equals" class="equals" aria-label="Equals">=</button>
      <div></div>
      <div></div>
    </div>
  </div>

<script>
  (function() {
    const display = document.getElementById('display');
    const buttons = document.querySelectorAll('button');
    const historyList = document.getElementById('history-list');
    const clearHistoryBtn = document.getElementById('clear-history');

    let currentInput = '0';
    let operator = null;
    let firstOperand = null;
    let waitingForSecondOperand = false;
    let history = [];

    function updateDisplay() {
      display.textContent = currentInput;
    }

    function addToHistory(entry) {
      history.push(entry);
      updateHistoryUI();
    }

    function updateHistoryUI() {
      historyList.innerHTML = '';
      for(let i = history.length - 1; i >= 0; i--) {
        const li = document.createElement('li');
        li.textContent = history[i];
        li.tabIndex = 0;
        historyList.appendChild(li);
      }
    }

    function clearHistory() {
      history = [];
      updateHistoryUI();
    }

    function inputDigit(digit) {
      if (waitingForSecondOperand) {
        currentInput = digit;
        waitingForSecondOperand = false;
      } else {
        if (currentInput === '0' && digit !== '.') {
          currentInput = digit;
        } else {
          currentInput += digit;
        }
      }
    }

    function inputDecimal(dot) {
      if (waitingForSecondOperand) {
        currentInput = '0.';
        waitingForSecondOperand = false;
        return;
      }
      if (!currentInput.includes(dot)) {
        currentInput += dot;
      }
    }

    function handleOperator(nextOperator) {
      const inputValue = parseFloat(currentInput);

      if (operator && waitingForSecondOperand) {
        operator = nextOperator;
        return;
      }

      if (firstOperand == null && !isNaN(inputValue)) {
        firstOperand = inputValue;
      } else if (operator) {
        const result = calculate(firstOperand, inputValue, operator);
        if (result === "Error") {
          currentInput = "Error";
          firstOperand = null;
          operator = null;
          waitingForSecondOperand = false;
          updateDisplay();
          return;
        }
        currentInput = String(result);
        addToHistory(`${firstOperand} ${operator} ${inputValue} = ${result}`);
        firstOperand = result;
      }
      operator = nextOperator;
      waitingForSecondOperand = true;
    }

    function calculate(first, second, operator) {
      if (operator === '+') {
        return first + second;
      } else if (operator === '-') {
        return first - second;
      } else if (operator === '*') {
        return first * second;
      } else if (operator === '/') {
        if (second === 0) return "Error";
        return first / second;
      }
      return second;
    }

    function performSquare() {
      let value = parseFloat(currentInput);
      let result = value * value;
      addToHistory(`sqr(${value}) = ${result}`);
      currentInput = String(result);
      resetOperationState();
      updateDisplay();
    }

    function performSquareRoot() {
      let value = parseFloat(currentInput);
      if (value < 0) {
        currentInput = "Error";
        resetOperationState();
        updateDisplay();
        return;
      }
      let result = Math.sqrt(value);
      addToHistory(`√(${value}) = ${result}`);
      currentInput = String(result);
      resetOperationState();
      updateDisplay();
    }

    function performPercent() {
      let value = parseFloat(currentInput);
      if (firstOperand != null && operator !== null) {
        let percentValue;
        if (operator === '+' || operator === '-') {
          percentValue = (firstOperand * value) / 100;
        } else {
          percentValue = value / 100;
        }
        addToHistory(`% of ${currentInput} = ${percentValue}`);
        currentInput = String(percentValue);
        updateDisplay();
        waitingForSecondOperand = false;
      } else {
        let percentValue = value / 100;
        addToHistory(`% ${value} = ${percentValue}`);
        currentInput = String(percentValue);
        updateDisplay();
        waitingForSecondOperand = false;
      }
    }

    function factorial(n) {
      if (n === 0 || n === 1) return 1;
      let f = 1;
      for (let i = 2; i <= n; i++) {
        f *= i;
      }
      return f;
    }

    function performFactorial() {
      let value = parseFloat(currentInput);
      if (!Number.isInteger(value) || value < 0) {
        currentInput = "Error";
        resetOperationState();
        updateDisplay();
        return;
      }
      if (value > 170) {
        currentInput = "Error";
        resetOperationState();
        updateDisplay();
        return;
      }
      let result = factorial(value);
      addToHistory(`${value}! = ${result}`);
      currentInput = String(result);
      resetOperationState();
      updateDisplay();
    }

    function performReciprocal() {
      let value = parseFloat(currentInput);
      if (value === 0) {
        currentInput = "Error";
        resetOperationState();
        updateDisplay();
        return;
      }
      let result = 1 / value;
      addToHistory(`1/(${value}) = ${result}`);
      currentInput = String(result);
      resetOperationState();
      updateDisplay();
    }

    function performPlusMinus() {
      if (currentInput === "0" || currentInput === "Error") {
        return;
      }
      if (currentInput.startsWith("-")) {
        currentInput = currentInput.substring(1);
      } else {
        currentInput = "-" + currentInput;
      }
      updateDisplay();
    }

    function resetOperationState() {
      firstOperand = null;
      operator = null;
      waitingForSecondOperand = false;
    }

    function resetCalculator() {
      currentInput = '0';
      resetOperationState();
    }

    buttons.forEach(button => {
      button.addEventListener('click', () => {
        if (button.hasAttribute('data-digit')) {
          if (button.getAttribute('data-digit') === '.') {
            inputDecimal('.');
          } else {
            inputDigit(button.getAttribute('data-digit'));
          }
          updateDisplay();
          return;
        }

        if (button.hasAttribute('data-operator')) {
          handleOperator(button.getAttribute('data-operator'));
          updateDisplay();
          return;
        }

        if (button.id === 'clear') {
          resetCalculator();
          updateDisplay();
          return;
        }

        if (button.id === 'equals') {
          if (operator && !waitingForSecondOperand) {
            const inputValue = parseFloat(currentInput);
            const result = calculate(firstOperand, inputValue, operator);
            if (result === "Error") {
              currentInput = "Error";
            } else {
              currentInput = String(result);
              addToHistory(`${firstOperand} ${operator} ${inputValue} = ${result}`);
            }
            resetOperationState();
            updateDisplay();
          }
          return;
        }

        if (button.id === 'square') {
          performSquare();
          return;
        }

        if (button.id === 'sqrt') {
          performSquareRoot();
          return;
        }

        if (button.id === 'percent') {
          performPercent();
          return;
        }

        if (button.id === 'factorial') {
          performFactorial();
          return;
        }

        if (button.id === 'reciprocal') {
          performReciprocal();
          return;
        }

        if (button.id === 'plus-minus') {
          performPlusMinus();
          return;
        }
      });
    });

    clearHistoryBtn.addEventListener('click', () => {
      clearHistory();
    });

    updateHistoryUI();
    updateDisplay();
  })();
</script>
</body>
</html>

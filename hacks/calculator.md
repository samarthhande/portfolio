---
title: JS Calculator
comments: true
hide: true
layout: base
description: A common way to become familiar with a language is to build a calculator.  This calculator shows off button with actions.
permalink: /calculator
---

<!-- 
Hack 0: Right justify result
Hack 1: Test conditions on small, big, and decimal numbers, report on findings. Fix issues.
Hack 2: Add the common math operation that is missing from calculator
Hack 3: Implement 1 number operation (ie SQRT) 
-->

<style>
  .calculator-output {
    grid-column: span 4;
    grid-row: span 1;
    border-radius: 10px;
    padding: 0.25em;
    font-size: 20px;
    border: 5px solid black;
    display: flex;
    align-items: center;
    justify-content: flex-end; /* Right justify result */
    background: #f2f2f2;
  }

  /* ===== Number Buttons with Fixed Colors ===== */
  .calculator-number {
    border-radius: 12px;
    padding: 0.5em;
    font-size: 18px;
    font-weight: bold;
    color: black;
    text-align: center;
    cursor: pointer;
    transition: transform 0.15s ease, box-shadow 0.15s ease;
  }

  .calculator-number:nth-of-type(2)  { background-color: #ff9999; } /* 1 */
  .calculator-number:nth-of-type(3)  { background-color: #ffcc66; } /* 2 */
  .calculator-number:nth-of-type(4)  { background-color: #ffff66; } /* 3 */
  .calculator-number:nth-of-type(6)  { background-color: #99ff99; } /* 4 */
  .calculator-number:nth-of-type(7)  { background-color: #66ffcc; } /* 5 */
  .calculator-number:nth-of-type(8)  { background-color: #66ccff; } /* 6 */
  .calculator-number:nth-of-type(10) { background-color: #cc99ff; } /* 7 */
  .calculator-number:nth-of-type(11) { background-color: #ff66cc; } /* 8 */
  .calculator-number:nth-of-type(12) { background-color: #ffb366; } /* 9 */
  .calculator-number:nth-of-type(14) { background-color: #66ffff; } /* 0 */
  .calculator-number:nth-of-type(15) { background-color: #ffeb99; } /* . */
  .calculator-number:nth-of-type(19) { background-color: #ffeb99; } /* . */

  /* ===== Operators, Clear, Equals ===== */
  .calculator-operation {
    background-color: #ff6666; /* Red */
    color: black;
    border-radius: 12px;
    padding: 0.5em;
    font-size: 18px;
    font-weight: bold;
    text-align: center;
    cursor: pointer;
  }

  .calculator-operation:nth-of-type(5)  { background-color: #ff6666; } /* + */
  .calculator-operation:nth-of-type(9)  { background-color: #66a3ff; } /* - */
  .calculator-operation:nth-of-type(13) { background-color: #66cc66; } /* * */
  .calculator-operation:nth-of-type(17) { background-color: #b266ff; } /* ÷ */
  .calculator-operation.sqrt { background-color: #ffaa33; } /* √ */

  .calculator-clear {
    background-color: #ff3333; /* Bright Red */
    color: white;
    border-radius: 12px;
    font-weight: bold;
    cursor: pointer;
    text-align: center;
    padding: 0.5em;
  }

  .calculator-equals {
    background-color: #ffcc00; /* Gold */
    color: black;
    border-radius: 12px;
    font-weight: bold;
    cursor: pointer;
    text-align: center;
    padding: 0.5em;
  }

  /* Hover effect */
  .calculator-number:hover,
  .calculator-operation:hover,
  .calculator-clear:hover,
  .calculator-equals:hover {
    transform: scale(1.1);
    box-shadow: 0 0 8px rgba(0,0,0,0.3);
  }

  canvas {
    filter: none;
  }
</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
      <!--result-->
      <div class="calculator-output" id="output">0</div>
      <!--row 1-->
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-operation">+</div>
      <!--row 2-->
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      <div class="calculator-operation">-</div>
      <!--row 3-->
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      <div class="calculator-number">9</div>
      <div class="calculator-operation">*</div>
      <!--row 4-->
      <div class="calculator-clear">A/C</div>
      <div class="calculator-number">0</div>
      <div class="calculator-number">.</div>
      <div class="calculator-equals">=</div>
      <!--row 5-->
      <div class="calculator-operation">÷</div>
      <div class="calculator-operation sqrt">√</div>
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
// initialize important variables to manage calculations
var firstNumber = null;
var operator = null;
var nextReady = true;

// build objects containing key elements
const output = document.getElementById("output");
const numbers = document.querySelectorAll(".calculator-number");
const operations = document.querySelectorAll(".calculator-operation");
const clear = document.querySelectorAll(".calculator-clear");
const equals = document.querySelectorAll(".calculator-equals");

// Number buttons listener
numbers.forEach(button => {
  button.addEventListener("click", function() {
    number(button.textContent);
  });
});

// Number action
function number (value) {
    if (value != ".") {
        if (nextReady == true) { 
            output.innerHTML = value;
            if (value != "0") { 
                nextReady = false;
            }
        } else {
            output.innerHTML = output.innerHTML + value; 
        }
    } else { 
        if (output.innerHTML.indexOf(".") == -1) {
            output.innerHTML = output.innerHTML + value;
            nextReady = false;
        }
    }
}

// Operation buttons listener
operations.forEach(button => {
  button.addEventListener("click", function() {
    let op = button.textContent;
    if (op === "÷") { op = "/"; }
    if (op === "×") { op = "*"; }
    if (op === "√") { sqrtOperation(); return; }
    operation(op);
  });
});

// Operator action
function operation (choice) {
    if (firstNumber == null) { 
        firstNumber = parseFloat(output.innerHTML);
        nextReady = true;
        operator = choice;
        return;
    }
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML)); 
    operator = choice;
    output.innerHTML = firstNumber.toString();
    nextReady = true;
}

// Calculator
function calculate (first, second) {
    let result = 0;
    switch (operator) {
        case "+":
            result = first + second;
            break;
        case "-":
            result = first - second;
            break;
        case "*":
            result = first * second;
            break;
        case "/":
            if (second === 0) {
              return "Error";
            }
            result = first / second;
            break;
        default: 
            break;
    }
    return result;
}

// Square root operation
function sqrtOperation() {
    let current = parseFloat(output.innerHTML);
    if (current < 0) {
      output.innerHTML = "Error";
      nextReady = true;
      return;
    }
    let result = Math.sqrt(current);
    output.innerHTML = result.toString();
    nextReady = true;
    firstNumber = null;
    operator = null;
}

// Equals button listener
equals.forEach(button => {
  button.addEventListener("click", function() {
    equal();
  });
});

// Equal action
function equal () {
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
    output.innerHTML = firstNumber.toString();
    nextReady = true;
}

// Clear button listener
clear.forEach(button => {
  button.addEventListener("click", function() {
    clearCalc();
  });
});

// A/C action
function clearCalc () {
    firstNumber = null;
    output.innerHTML = "0";
    nextReady = true;
}

// ============================
// Keyboard Support
// ============================
document.addEventListener("keydown", function(event) {
  const key = event.key;

  // Handle numbers and decimal
  if (!isNaN(key) || key === ".") {
    number(key);
  }

  // Handle operations with more symbol support
  if (key === "+" || key === "-" || key === "*" || key === "/" || 
      key === "x" || key === "X" || key === ":" || key === "÷" || key === "×") {
    let mappedOp = key;
    if (key === "x" || key === "X" || key === "×") mappedOp = "*";
    if (key === ":" || key === "÷") mappedOp = "/";
    operation(mappedOp);
  }

  // Handle square root with 'r' key
  if (key === "r" || key === "R") {
    sqrtOperation();
  }

  // Handle equals (= or Enter)
  if (key === "=" || key === "Enter") {
    equal();
  }

  // Handle clear (Escape)
  if (key === "Escape") {
    clearCalc();
  }

  // Handle backspace (delete last digit)
  if (key === "Backspace") {
    if (output.innerHTML.length > 1) {
      output.innerHTML = output.innerHTML.slice(0, -1);
    } else {
      output.innerHTML = "0";
      nextReady = true;
    }
  }
});
</script>

<!-- Vanta animations just for fun -->
<script src="{{site.baseurl}}/assets/js/three.r119.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.halo.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.birds.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.net.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.rings.min.js"></script>

<script>
var vantaInstances = {
  halo: VANTA.HALO,
  birds: VANTA.BIRDS,
  net: VANTA.NET,
  rings: VANTA.RINGS
};
var vantaInstance = vantaInstances[Object.keys(vantaInstances)[Math.floor(Math.random() * Object.keys(vantaInstances).length)]];
vantaInstance({
  el: "#animation",
  mouseControls: true,
  touchControls: true,
  gyroControls: false
});
</script>

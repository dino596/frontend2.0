---
layout: default
search_exclude: true
title: Calculator Base
permalink: 
---

<link rel="stylesheet" href="CalculatorStyle.css">

<div class="mouse-follower"></div>
<div class="calculator-container">
    <div class="calculator-outputs">
      <div class="calculator-output" id="binary">0</div>
      <div class="calculator-output" id="octal">0</div>
      <div class="calculator-output" id="output">0</div>
      <div class="calculator-output" id="hexadecimal">0</div>
    </div>
    <div class="calculator-switch">Logic Gates</div>
    <div class="calculator-gates">
      <div class="calculator-gate" id="AND">AND</div>
      <div class="calculator-gate" id="OR">OR</div>
      <div class="calculator-gate" id="NOT">NOT</div>
      <div class="calculator-gate" id="NAND">NAND</div>
      <div class="calculator-gate" id="XOR">XOR</div>
      <div class="calculator-gate" id="XNOR">XNOR</div>
    </div>
    <div class="calculator-bit-shifts">
      <div class="calculator-bit-shift" id="left"><<</div>
      <div class="calculator-bit-shift" id="right">>></div>
    </div>
    <div class="calculator-clear">C</div>
    <div class="calculator-operations">
      <div class="calculator-operation" id="division">÷</div>
      <div class="calculator-operation" id="multiplication">×</div>
      <div class="calculator-operation" id="subtraction">-</div>
      <div class="calculator-operation" id="addition">+</div>
    </div>
    <div class="calculator-numbers">
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      <div class="calculator-number">9</div>
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-number">0</div>
    </div>
    <div class="calculator-backspace">⌫</div>
    <div class="calculator-equals">=</div>
</div>

<script src="scripts/logic_gates.js"></script>

<script>
  var firstNumber = null;
  var operator = null;
  var nextReady = true;

  const output = document.getElementById("output");
  const binaryOutput = document.getElementById("binary");

  const numbers = document.querySelectorAll(".calculator-number");
  const operations = document.querySelectorAll(".calculator-operation");
  const clear = document.querySelectorAll(".calculator-clear");
  const equals = document.querySelector(".calculator-equals");
  const backspace = document.querySelector(".calculator-backspace");

  const toggleSwitch = document.querySelector(".calculator-switch");
  const gates = document.querySelectorAll(".calculator-gate");

  const bitSwitch = document.querySelectorAll(".calculator-bit-shift");
  
  var ANDgate = document.getElementById("AND");
  var ORgate = document.getElementById("OR");
  var NOTgate = document.getElementById("NOT");
  var NANDgate = document.getElementById("NAND");
  var XORgate = document.getElementById("XOR");
  var XNORgate = document.getElementById("XNOR");

  numbers.forEach(button => {
    button.addEventListener("click", function() {
      if (nextReady == true) {
          output.innerHTML = button.textContent;
          binaryOutput.innerHTML = parseInt(output.innerHTML).toString(2);
          if (button.textContent != "0") {
              nextReady = false;
          }
      } else {
          output.innerHTML = output.innerHTML + button.textContent;
          binaryOutput.innerHTML = output.innerHTML.toString(2);
      }
    });
  });

  operations.forEach(button => {
    button.addEventListener("click", function() {
      firstNumber = parseInt(output.innerHTML);
      nextReady = true;
      operator = button.textContent;
    });
  });
  
  function calculate (first, second) {
    let result = 0;
    switch (operator) {
      case "+":
        result = first + second;
        break;
      case "-":
        result = (result = first - second) < 0 ? 0 : result;
        break;
      case "×":
        result = first * second;
        break;
      case "÷":
        result = first / second;
        break;
      case "AND":
        result = first & second;
        break;
      case "OR":
        result = first | second;
        break;
      case "NAND":
        result = notGate(first & second);
        break;
      case "NOR":
        result = notGate(first | second);
        break;
      case "XOR":
        result = first ^ second;
        break;
      case "XNOR":
        result = notGate(first ^ second);
        break;
      case ">>":
        result = first >> second;
        break;
      case "<<":
        result = first << second;
        break;
      default: 
        break;
    }
    return Math.floor(result);
  }
  toggleSwitch.addEventListener("click", function() {
    if(toggleSwitch.textContent == "Hexadecimal") {
      toggleSwitch.textContent = "Logic Gates";
      ANDgate.textContent = "AND";
      ORgate.textContent = "OR";
      NOTgate.textContent = "NOT";
      NANDgate.textContent = "NAND";
      XORgate.textContent = "XOR";
      XNORgate.textContent = "XNOR";
    } else {
      toggleSwitch.textContent = "Hexadecimal";
      ANDgate.textContent = "A";
      ORgate.textContent = "B";
      NOTgate.textContent = "C";
      NANDgate.textContent = "D";
      XORgate.textContent = "E";
      XNORgate.textContent = "F";
    }
  });

  gates.forEach(button => {
    button.addEventListener("click", function() {
      if (button.textContent == "NOT") {
        output.innerHTML = notGate(parseInt(output.innerHTML))
        nextReady = true;
        operator = null;
        firstNumber = null;
        return;
      }
      firstNumber = parseInt(output.innerHTML);
      nextReady = true;
      operator = button.textContent;
    });
  });

  bitSwitch.forEach(button => {
    button.addEventListener("click", function() {
      firstNumber = parseInt(output.innerHTML);
      nextReady = true;
      operator = button.textContent;     
    });
  });

  function notGate (num) {
    num = num.toString(2);
    var result = "";
    for (let i = 0; i < num.length; i++) {
      if (num[i] == "0") {
        result += "1";
      } else {
        result += "0";
      }
    }
    return parseInt(result, 2);
  }

  operations.forEach(button => {
    button.addEventListener("click", function() {
      firstNumber = parseInt(output.innerHTML);
      nextReady = true;
      operator = button.textContent;
    });
  });

  equals.addEventListener("click", function() {
    if (firstNumber){
      firstNumber = calculate(firstNumber, parseInt(output.innerHTML));
      output.innerHTML = firstNumber.toString();
      binaryOutput.innerHTML = output.innerHTML.toString(2);
      nextReady = true;
      operator = null;
      firstNumber = null;
      return;
    }
  });

  clear.forEach(button => {
    button.addEventListener("click", function() {
      firstNumber = null;
      output.innerHTML = "0";
      nextReady = true;
    });
  });

  backspace.addEventListener("click", function() {
    if (output.innerHTML.length > 1) {
      output.innerHTML = output.innerHTML.slice(0, -1);
    } else {
      output.innerHTML = "0";
      nextReady = true;
    }
  });
</script>

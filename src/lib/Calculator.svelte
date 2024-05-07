<script lang="ts">
  import Display from './Display.svelte';
  import Keypad from './Keypad.svelte';

  let prevVal: number | null = null;
  let currentVal: number = 0;
  let lastRhs: number | null = null;
  let lastOp: string | null = null;
  let justFlipped = false;
  let decimalMode = false;
  let currentOp = '';

  function isOperator(op: string) {
    return op == '+' || op == '-' || op == '÷' || op == '×';
  }

  function opFunc(op: string, lhs: number, rhs: number) {
    switch (op) {
      case '+':
        return lhs + rhs;
      case '-':
        return lhs - rhs;
      case '÷':
        return lhs / rhs;
      case '×':
        return lhs * rhs;
      default:
        return 0;
    }
  }

  function keyClick(key: string) {
    if (key == '.') {
      if (decimalMode) {
        decimalMode = false;
      } else if (justFlipped) {
        currentVal = 0;
        decimalMode = true;
        justFlipped = false;
      } else if (Number.isInteger(currentVal)) {
        decimalMode = true;
      }
    } else if (key >= '0' && key <= '9') {
      const keyNum = Number(key);

      if (justFlipped) {
        currentVal = keyNum;
        justFlipped = false;
      } else if (decimalMode) {
        currentVal = Number(currentVal + '.' + keyNum);
        decimalMode = false;
      } else {
        currentVal = Number(currentVal + '' + keyNum);
      }
    } else if (key == '±') {
      currentVal = currentVal * -1;
    } else if (key == '%') {
      currentVal = currentVal / 100.0;
    } else if (key == 'AC') {
      // Reset all state
      prevVal = null;
      currentVal = 0;
      lastRhs = null;
      lastOp = null;
      justFlipped = false;
      decimalMode = false;
      currentOp = '';
    } else if (key == '=') {
      if (lastRhs != null && lastOp != null) {
        // Redo last calculation
        currentVal = opFunc(lastOp, currentVal, lastRhs);
      } else if (prevVal != null) {
        // Normal calculation
        lastRhs = currentVal;
        lastOp = currentOp;
        currentVal = opFunc(currentOp, prevVal, currentVal);
      }
      decimalMode = false;
      justFlipped = true;
      prevVal = null;
      currentOp = '';
    } else if (isOperator(key)) {
      if (currentOp != '' && prevVal != null && !justFlipped) {
        // Apply last selected operation
        currentVal = opFunc(currentOp, prevVal, currentVal);
        decimalMode = false;
      }
      currentOp = key;
      prevVal = currentVal;
      lastRhs = null;
      lastOp = null;
      justFlipped = true;
    }
  }

  $: displayValue = decimalMode ? currentVal + '.' : currentVal + '';
</script>

<div
  class="
    grid grid-rows-5 grid-cols-1 gap-[1px]
    w-[calc(min(340px,100vw))] h-[calc(min(450px,100vh))]
    bg-zinc-800
    border rounded-md border-zinc-500 overflow-hidden
    touch-manipulation
  "
>
  <Display value={displayValue} />
  <Keypad {currentOp} on:click={(e) => keyClick(e.detail)} />
</div>

<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import Key from './Key.svelte';

  export let currentOp: string | null;

  const dispatch = createEventDispatcher();

  function opStyle(currentOp: string | null, op: string) {
    if (op == currentOp) {
      return 'current';
    } else {
      return 'operator';
    }
  }

  function onKeydown(e: KeyboardEvent) {
    if ((e.key >= '0' && e.key <= '9') || e.key == '+' || e.key == '-' || e.key == '%') {
      dispatch('click', e.key);
    } else if (e.key == 'Enter') {
      dispatch('click', '=');
    } else if (e.key == '*') {
      dispatch('click', '×');
    } else if (e.key == '/') {
      dispatch('click', '÷');
    } else if (e.key == 'Escape') {
      dispatch('click', 'AC');
    }
  }
</script>

<svelte:window on:keydown|preventDefault={onKeydown} />

<div class="grid grid-rows-5 grid-cols-4 gap-[1px]">
  <Key key="AC" style="function" on:click />
  <Key key="±" style="function" on:click />
  <Key key="%" style="function" on:click />
  <Key key="÷" style={opStyle(currentOp, '÷')} on:click />
  <Key key="7" style="number" on:click />
  <Key key="8" style="number" on:click />
  <Key key="9" style="number" on:click />
  <Key key="×" style={opStyle(currentOp, '×')} on:click />
  <Key key="4" style="number" on:click />
  <Key key="5" style="number" on:click />
  <Key key="6" style="number" on:click />
  <Key key="+" style={opStyle(currentOp, '+')} on:click />
  <Key key="1" style="number" on:click />
  <Key key="2" style="number" on:click />
  <Key key="3" style="number" on:click />
  <Key key="-" style={opStyle(currentOp, '-')} on:click />
  <Key key="0" style="number" spanDouble={true} on:click />
  <Key key="." style="number" on:click />
  <Key key="=" style="equals" on:click />
</div>

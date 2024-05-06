<script lang="ts">
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let key: string;
  export let style: string;
  export let spanDouble: boolean = false;
</script>

<button
  class="
    grid place-items-center
    text-white
    select-none
    {spanDouble ? 'col-span-2' : ''}
    {style == 'function' ? 'bg-slate-700' : ''}
    {style == 'number' ? 'bg-slate-600' : ''}
    {style == 'operator' ? 'bg-orange-500' : ''}
    {style == 'current' ? 'bg-orange-600' : ''}
    {style == 'equals' ? 'bg-blue-400 text-black' : ''}
  "
  on:click={() => dispatch('click', key)}
>
  {key}
</button>

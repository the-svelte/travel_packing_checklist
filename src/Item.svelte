<script>
  import IconXSquare from './icons/IconXSquare.svelte';
  import {blurOnKey} from './util';

  // { id: string, name: string, packed: boolean }
  export let item;

  let editing = false;
</script>

<li class="list-group-item">
  <div class="d-flex align-items-center justify-content-between">
    <div class="">
      <input
        class="form-check-input"
        type="checkbox"
        id=""
        bind:checked={item.packed}
      >
    </div>
    <div class="flex-grow-1 px-4">
      {#if editing}
        <!-- A11y: Avoid using autofocus -->
        <input
          autofocus
          type="text"
          class="form-control form-control-sm"
          bind:value={item.name}
          on:blur={() => editing = false}
          on:keydown={blurOnKey}
        />
      {:else}
        <label
          class="{item.packed ? 'text-decoration-line-through' : ''}"
          for=""
          on:click={() => editing = true}
        >{item.name}</label>
      {/if}
    </div>
    <div class="">
      <button
        type="button"
        class="btn btn-sm text-muted"
      ><IconXSquare/></button>
    </div>
  </div>
</li>

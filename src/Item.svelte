<script>
  import {blurOnKey} from './util';

  export let item;

  let editing = false;

  
</script>

<li class="list-group-item">
  <div class="d-flex justify-content-between">
    <div class="form-check">
      <input
        class="form-check-input"
        type="checkbox"
        value=""
        id="flexCheckDefault"
        bind:checked={item.packed}
      >
      {#if editing}
        <input
          class="form-input"
          bind:value={item.name}
          on:blur={() => editing = false}
          on:keydown={blurOnKey}
        />
      {:else}
        <label
          class:decoration={item.packed}
          class="form-check-label"
          for="flexCheckDefault"
          on:click={() => editing = true}
        >{item.name}</label>
      {/if}
    </div>
    <div class="">
      <button
        type="button"
        class="btn btn-primary btn-sm"
      >Confirm identity</button>
    </div>
  </div>
</li>

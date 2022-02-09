<script>
  import {blurOnKey} from './util';

  export let item;

  let editing = false;
</script>

<li class="list-group-item">
  <div class="d-flex align-items-center justify-content-between ">
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
          class="form-check-label {item.packed && 'text-decoration-line-through'}"
          for="flexCheckDefault"
          on:click={() => editing = true}
        >{item.name}</label>
      {/if}
    </div>
    <div class="">
      <button
        type="button"
        class="btn btn-primary btn-sm"
      >Delete</button>
    </div>
  </div>
</li>

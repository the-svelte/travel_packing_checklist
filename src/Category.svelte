<script>
  import Item from './Item.svelte';
  import {getGuid, blurOnKey, sortOnName} from './util';

  // export let categories;
  // { id: string, name: string, items: { [id string]: Item }}
  export let category;
  export let show;

  let editing = false;
  let itemName = '';
  let items = [];
  let message = '';

  $: items = Object.values(category.items);
  $: remaining = items.filter(i => i.packed).length;
  $: total = items.length;
  $: status = `${remaining} of ${total} remaining`;
  $: itemsToShow = sortOnName(items.filter(i => shouldShow(show, i)));

  function addItem() {
    const {items} = category;
    const id = getGuid();
    items[id] = { id, name: itemName, packed: false };
    category.items = items;
    itemName = '';
  }

  function shouldShow(show, item) {
    return (
      show === 'all'
      || (show === 'packed' && item.packed)
      || (show === 'unpacked' && !item.packed)
    );
  }
</script>

<section>
  <div class="d-flex justify-content-between align-items-center h4">
    {#if editing}
      <form>
        <input
          type="text"
          class="form-control"
          autofocus
          bind:value={category.name}
          on:blur={() => editing = false}
          on:keypress={blurOnKey}
        />
      </form>
    {:else}
      <span
        on:click={() => editing = true}
      >{category.name}</span>
    {/if}
    <small class="text-muted">{status}</small>
    <button class="btn btn-danger">Del</button>
  </div>

  <form
    class="my-3"
    on:submit|preventDefault={addItem}
  >
    <div class="input-group mb-3">
      <div class="form-floating flex-grow-1">
        <input
          type="text"
          class="form-control"
          id="floatingItem"
          placeholder="Niew Item"
          bind:value={itemName}>
        <label for="floatingItem">New Item</label>
      </div>
      <button class="btn btn-outline-secondary" type="button" id="button-addon2">Enter</button>
    </div>


    <!-- <div class="col"> -->
      <!-- <div class="row">
        <label
        for="exampleInputEmail1"
        class="col-sm-3 col-form-label"
        >New Item</label>
        <div class="col-sm-9">
          <input
          type="text"
          class="form-control"
          id="exampleInputEmail1"
          bind:value={itemName}
          />
        </div>
      </div> -->
    <!-- </div> -->
    <!-- <div class="col">
      <button
        type="submit"
        class="btn btn-primary"
        disabled={!itemName}
      >Add Item</button>
    </div> -->
  </form>

  <ul class="list-group">
    {#each itemsToShow as item (item.id)}
      <Item bind:item />
    {:else}
      <div>This category does not contain any items yet.</div>
    {/each}
  </ul>
</section>

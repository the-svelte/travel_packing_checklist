<script>
  import Item from './Item.svelte';
  import {getGuid, blurOnKey, sortOnName} from './util';

  // export let categories;
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
      show === 'all' ||
      (show === 'packed' && item.packed) ||
      (show === 'unpacked' && !item.packed)
    );
  }
</script>

<section>
  <div class="d-flex justify-content-between align-items-center h4">
    {#if editing}
      <input
        bind:value={category.name}
        on:blur={() => editing = false}
        on:keypress={blurOnKey}
      />
    {:else}
      <span
        on:click={() => editing = true}
      >{category.name}</span>
    {/if}
    <small class="text-muted">{status}</small>
    <button class="btn btn-danger">Del</button>
  </div>

  <form
    class="row row-cols-lg-auto g-3 align-items-center"
    on:submit|preventDefault={addItem}
  >
    <div class="col-auto">
      <label
        for="exampleInputEmail1"
        class="visually-hidden form-label-"
      >New Item</label>
      <input
        type="text"
        class="form-control"
        id="exampleInputEmail1"
        bind:value={itemName}
      />
    </div>
    <div class="col-auto">
      <button
        type="submit"
        class="btn btn-primary"
        disabled={!itemName}
      >Add Item</button>
    </div>
  </form>

  <ul>
    {#each itemsToShow as item (item.id)}
      <Item bind:item />
    {:else}
      <div>This category does not contain any items yet.</div>
    {/each}
  </ul>
</section>

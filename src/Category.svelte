<script>
  import Item from './Item.svelte';
  import {getGuid, blurOnKey, sortOnName} from './util';

  export let categories;
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
    const duplicate = Object.values(categories).some(
      cat => Object.values(cat.items).some(item => item.name === itemName)
    );
    if (duplicate) {
      message = `The Item "${itemName}" aleady exists.`;
      alert(message);
      return;
    }

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

<div class="card mb-4">
  <div class="card-header">
    <div class="d-flex justify-content-between align-items-center h6 mb-0">
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
      <button class="btn btn-outline-danger btn-sm">Del</button>
    </div>
  </div>
  <div class="card-body">
    <form on:submit|preventDefault={addItem}>
      <div class="input-group">
        <div class="form-floating flex-grow-1">
          <input
            type="text"
            class="form-control"
            id="floatingItem"
            placeholder="Niew Item"
            bind:value={itemName}>
          <label for="floatingItem">New Item</label>
        </div>
        <button
          class="btn btn-outline-secondary"
          type="submit"
          id="button-addon2"
          disabled={!itemName}
        >Enter</button>
      </div>
    </form>
  </div>
  <ul class="list-group list-group-flush">
    {#each itemsToShow as item (item.id)}
      <Item bind:item />
    {:else}
      <div>This category does not contain any items yet.</div>
    {/each}
  </ul>
</div>

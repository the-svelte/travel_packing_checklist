<script>
  import Category from './Category.svelte';
  import {getGuid, sortOnName} from './util';

  export let categories;

  let categoryArray = [];
  let categoryName = '';
  let message = '';
  let show = 'all';

  $: categoryArray = sortOnName(Object.values(categories));

  function addCategory() {
    const id = getGuid();
    categories[id] = { id, name: categoryName, items: {}};
    categories = categories;
    categoryName = '';
  }

  function clearAllChecks() {
    for (const cateogory of Object.values(categories)) {
      for (const item of Object.values(cateogry.item)) {
        item.packed = false;
      }
    }
    categories = categories;
  }
</script>

<section>
  <header>
    <form class="my-3" on:submit|preventDefault={addCategory}>
      <div class="input-group mb-3">
        <div class="form-floating flex-grow-1">
          <input
            type="text"
            class="form-control"
            id="floatingItem"
            placeholder="Niew Item"
            bind:value={categoryName}>
          <label for="floatingItem">New Category</label>
        </div>
        <button
          class="btn btn-outline-secondary"
          type="submit"
          id="button-addon2"
          disabled={!categoryName}
        >Add</button>
      </div>
    </form>
    <p>Suggested categories include Backpack, Clothes,<br>Last minute, Medicines, Running Gear, and Toiletries.</p>
    <div>
      <div class="form-check form-check-inline">
        <input class="form-check-input" type="radio" name="show" id="inlineRadio1" bind:group={show} value="all">
        <label class="form-check-label" for="inlineRadio1">All</label>
      </div>
      <div class="form-check form-check-inline">
        <input class="form-check-input" type="radio" name="show" id="inlineRadio2" bind:group={show} value="packed">
        <label class="form-check-label" for="inlineRadio2">Packed</label>
      </div>
      <div class="form-check form-check-inline">
        <input class="form-check-input" type="radio" name="show" id="inlineRadio3" bind:group={show} value="unpacked">
        <label class="form-check-label" for="inlineRadio3">Unpacked</label>
      </div>
    </div>
  </header>

  <div>
    {#each categoryArray as category (category.id)}
      <Category {category} {show} />
    {/each}
  </div>
</section>


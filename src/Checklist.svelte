<script>
  import {createEventDispatcher} from 'svelte';
  import Category from './Category.svelte';
  import {getGuid, sortOnName} from './util';

  const dispatch = createEventDispatcher();

  let categories = {};
  let categoryArray = [];
  let categoryName = '';
  let message = '';
  let show = 'all';

  $: categoryArray = sortOnName(Object.values(categories));

  function addCategory() {
    const duplicate = Object.values(categories).some(
      cat => cat.name === categoryName
    );
    if (duplicate) {
      message = `The Category "${categoryName}" already exists.`;
      alert(message);
      return;
    }

    const id = getGuid();
    categories[id] = { id, name: categoryName, items: {}};
    categories = categories;
    categoryName = '';
  }

  function clearAllChecks() {
    for (const category of Object.values(categories)) {
      for (const item of Object.values(category.items)) {
        item.packed = false;
      }
    }
    categories = categories;
  }

  function deleteCategory(category) {
    delete categories[category.id];
    categories = categories;
  }
</script>

<section>
  <div>
    <button class="btn btn-danger"
      on:click={() => dispatch('logout')}
    >Log Out</button>
  </div>
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
    <div class="d-flex justify-content-between align-items-center mb-3">
      <div class="d-flex flex-column flex-sm-row">
        <div class="form-check me-3">
          <input class="form-check-input" type="radio" name="show" id="" bind:group={show} value="all">
          <label class="form-check-label" for="">All</label>
        </div>
        <div class="form-check me-3">
          <input class="form-check-input" type="radio" name="show" id="" bind:group={show} value="packed">
          <label class="form-check-label" for="">Packed</label>
        </div>
        <div class="form-check me-3">
          <input class="form-check-input" type="radio" name="show" id="" bind:group={show} value="unpacked">
          <label class="form-check-label" for="">Unpacked</label>
        </div>
      </div>
      <button class="btn btn-secondary btn-sm" on:click={clearAllChecks}>Clear All Checks</button>
    </div>
  </header>

  <div>
    {#each categoryArray as category (category.id)}
      <Category {category} {categories} {show}
        on:delete={() => deleteCategory(category)}
      />
    {/each}
  </div>
</section>


<script>
import Login from './Login.svelte';
import Category from './Category.svelte';
</script>

<section class="container-fluid">
  <div class="row justify-content-center">
    <div class="py-3 bg-light display-6 text-center">Travel Packing Checklist</div>
    <!-- <div class="col-sm-6 col-md-4 col-xl-3 pt-4">
      <Login />
    </div> -->
  </div>
  <div class="row justify-content-center">
    <div class="col-sm-6 col-xl-3 pt-4">
      <Category category={{id: 'xxx', name: 'House work', items: {}}} show={'all'}/>
    </div>
  </div>
</section>

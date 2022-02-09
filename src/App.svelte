<script>
import Login from './Login.svelte';
import CheckList from './CheckList.svelte';

const categories = {
  'xxx': {
    id: 'xxx',
    name: 'House work',
    items: {
      aaa: { id: 'xxx-aaa', name: 'Dish' },
      bbb: { id: 'xxx-bbb', name: 'Wash' },
      ccc: { id: 'xxx-ccc', name: 'Clean' },
    },
  },
  'yyy': {
    id: 'yyy',
    name: 'Office work',
    items: {
      aaa: { id: 'yyy-aaa', name: 'React' },
      bbb: { id: 'yyy-bbb', name: 'Vue.js' },
      ccc: { id: 'yyy-ccc', name: 'Svelte' },
    },
  },
};
</script>

<section class="container-fluid">
  <div class="row justify-content-center">
    <div class="py-3 bg-light display-6 text-center">Travel Packing Checklist</div>
    <!-- <div class="col-sm-6 col-md-4 col-xl-3 pt-4">
      <Login />
    </div> -->
  </div>
  <div class="row justify-content-center">
    <div class="col-sm-5 col-xl-3 pt-4">
      <CheckList
        categories={categories}
      />
    </div>
  </div>
</section>

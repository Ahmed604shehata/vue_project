
<template>
  <div class="home">
    <Banner />
    <Product />
  </div>
</template>

<script>

import Banner from '../components/Banner'
import Product from '../components/Products'
export default {
  name: "Home",
  components: {
    Banner,
    Product,
  },
};
</script>

<style></style>

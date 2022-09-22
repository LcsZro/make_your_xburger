<template class="main-container">
  <Banner />
  <div>
    <h1>Monte o seu Burger:</h1>
    <p>formulário</p>
    <Burgerform />
  </div>
</template>

<script>
import Banner from "../components/Banner.vue";
import Burgerform from "../components/Burgerform.vue";

export default {
  name: "Home",
  components: {
    Banner,
    Burgerform,
  } 
}

</script>
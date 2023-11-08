<template>
  <div class="min-h-screen">
    <div>
      <navBar></navBar>
      <LayoutPage></LayoutPage>
      <foot></foot>
    </div>
  </div>
</template>
<script>
import LayoutPage from "./layout/LayoutPage.vue";
import navBar from "./components/navBar.vue";
import foot from "./components/foot.vue";

export default {
  components: {
    LayoutPage,
    navBar,
    foot,
  },
};
</script>

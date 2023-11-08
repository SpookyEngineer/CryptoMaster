<template>
  <div class="min-h-screen">
    <div class="pb-72">
      <navBar></navBar>
      <LayoutPage></LayoutPage>
      <foot class="fixed bottom-0"></foot>
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

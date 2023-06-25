<script>
import PopupContainer from "./PopupContainer.vue";
import HelpHomePage from "./HelpHomePage.vue";
export default {
  components: {
    PopupContainer,
    HelpHomePage,
  },
  props: {
    config: {
      type: Object,
      required: true
    }
  }
}
</script>

<template>
  <div class="text-center pt-20 pb-10 grid justify-center items-center">
    <div class="flex items-center">
      <img :src="config.homePage.handHello" alt="hand" class="w-12">
      <h1 class="text-2xl font-black text-white px-3">{{config.homePage.title}}</h1>
    </div>
    <PopupContainer :config="config" />
    <HelpHomePage  :config="config" />
  </div>
</template>

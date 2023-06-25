<script>
import PopupContainer from "./PopupContainer.vue";
import PopupHelp from "./PopupHelp.vue";
export default {
  components: {
    PopupContainer,
    PopupHelp,
  },
  props: {
    config: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      img: 'https://mwcorvettes.com/wp-content/uploads/2019/03/savethewave-300x252.gif'
    }
  }
}
</script>

<template>
  <div class="text-center pt-20 pb-10 grid justify-center items-center">
    <div class="flex items-center">
      <img :src="img" alt="hand" class="w-12">
      <h1 class="text-2xl font-black text-white px-3">{{config.homePage.title}}</h1>
    </div>
    <PopupContainer :config="config" />
    <PopupHelp :config="config" />
  </div>
</template>

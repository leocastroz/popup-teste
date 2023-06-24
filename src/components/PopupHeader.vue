<template>
  <div class="text-center pt-20 pb-10 grid justify-center items-center">
    <div class="flex items-center">
      <img src="../assets/images/hand.gif" alt="" class="w-12">
      <h1 class="text-2xl font-black text-white px-3">POPUP - POPCONVERT</h1>
    </div>
    <PopupContainer />
    <PopupHelp />
  </div>
</template>

<script>
import PopupContainer from "./PopupContainer.vue";
import PopupHelp from "./PopupHelp.vue";
export default {
  components: {
    PopupContainer,
    PopupHelp,
  }
}
</script>

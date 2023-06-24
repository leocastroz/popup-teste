<template>
  <div class="text-center w-80 mx-auto flex">
    <p class="text-white font-medium text-sm">{{config.homePage.subDescription}}</p>
  </div>
</template>

<script>
export default {
  props: {
    config: {
      type: Object,
      required: true
    }
  }
}
</script>

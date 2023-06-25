<template>
  <div>
    <div class="flex justify-between">
      <h2 class="font-black text-base my-5 text-white">
        {{ config.firstModal.title }}
      </h2>
      <img
        :src="config.firstModal.closeFistModal"
        alt="close"
        class="close cursor-pointer w-5"
        @click="closeIn"
      />
    </div>
    <p class="text-xs/[15px] text-justify pb-4 text-blue-300">{{ config.firstModal.subtitle }}</p>
    <div class="w-full flex justify-center">
      <img :src="config.firstModal.imgURL" alt="slot" class="w-40" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    config: {
      type: Object,
      required: true,
    },
  },
  methods: {
    closeIn() {
      this.$emit("close-in");
    },
  },
};
</script>

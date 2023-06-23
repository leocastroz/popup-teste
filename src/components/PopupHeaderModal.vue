<template>
  <div>
    <div class="flex justify-between">
      <h2 class="dates-title font-black text-base my-5">
        {{ config.title }}
      </h2>
      <img
        :src="config.close"
        alt="close"
        class="close cursor-pointer w-5"
        @click="closeIn"
      />
    </div>
    <p class="introdution">{{ config.subtitle }}</p>
    <div class="w-full flex justify-center">
      <img :src="config.videoURL" alt="GIF" class="w-40" />
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

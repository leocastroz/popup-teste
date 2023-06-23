<template>
  <div>
    <div class="register flex justify-between mt-5">
      <h2 class="text-base font-black">{{ config.video.title }}</h2>
      <img
        :src="config.close"
        alt="GIF"
        class="close cursor-pointer w-5"
        @click="closeSeconde"
      />
    </div>
    <p class="mt-3 mb-5 text-xs/[15px] text-justify pb-4 text-blue-300">
      {{ config.video.descriptionVideo }}
    </p>
    <div class="flex items-center justify-center">
      <video controls class="w-11/12 rounded">
        <source :src="config.video.videoURL" type="video/mp4" />
      </video>
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
    closeSeconde() {
      this.$emit("closeSecondModal");
    },
  },
};
</script>


<script>
import '../../assets/config/popupbuttons.css'

export default {
  props: {
    config: {
      type: Object,
      required: true,
    },
    openModal: {
      type: Function,
      required: true,
    },
  },
}
</script>

<template>
  <div class="text-center p-10 flex justify-center">
    <button
      class="popup-button py-2 px-5 bg-black tracking-wide font-black hover:bg-blue-500 text-xs border-solid border-4 border-sky-500 cursor-pointer text-white rounded border-solid m-5"
      @click="openModal('form')"
    >
      {{ config.homePage.buttonAbout }}
    </button>
    <button
      class="my-data items-center flex text-xs tracking-wide font-black py-3 px-5 m-5 bg-white text-black border-none rounded hover:bg-green-300"
      @click="openModal('news')"
    >
      <p>{{ config.homePage.watchVideo }}</p>
      <img :src="config.homePage.playerVideo" alt="player" class="ml-2" />
    </button>
  </div>
</template>

<template>
  <div class="text-center p-10 flex justify-center">
    <button
      class="popup-button py-2 px-5 bg-black tracking-wide font-black hover:bg-blue-500 border-solid border-2 border-sky-500  hover:text-white cursor-pointer text-xs text-white rounded border-2 border-solid m-5"
      @click="openModal('form')"
    >
      SOBRE O JOGO
    </button>
    <button
      class="my-data items-center flex text-xs tracking-wide font-black py-3 px-5 m-5 bg-white text-black border-none rounded hover:bg-green-300"
      @click="openModal('news')"
    >
      <p>VER VÍDEO</p>
      <img src="../assets/images/player.svg" alt="player" class="ml-2" />
    </button>
  </div>
</template>

<script>
import '../assets/config/popupbuttons.css';

export default {
  props: {
    openModal: {
      type: Function,
      required: true,
    },
  },
};
</script>
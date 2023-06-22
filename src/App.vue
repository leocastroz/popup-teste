<template>
  <div>
    <Popup :config="popupConfig['caça-níquel']" />
  </div>
</template>

<script setup>
import Popup from './components/Popup.vue';
import popupConfig from './data/popupConfig.json'


const setup = () => {
  return {
    popupConfig
  };
};
</script>

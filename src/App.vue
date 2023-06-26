<template>
  <div>
    <Popup :config="popupConfig['slot-machine']" />
  </div>
</template>

<script setup>
import Popup from './components/Popup.vue'
import popupConfig from './data/popupConfig.json'

const setup = () => {
  return {
    popupConfig,
  }
}
</script>

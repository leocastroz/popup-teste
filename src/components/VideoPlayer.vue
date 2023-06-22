<script>
import jsonData  from '../data/popupConfig';

export default {
  name: 'VideoPlayer',
  props: {
    videoBaseURL: {
      type: String,
      required: true
    }
  },
}
</script>

<template>
  <div>
    <video controls>
      <source :src="videoBaseURL" type="video/mp4">
    </video>
  </div>
</template>
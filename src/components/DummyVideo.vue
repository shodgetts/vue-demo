<template>
  <div class="component-container">
    <iframe 
      class="youtube-video"
      :src="videoSrc"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>
</template>

<script>
export default {
  name: 'DummyVideo',
  data() {
    return {
      videoId: 'BQVpzCxumGY' // Replace with your YouTube video ID
    };
  },
  computed: {
    videoSrc() {
      return `https://www.youtube.com/embed/${this.videoId}`;
    }
  }
};
</script>

<style scoped>
.component-container {
  max-width: 600px;
  /* margin: 20px; */
  padding: 0; /* Remove padding to fit the video */
  height: 500px; /* Set to maintain aspect ratio */
  background-color: #000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.youtube-video {
  width: 100%;
  height: 100%;
  border: none;
}
</style>

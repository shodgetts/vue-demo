<template>
  <div id="app">
    <div class="center-container">
      <div class="component-container dummy-video">
        <DummyVideo />
      </div>
      <div class="component-container chat-window">
        <ChatWindow />
      </div>
    </div>
  </div>
</template>

<script>
import DummyVideo from './components/DummyVideo.vue';
import ChatWindow from './components/ChatWindow.vue';

export default {
  name: 'App',
  components: {
    DummyVideo,
    ChatWindow,
  },
};
</script>

<style>
html, body, #app {
  height: 100%;
  margin: 0;
  font-family: Arial, sans-serif; /* Use a clean, sans-serif font */
  background-color: #f0f2f5; /* Light gray background for the app */
}

.center-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  min-height: 100vh; /* Full viewport height */
  gap: 20px; /* Gap between components */
  padding: 20px; /* Padding around the components */
}

.component-container {
  width: 100%;
  max-width: 800px; /* Maximum width for the component containers */
  border-radius: 8px; /* Rounded corners for a modern look */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
  overflow: hidden; /* Ensure contents are contained within the borders */
}

.dummy-video {
  background-color: #000; /* Black background to mimic a video player */
  color: #fff; /* White text color for contrast */
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px; /* Padding inside the video component */
  font-size: 18px; /* Font size for any text inside */
}

.chat-window {
  background-color: #ffffff; /* White background for the chat window */
  border: 2px solid #004282; /* Blue border to match the theme */
  height: 600px; /* Set a fixed height for the chat window */
  padding: 20px; /* Padding around the chat content */
}
</style>

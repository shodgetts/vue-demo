<template>
  <div id="chat-container">
    <div v-if="messages.length" class="messages" ref="messagesContainer">
      <Message
        v-for="message in messages"
        :key="message.id"
        :message="message"
      />
    </div>
    <input v-model="newMessage" placeholder="Type your message here..." @keyup.enter="sendMessage" />
  </div>
</template>

<script>
import { StreamChat } from 'stream-chat';
import Message from './Message.vue';

export default {
  name: "ChatWindow",
  components: {
    Message,
  },
  data() {
    return {
      chatClient: null,
      channel: null,
      messages: [],
      newMessage: '',
    };
  },
  async created() {
    // Initialize the Stream Chat client
    this.chatClient = new StreamChat('u2794wrxzrqh');

    // Connect the user
    await this.chatClient.connectUser(
      {
        id: 'katie',
        name: 'Katie',
      },
      'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoia2F0aWUifQ.8ASzuIgC_Z3k8oH47uDCvUpVwlHzeoYU8iwdlheB-2k' // Replace with the actual token
    );

    // Get or create a channel
    this.channel = this.chatClient.channel('messaging', 'folk-lime-katie-1', {
      name: 'Folk Lime Katie 1',
    });

    // Watch the channel to receive messages
    await this.channel.watch();

    // Listen to new messages
    this.channel.on('message.new', event => {
      this.messages.push(event.message);
      this.scrollToBottom();
    });

    // Fetch the initial message history
    const messages = await this.channel.query({ messages: { limit: 20 } });
    this.messages = messages.messages;

    // Scroll to bottom initially
    this.scrollToBottom();
  },
  methods: {
    async sendMessage() {
      if (!this.newMessage.trim()) return;

      // Send the message
      await this.channel.sendMessage({
        text: this.newMessage,
      });

      // Clear the input
      this.newMessage = '';
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const container = this.$refs.messagesContainer;
        if (container) {
          container.scrollTop = container.scrollHeight;
        }
      });
    },
  },
  watch: {
    messages() {
      this.scrollToBottom();
    },
  },
};
</script>

<style scoped>
#chat-container {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #004282; /* Blue border */
  background-color: #f8f9fa; /* Light background for contrast */
  border-radius: 5px; /* Rounded corners for a modern look */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
}

.messages {
  max-height: 400px;
  overflow-y: auto;
  margin-bottom: 20px;
  border: 1px solid #004282; /* Blue border around messages area */
  background-color: #ffffff; /* White background for messages */
  border-radius: 5px;
  padding: 10px;
}

input {
  width: 100%;
  padding: 15px;
  box-sizing: border-box;
  border: 1px solid #004282; /* Blue border */
  border-radius: 5px; /* Rounded corners */
  margin-top: 10px;
  outline: none;
}

input::placeholder {
  color: #888;
}

input:focus {
  border-color: #4a2fd1; /* Red border on focus */
}
</style>

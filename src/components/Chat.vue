<template>
  <div id="chat-container">
    <div v-if="messages.length" class="messages">
      <div
        v-for="message in messages"
        :key="message.id"
        class="message"
      >
        <strong>{{ message.user.id }}:</strong>
        {{ message.text }}
      </div>
    </div>
    <input
      v-model="newMessage"
      placeholder="Type your message here..."
      @keyup.enter="sendMessage"
    />
  </div>
</template>

<script>
import { StreamChat } from "stream-chat";

export default {
  name: "ChatComponent",
  data() {
    return {
      chatClient: null,
      channel: null,
      messages: [],
      newMessage: "",
    };
  },
  async created() {
    // Initialize the Stream Chat client
    this.chatClient = new StreamChat(
      "u2794wrxzrqh"
    );

    // Connect the user
    await this.chatClient.connectUser(
      {
        id: "katie",
        name: "Katie The Dog",
      },
      "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoia2F0aWUifQ.8ASzuIgC_Z3k8oH47uDCvUpVwlHzeoYU8iwdlheB-2k"
    );

    // Get or create a channel
    this.channel = this.chatClient.channel(
      "messaging",
      "general",
      {
        name: "General Chat",
      }
    );

    // Watch the channel to receive messages
    await this.channel.watch();

    // Listen to new messages
    this.channel.on("message.new", (event) => {
      this.messages.push(event.message);
    });

    // Fetch the initial message history
    const messages = await this.channel.query({
      messages: { limit: 20 },
    });
    this.messages = messages.messages;
  },
  methods: {
    async sendMessage() {
      if (!this.newMessage.trim()) return;

      // Send the message
      await this.channel.sendMessage({
        text: this.newMessage,
      });

      // Clear the input
      this.newMessage = "";
    },
  },
};
</script>

<style scoped>
#chat-container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
}
.messages {
  max-height: 400px;
  overflow-y: auto;
  margin-bottom: 20px;
}
.message {
  padding: 10px;
  border-bottom: 1px solid #f1f1f1;
}
input {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}
</style>

<template>
  <div class="message">
    <div class="message-content">
      <strong>{{ message.user.name }}:</strong> {{ message.text }}
    </div>
    <div class="actions">
      <div class="reactions">
        <button @click="react('like')">👍 {{ likes }}</button>
        <button @click="react('love')">❤️ {{ loves }}</button>
      </div>
      <div class="menu">
        <button class="menu-button" @click="toggleMenu">⋮</button>
        <div v-if="menuVisible" class="menu-options">
          <button @click="flagMessage">🚩 Flag</button>
          <!-- Add more options here as needed -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MessageComponent',
  props: {
    message: Object,
  },
  data() {
    return {
      likes: this.getReactionCount('like'),
      loves: this.getReactionCount('love'),
      menuVisible: false,
    };
  },
  methods: {
    getReactionCount(reactionType) {
      return this.message.reaction_counts && this.message.reaction_counts[reactionType]
        ? this.message.reaction_counts[reactionType]
        : 0;
    },
    async react(reactionType) {
      try {
        await this.$parent.channel.sendReaction(this.message.id, { type: reactionType });
        if (reactionType === 'like') this.likes += 1;
        if (reactionType === 'love') this.loves += 1;
      } catch (error) {
        console.error('Error sending reaction:', error);
      }
    },
    toggleMenu() {
      this.menuVisible = !this.menuVisible;
    },
    async flagMessage() {
      try {
        const flag = await this.$parent.chatClient.flagMessage(this.message.id);
        if (flag) {
          console.log('Message flagged successfully');
        }
        this.menuVisible = false; // Close the menu after flagging
      } catch (error) {
        console.error('Error flagging message:', error);
      }
    },
  },
};
</script>

<style scoped>
.message {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 0.2px solid #d3d8de;
  background-color: #f5f5f5;
  transition: background-color 0.3s;
}

.message-content {
  flex-grow: 1;
  font-size: 16px;
  color: #333;
}

.message-content strong {
  color: #004282;
}

.actions {
  display: flex;
  align-items: center;
}

.reactions {
  display: flex;
  gap: 10px;
}

button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  color: #555;
  transition: color 0.3s;
}

button:hover {
  color: #d12f2f;
}

.menu {
  position: relative;
  margin-left: 10px;
}

.menu-button {
  font-size: 18px;
  color: #555;
}

.menu-options {
  position: absolute;
  right: 0;
  background: #fff;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  overflow: hidden;
}

.menu-options button {
  display: block;
  width: 100%;
  padding: 10px 20px;
  border: none;
  background: none;
  text-align: left;
  font-size: 16px;
  color: #333;
}

.menu-options button:hover {
  background-color: #f0f0f0;
  color: #d12f2f;
}

.message:hover {
  background-color: #e5e5e5;
}
</style>

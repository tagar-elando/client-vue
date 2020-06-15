<template>
  <div class="centered-content">
    <div class="container-chat">
      <div class="main-messages">
        <div
          v-for="(msg, index) in room.messages"
          :id="`${msg.user}-${index}`"
          :key="`msg-${msg.user}-${index}`"
          :class="['msg', msg.user === user ? 'my-msg' : 'stranger-msg']"
        >
          <p>{{ msg.body }}</p>
        </div>
      </div>
      <form @submit.prevent="sendMessage" class="main-bar">
        <button type="button" class="button is-black has-text-white" @click="exit">
          Exit
        </button>
        <div class="main-input">
          <input type="text" v-model="text" placeholder="Type here..." />
        </div>
        <button type="submit" class="button is-primary has-text-white">
          Send
        </button>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

import { IRoom, IMessage } from "../shared/interfaces";

@Component({})
export default class ChatPage extends Vue {
  @Prop() user!: string;
  @Prop() rooms!: Array<IRoom>;
  text = "";

  sendMessage() {
    this.$emit("send:message", this.room, this.text);
    this.text = "";
  }

  exit(){
    this.$emit("exit", this.room);
  }

  get room() {
      return this.rooms[0];
  }
}
</script>

<style lang="scss" scoped>
.container-chat {
  height: 90vh;
  width: 75vw;
  max-width: 840px;
  background: $title-color;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-radius: 3px;
}

.main-bar {
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: stretch;
}

.main-input {
  flex-grow: 1;

  input {
    background: rgba(255, 255, 255, 0.5);
    width: 100%;
    height: 100%;
    padding: 0 20px;
    border: 0;
    outline: 0;
  }
}

.main-messages {
  .msg {
    margin-bottom: 6px;
    width: 100%;

    p {
      padding: 0 20px;
      display: inline-flex;
      height: 35px;
      align-items: center;
      border-radius: 15px;
    }

    &.my-msg {
      text-align: right;
      p {
        background: #ffeaa7;
      }
    }

    &.stranger-msg {
      p {
        background: #dfe6e9;
      }
    }
  }
}
</style>
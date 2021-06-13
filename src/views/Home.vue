<template>
  <Header />
  <div class="home">
    <div class="messages-container">
      <div v-for="message in messages" :key="message._id" class="record">
        <div class="user-container">
          <img src="../assets/user.png" alt="" class="user-icon" />
          <span class="username">{{ message.user_name }}</span>
          <span class="date">{{ message.createdAt.slice(0, 10) }}</span>
        </div>
        <div class="message-container">
          <p class="message">{{ message.content }}</p>

          <div
            class="message-actions-wrapper"
            v-if="message.user_id === this.$store.state.user?._id"
          >
            <button class="editBtn" @click="editMessage">Edit</button>
            <button class="deleteBtn" @click="deleteMessage(message._id)">
              Delete
            </button>
          </div>
        </div>

        <div v-for="reply in replies" :key="reply._id">
          <div
            v-if="reply.message_id === message._id"
            class="replies-container"
          >
            <div class="reply-user-container">
              <img src="../assets/user.png" alt="" class="user-icon" />
              <span class="username">{{ reply.user_name }}</span>
              <span class="date">{{ reply.createdAt.slice(0, 10) }}</span>
            </div>
            <span class="reply">{{ reply.content }}</span>
            <div
              class="reply-actions-wrapper"
              v-if="message.user_id === this.$store.state.user?._id"
            >
              <button class="editBtn">E</button>
              <button class="deleteBtn" @click="deleteReply(reply._id)">
                D
              </button>
            </div>
          </div>
        </div>
        <div class="add-reply-container">
          <input
            type="text"
            placeholder="Add a reply"
            @keyup="addNewReply($event, message._id)"
          />
        </div>
      </div>
    </div>

    <form class="new-message-wrapper" @submit.prevent="addNewMessage">
      <input
        placeholder="Type your new message for your friends :)"
        v-model="newMsg"
      />
      <button type="submit">New Message</button>
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import axios from "../axios";

export default {
  name: "Home",
  components: {
    Header,
  },
  data() {
    return {
      messages: [],
      replies: [],
      user: {},
      newMsg: "",
    };
  },
  methods: {
    getMessages() {
      fetch("http://localhost:8999/message")
        .then((res) => res.json())
        .then((data) => (this.messages = data));
    },
    getReplies() {
      fetch("http://localhost:8999/reply")
        .then((res) => res.json())
        .then((data) => (this.replies = data));
    },
    addNewMessage() {
      if (this.newMsg.trim()) {
        axios
          .post(`message/${this.user._id}/add`, {
            content: this.newMsg,
            user_id: this.user._id,
            user_name: this.user.user_name,
          })
          .then((res) => {
            this.newMsg = "";
            this.getMessages();
          });
      }
    },
    addNewReply(e, msgId) {
      if (e.target.value.trim() && e.key === "Enter") {
        axios
          .post(`reply/${this.user._id}/add`, {
            content: e.target.value,
            message_id: msgId,
            user_id: this.user._id,
            user_name: this.user.user_name,
          })
          .then((res) => {
            e.target.value = "";
            this.getReplies();
          });
      }
    },
    deleteMessage(msgId) {
      axios.delete(`/message/${msgId}/delete`).then(() => {
        this.getMessages();
      });
    },
    deleteReply(replyId) {
      axios.delete(`/reply/${replyId}/delete`).then(() => {
        this.getReplies();
      });
    },
  },
  created() {
    if (!this.$store.state.user) {
      this.$router.push({ name: "Login" });
    }
  },
  mounted() {
    axios("message").then((res) => {
      this.messages = res.data;
      axios("reply").then((res) => (this.replies = res.data));
    });
    this.user = this.$store.getters.getUserInfo;
  },
};
</script>

<style>
.home {
  padding: 20px 50px;
  display: flex;
  flex-direction: column;
}

.messages-container {
  max-width: 1000px;
  height: 450px;
  overflow-y: auto;
  padding: 10px 20px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 9px;
}

.messages-container::-webkit-scrollbar {
  width: 1em;
}

.messages-container::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

.messages-container::-webkit-scrollbar-thumb {
  background-color: #2c3e50;
  /* outline: 1px solid slategrey; */
  border-radius: 10px;
  border: 3px solid #2c3e50;
}

.messages-container .record {
  border-bottom: 1px solid lightgray;
  margin-bottom: 15px;
}

.user-container {
  display: flex;
  align-items: flex-end;
}

.user-container .username,
.reply-user-container .username {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.user-container .date {
  color: burlywood;
  margin-left: 10px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

.reply-user-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100px;
}

.reply-user-container .username {
  font-size: 12px;
}

.reply-user-container .date {
  color: burlywood;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-size: 12px;
}

.user-icon {
  width: 22px;
  margin-right: 10px;
}

.message-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 10px 0 25px;
}

.replies-container,
.add-reply-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 50px 0 50px;
}

.add-reply-container {
  margin-top: 10px;
}

.message,
.reply {
  word-break: break-all;
  margin-right: 20px;
  color: #2c3e50;
  background-color: #eee;
  width: fit-content;
  border-radius: 6px;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}

.message {
  background-color: transparent;
}

.reply {
  padding: 7px;
  margin-bottom: 10px;
  font-size: 14px;
}

.message-actions-wrapper,
.reply-actions-wrapper {
  display: flex;
  margin-left: auto;
}

.add-reply-container {
  margin-bottom: 10px;
}

.add-reply-container input {
  display: block;
  resize: none;
  padding: 5px 10px;
  flex: 1;
  box-sizing: border-box;
  border: none;
  border: 1px solid #ddd;
  border-radius: 6px;
  outline: none;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-size: 16px;
}

.add-reply-btn {
  display: none;
}

.editBtn {
  display: inline-block;
  padding: 7px 10px;
  height: fit-content;
  font-size: 16px;
  color: snow;
  background-color: #2c3e50;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.deleteBtn {
  margin-left: 10px;
  padding: 7px 10px;
  font-size: 16px;
  color: snow;
  height: fit-content;
  background-color: firebrick;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reply-actions-wrapper .editBtn,
.reply-actions-wrapper .deleteBtn {
  font-size: 12px;
}

.new-message-wrapper {
  display: flex;
  align-items: center;
  max-width: 1000px;
  padding: 10px 20px;
  margin-top: 20px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 9px;
}

.new-message-wrapper textarea {
  display: block;
  resize: none;
  padding: 5px 10px;
  flex: 1;
  box-sizing: border-box;
  border: none;
  border: 1px solid #ddd;
  border-radius: 6px;
  outline: none;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-size: 16px;
  /* background-color: snow; */
}

.new-message-wrapper button {
  display: block;
  width: 180px;
  margin-left: 20px;
  padding: 10px;
  font-size: 16px;
  color: snow;
  background-color: goldenrod;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.new-message-wrapper button:hover {
  background-color: darkgoldenrod;
}
</style>

<template>
  <div>
    <!-- Button -->
    <a href="#" @click="activate" class="float-button" :class="{active: isActive}">
      <!-- <i class="fa fa-plus my-float"></i> -->
      <img class="button-icon" src="../assets/img/chatbot-icon-01.png" alt>
      <div class="cross">
        <div class="line left"></div>
        <div class="line right"></div>
      </div>
      <div class="text-button">Need Help?</div>
    </a>
    <!-- Box -->
    <div class="box-chat" :class="{active: isActive,fullscreen: isFullScreen}">
      <!-- Header -->
      <div class="box-header">
        <img class="hand" src="../assets/img/chatbot-icon-04.png" alt title="full screen">
        <p class="hi">
          <strong>Hi Emilie,</strong>
          <br>How can I help you today?
        </p>
      </div>
      <div class="conv-container" id="container">
        <Bubble v-for="(line, index) in chatlog" :key="index" :type="line.type" :text="line.text" :images="line.images"/>
      <div v-if="isWaiting" class="bot-message-container">
        <div class="img-avatar-container"></div>
        <div class="chat-bubble bot">
          <div id="wave">
            <span class="dot"></span>
            <span class="dot"></span>
            <span class="dot"></span>
          </div>
        </div>
      </div>
      </div>
      <!-- submit -->
      <form class="submit-bar" action="POST" @submit.prevent="sendQuestion" autocomplete="off">
        <a href="#" @click="toggleFullscreen">
          <i class="fas fa-expand"></i>
        </a>
        <input
          v-model="text"
          id="question"
          type="text"
          placeholder="Type your message"
          maxlength="140"
        >
        <button type="submit" class="btn">
          <i class="fab fa-telegram-plane"></i>
        </button>
      </form>
    </div>
  </div>

  <!-- /// -->
  <!-- <div class="chat-window">
      <Bubble v-for="(line, index) in chatlog" :key="index" :type="line.class" :text="line.text"/>
  </div>-->
  <!-- <form @submit.prevent="sendQuestion">
      <input id="question" type="text" placeholder="Type your message" maxlength="140">
      <button type="submit" class="btn">
        <i class="far fa-arrow-alt-circle-right"></i>
      </button>
  </form>-->
  <!-- </div> -->
</template>

<script>
import axios from 'axios';
import Bubble from "./Bubble.vue";
export default {
  data() {
    return {
      text: "",
      chatlog: [],
      request: {conversationId:"",text:"oops",images:[]},
      isActive: false,
      isFullScreen: false,
      isWaiting: false,
      conversationStarted: false,
      conversationId: ""
    };
  },
    created(){

    },
  components: {
    Bubble
  },
  methods: {
    startConversation() {
      if (!this.conversationStarted) {
        this.conversationStarted = true;
        axios.post("https://chatbotmwla5.azurewebsites.net/api/values/start")
        .then(response => {
          this.conversationId = response.data;

      })
      }
    },
    activate() {
      this.startConversation();
      this.isActive = !this.isActive;
    },
    toggleFullscreen() {
      this.isFullScreen = !this.isFullScreen;
    },
  	scrollToEnd() {   	
      var container = this.$el.querySelector("#container");
      container.scrollTop = container.scrollHeight;
    },
    sendQuestion() {
      if (this.text != "") {
      /* eslint-disable no-console */
      console.log(this.text);
      /* eslint-enable no-console */
        let question = {
          text: this.text,
          type: "question",
          images: []
        };
        this.chatlog.push(question);
        this.request.conversationId = this.conversationId;
        this.request.text = this.text;
        this.$nextTick(()=>{
          this.scrollToEnd();
        })
        this.isWaiting = true;
      /* eslint-disable no-console */
      // console.log(this.request);
      /* eslint-enable no-console */
        axios.post("https://chatbotmwla5.azurewebsites.net/api/values/message", this.request)
        .then(response => {
          setTimeout(() =>{
            this.isWaiting = false;
            this.text = "";
            let answer = {
              text: response.data.text,
              type: "answer",
              images:response.data.images
            };
            this.chatlog.push(answer);
            this.scrollToEnd();
          }, 500)
        })
        .catch((error) => {
            this.isWaiting = false;
            this.text = "";
            let answer = {
              text: "Oops, something went wrong :(",
              type: "answer",
              images:[""]
            };
            this.chatlog.push(answer);
            this.scrollToEnd();
      /* eslint-disable no-console */
      console.log("error: "+error);
      /* eslint-enable no-console */
  })
        
      }
    },
  }
};
</script>

<style lang="scss">
@import "https://use.fontawesome.com/releases/v5.7.2/css/all.css";
</style>

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
        <img class="hand" src="../assets/img/main-hello.gif" alt title="full screen">
        <p class="hi">
          <strong>Hi Emilie,</strong> it's Simon.
          <br>How can I help you today?
        </p>
      </div>
      <div class="conv-container" id="container" @click="imgClick">
        <Bubble
          v-for="(line, index) in chatlog"
          :key="index"
          :type="line.type"
          :text="line.text"
          :images="line.images"
           @click="imgClick"
        />
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
    <div class="lightbox" :class="{open: isOpen}" @click="close"><img :src="imgSrc" @click="close"></div>
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
import axios from "axios";
import Bubble from "./Bubble.vue";
export default {
  data() {
    return {
      text: "",
      chatlog: [],
      request: { conversationId: "", text: "oops", images: [] },
      isActive: false,
      isFullScreen: false,
      isWaiting: false,
      conversationStarted: false,
      conversationId: "",
      isOpen: false,
      imgSrc: ""
    };
  },
  created() {},
  components: {
    Bubble
  },
  methods: {
    startConversation() {
      if (!this.conversationStarted) {
        this.conversationStarted = true;
        axios
          .post("https://chatbotmwla5.azurewebsites.net/api/values/start")
          .then(response => {
            this.conversationId = response.data;
          });
      }
    },
    activate(event) {
      event.preventDefault();
      this.startConversation();
      this.isActive = !this.isActive;
    },
    toggleFullscreen(event) {
      event.preventDefault();
      this.isFullScreen = !this.isFullScreen;
    },
    scrollToEnd() {
      var container = this.$el.querySelector("#container");
      container.scrollTop = container.scrollHeight;
    },
    sendQuestion() {
      if (this.text != "" && !this.isWaiting) {
        let question = {
          text: this.text,
          type: "question",
          images: []
        };
        this.chatlog.push(question);
        this.request.conversationId = this.conversationId;
        this.request.text = this.text;
        this.$nextTick(() => {
          this.scrollToEnd();
        });
        this.text = "";
        this.isWaiting = true;
        axios
          .post(
            "https://chatbotmwla5.azurewebsites.net/api/values/message",
            this.request
          )
          .then(response => {
            setTimeout(() => {
              this.isWaiting = false;
              let answer = {
                text: response.data.text,
                // text: "It’s a first-person view that allows you to navigate into your process without a Virtual Reality kit. Use the arrows on your keyboard to navigate and your mouse or pad to look around. Click on a piece of equipment to display its information sheet. ↵ ![product_portfolio-1024x507.png](http://hakobio-support.com/wp-content/uploads/2018/07/product_portfolio-1024x507.png)",
                type: "answer",
                images: response.data.images
              };
              this.chatlog.push(answer);
              this.$nextTick(() => {
                this.scrollToEnd();
              });
            }, 500);
          })
          .catch(error => {
            this.isWaiting = false;
            this.text = "";
            let answer = {
              text: "Oops, something went wrong :(",
              type: "answer",
              images: [""]
            };
            this.chatlog.push(answer);
            this.$nextTick(() => {
              this.scrollToEnd();
            });
            /* eslint-disable no-console */
            console.log("error: " + error);
            /* eslint-enable no-console */
          });
      }
    },
    imgClick(ev) {
            /* eslint-disable no-console */
            console.log(ev.target);
            /* eslint-enable no-console */
      if (ev.target.getAttribute("src")) {
      this.imgSrc = ev.target.getAttribute("src");
      this.isOpen = true;
      }
    },
    close() {
      this.isOpen = false;
      this.imgSrc = "";
    }
  }
};
</script>

<style lang="scss">
@import "https://use.fontawesome.com/releases/v5.7.2/css/all.css";
</style>

<template>
    <div>
      <div class="chat-window">
        <Bubble v-for="(line, index) in chatlog" :key="index"/>
      </div>
      <form @submit.prevent="sendQuestion">
      <input id="question" type="text" placeholder="Type your message" maxlength="140"/>
      <button type="submit" class="btn"><i class="far fa-arrow-alt-circle-right"></i></button>
      </form>
    </div>
</template>

<script>
export default {
  data(){
    return {
      chatlog: [],
    }
  },
  methods: {
    sendQuestion() {
       let question = {text:document.getElementById("question").value,class:question};
       this.chatlog.push(question);
       console.log(this.chatlog[0]);
       
    }
  }
}
</script>

<style lang="scss">
@import 'https://use.fontawesome.com/releases/v5.7.2/css/all.css';
.chat-window{
    height: 300px;
    width:80%;
    background:grey;
}
</style>

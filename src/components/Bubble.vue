<template>
    <!-- <div :class="type">{{text}}</div> -->
    <div>
     <!-- bot answer -->
        <div v-if= "type=='answer'" class="bot-message-container">
          <div class="img-avatar-container"></div>
          <div class="chat-bubble bot">
            <vue-markdown  :source="text"></vue-markdown>
          </div>
        </div>
     <!-- bot images answer -->
        <!-- <div v-if= "type=='answer'  && images != ''" class="bot-message-container">
          <div class="img-avatar-container"></div>
          <div class="chat-bubble bot">
            img
          </div>
        </div> -->
        <!-- user question -->
        <div v-else class="user-message-container">
          <div class="chat-bubble user">{{text}}</div>
        </div>
      </div>
</template>

<script>
import VueMarkdown from 'vue-markdown';
export default {
  components: {
    VueMarkdown
  },
  props : ["type","text","images"],
}
</script>
<style>

</style>

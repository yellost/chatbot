<template>
  <body>
    <!-- Button -->
    <a href="#" class="float-button">
        <!-- <i class="fa fa-plus my-float"></i> -->
        <img class="button-icon" src="img/chatbot-icon-01.png" alt="">
        <div class="text-button">Need Help?</div>
    </a>
    <!-- Box -->
    <div class="box-chat">
        <!-- Header -->
        <div class="box-header">
            <img class="hand" src="img/chatbot-icon-04.png" alt="" title="full screen">
            <p class="hi"><strong>Hi Samy,</strong> <br> How can I help you today?</p>
        </div>
        <div class="conv-container">
            <!-- bot answer -->
            <div class="bot-message-container">
                <div class="img-avatar-container"></div>
                <div class="chat-bubble bot">HELLLLLOOOOOOO</div>
            </div>
            <!-- user question -->
            <div class="user-message-container">
                <div class="chat-bubble user">Hello you!!!</div>
            </div>
        </div>
        <!-- submit -->
        <form class="submit-bar" action="POST">
            <i class="fas fa-expand"></i>
            <input type="text">
            <input type="submit" value="">
            <i class="fab fa-telegram-plane"></i>
        </form>

    </div>

</body>
</template>


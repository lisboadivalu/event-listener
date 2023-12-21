<script>
import Pusher from 'pusher-js';

export default {
  data() {
    return {
      notificationData: [] 
    }
  },
  methods: {
    myClient() {
      return new Pusher('app-key', {
        wsHost: '127.0.0.1',
        wsPort: 6001,
        forceTLS: false,
        encrypted: true,
        disableStats: true,
        enabledTransports: ['ws', 'wss'],
        cluster: 'mt1'
      });
    },
    pushNotification() {
      this.myClient().subscribe('notification').bind('notification-created', (data) => {
      var notification = data.dataMessage
      this.notificationData.push({ author: notification.author, description: notification.description })
      })
    },
  },
  created() {
    this.pushNotification()
  }
}

</script>

<template>
    <div class="notification-content greetings" v-for="item in notificationData">
      <div class="photo-container">
        <img 
        class="photo-content"
        src="https://curitibanabagagem.com.br/wp-content/uploads/2021/11/gralha-azul-300x200.jpg" 
        alt="passaro"
        >
      </div>
      <div class="text-container">
        <p>Author: <span>{{ item.author }}</span></p>
        <p>Desription: <span>"{{ item.description }}"</span></p>
      </div>
    </div>
</template>

<style scoped>
.notification-content {
    background-color: #d8d8d8;
    border-radius: 7px;
    margin: 30px 0;
    padding: 10px;
    height: 320px;
    width: 350px;
    color: #141414;
  }

  .text-container p {
    font-weight: 500;
    text-align: center;
    font-size: 1rem;
  }

  .text-container span {
    font-style: italic;
    font-size: 1rem;
  }


  .photo-container {
    margin: 10px auto 30px auto;
    padding: 0 30px;
  }

  .photo-container img{
    object-fit: cover;
    width: 100%;
    height: 100%;
    border-radius: 7px; 
  }

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }


  .notification-content {
    border: red 1px solid;
    margin: 30px 0;
    padding: 10px;
  }
}

</style>

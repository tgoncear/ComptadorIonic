<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="primary">
        <ion-title>Time Fighter</ion-title>
        <ion-buttons slot="primary">
          <ion-button @click="info" color="primary" fill="solid">
            <ion-icon :icon="InfoIcon"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>

    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header class="ion-no-border ion-padding" >
        <ion-grid>
          <ion-row>
            <ion-col id="score" class="ion-text-start"><div>Your Score: {{score}}</div></ion-col>
            <ion-col class="ion-text-end"><div>Time Left: {{timeLeft}}</div></ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>
    
      <div id="container">
        <ion-button id="tapMeButton" @click="tap" color="primary" fill="solid">TAPME</ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  alertController, createAnimation, IonCol,
  IonContent,
  IonGrid,
  IonHeader,
  IonIcon,
  IonPage,
  IonRow,
  IonTitle,
  IonToolbar, toastController, //toastController
} from '@ionic/vue';
import { defineComponent } from 'vue';
import { informationCircleOutline} from 'ionicons/icons';

const INITIAL_TIME = 60
export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonIcon,
    IonGrid,
    IonRow,
    IonCol
  },
  setup(){
    return {
      InfoIcon: informationCircleOutline,
      started: false,
    }
  },
  data(){
    return{
      score: 0,
      timeLeft: INITIAL_TIME
    }

  },
  watch: {
    timeLeft: function (newTimeLeft) {
      if (newTimeLeft <= 0) {
        this.started = false
        this.timeLeft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showResult()
        this.score = 0
      }
    },
  },
  methods:{
    bounce () {
      const animation = createAnimation()
      animation.addElement(document.getElementById('tapMeButton'))
          .duration(2000)
          .fromTo('transform', 'scale(2.0)', 'scale(1.0)')
      animation.play();
    },
    blink () {
      const animation = createAnimation()
      animation.addElement(document.getElementById('score'))
          .duration(500)
          .fromTo('opacity', '0', '1')
      animation.play();
    },
    async info(){
        const alert = await alertController
          .create({
            header: 'Time Fighter 1.0',
            subHeader: 'Creat per Tudor Goncear',
            message: 'Podeu trovar el codi font a <a href="https://github.com/tgoncear/ComptadorIonic">https://github.com/tgoncear/ComptadorIonic</a>',
            buttons: ['OK']
          });
        await alert.present();
        },
    tap () {
      this.bounce()
      this.blink()
      this.score++
      if (!this.started) {
        this.counterInterval = setInterval(() => {
          this.timeLeft--
        },1000)
        this.started = true
      }
    },
    async showResult() {
      // TOAST
      const toast = await toastController.create({
        color: 'dark',
        duration: 2000,
        message: `Time's Up. Your Score was ${this.score}`,
        showCloseButton: true
      });
      await toast.present();
    },

  },
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
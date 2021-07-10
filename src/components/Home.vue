<template>

  <section class="src-components-home">
    <div class="overlay"></div>
    <audio id="audioDrone" loop>
      <source src="../assets/please-let-me-out-drone.mp3" type="audio/mp3">
    </audio>
    <audio id="audioIn" loop>
      <source src="../assets/please-let-me-out-drone-in.mp3" type="audio/mp3">
    </audio>

    <div class="container">
      <h1>Actions have consecuencies</h1>
      <h4>Don't get inside. Write him a message</h4>
      <div class="input-wrapper">
        <input autocomplete="off" id="textInput" type="text" v-model="inputText" @click="typing()" @keydown="handleWrite($event)">
        <div class="input-see" contenteditable="true">{{ achivedText }}</div>
      </div>
      <p class="message reverse" v-if="!needsAnimation">You've made your choice</p>
    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-home',
    props: [],
    mounted () {
      this.textArr = this.textGoal.split('');
      this.subjectIsIn = localStorage.getItem('subjectIsIn');
      this.audio = document.getElementById('audioDrone');
      this.audioIn = document.getElementById('audioIn');
      if(this.subjectIsIn) {
        this.setAfterScene();
      }
      document.querySelector('.overlay').addEventListener('click', () => {
        this.interactionDone = true;
      });
    },
    data () {
      return {
        achivedText: '',
        textGoal: 'Please, let me out!!',
        textArr: [],
        inputText: '',
        needsAnimation: true,
        subjectIsIn: '',
        firstSceneIsSet: false,
        secondSceneIsSet: false,
        count: -1,
        audio: null,
        audioIn: null,
        interactionDone: false
      }
    },
    methods: {
      handleWrite($event) {
        if($event.keyCode !== 8) {
          this.inputText = this.achivedText;
          this.count++;
          this.achivedText = this.achivedText + this.textArr[this.count];
        } else {
          this.count--;
          this.achivedText = this.achivedText.substring(0,this.achivedText.length - 1);
        }
        if(this.count < 0) this.count = - 1;
        if(this.count === this.textArr.length) {
          this.count = this.textArr.length - 1;
          this.achivedText = this.textGoal;
          if(this.needsAnimation) {
            this.addAnimation(true, 'container', 'animation');
            this.setHideScene();
            this.setComeScene();
          }
        }
        this.inputText = this.achivedText;
      },
      setHideScene() {
        this.fader();
        setTimeout(() => {
          this.addAnimation(true, 'src-components-home', 'dark-animation');
          this.firstSceneIsSet = true;
        }, 1000);
        setTimeout(() => {
          this.audio.pause();
        }, 5000);
      },
      setComeScene() {
        setTimeout(() => {
          this.setAfterScene();
        }, 6000);
      },
      setAfterScene() {
        this.subjectIsIn = localStorage.getItem('subjectIsIn');
        if(this.interactionDone) {
          this.audioIn.play();
        } else {
          window.addEventListener('click', () => {
            this.audioIn.play();
          });
        }
        this.$emit('reverse');
        this.addAnimation(true, 'container', 'no-animation');
        this.addAnimation(true, 'src-components-home', 'no-animation');
        this.achivedText = '';
        this.count = -1;
        this.needsAnimation = false;
        localStorage.setItem('subjectIsIn', true);
        setTimeout(() => {
          document.querySelector('.message').classList.add('hide-message');
        }, 5000);
      },
      addAnimation(isClass, element, animation) {
        let type = '';
        isClass ? type = '.' : type = '#';
        const target = document.querySelector(`${type}${element}`);
        target.classList.add(`${animation}`);
      },
      typing() {
        if(this.subjectIsIn !== 'true') {
          if(this.audio.paused) {
            this.play();
          }
        }
      },
      play() {
        this.audio.play();
      },
      fader() {
        const duration = this.audio.duration;
        setTimeout(() => {
          this.audio.currentTime = duration - 5;
        }, 2000);
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  * {
    color: white;
  }

  h1 {
    font-size: 70px;
    font-weight: 500;
    margin-bottom: 20px;
  }

  h4 {
    font-size: 30px;
    margin-bottom: 20px;
  }

  input {
    opacity: 0;
    z-index: 9;
    padding: 10px;
    border: none;
    border-radius: 6px;
    font-size: 20px;
    background-color: #ffffff44;
    color: white;
  }

  #textInput:focus + .input-see {
    box-shadow: 2px 2px 8px 2px #746194a8;
  }

  .no-animation {
    animation: none !important;
  }

  .animation {
    animation: blur 5s forwards;
  }

  .dark-animation {
    animation: dark 5s forwards;
  }

  .input-wrapper {
    position: relative;
    height: 44px;
    width: 272px;
  }

  .input-see {
    height: 100%;
    z-index: 1;
    position: absolute;
    pointer-events: none;
    top: 0;
    left: 0;
    padding: 10px;
    border: none;
    border-radius: 6px;
    font-size: 20px;
    background-color: #ffffff44;
    color: white;
    width: 100%;
    transition: all 0.2s ease-in-out;
  }

  .container {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .reverse {
    -moz-transform: scaleX(-1);
    -o-transform: scaleX(-1);
    -webkit-transform: scaleX(-1);
    transform: scaleX(-1);
    filter: FlipH;
    -ms-filter: "FlipH";
  }

  .message {
    margin-top: 20px;
    font-size: 40px;
    padding: 10px;
    background-color: #ffffff10;
    border-radius: 6px;
    transition: all 4s ease-in-out;
  }

  .hide-message {
    opacity: 0;
    filter: blur(20px);
  }

  .overlay {
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
  }

  @keyframes blur {
    0% {
      filter: blur(0px);
    }
    1% {
      pointer-events: none;
    }
    50% {
      opacity: 0.4;
    }
    99% {
      filter: blur(50px);
    }
    100% {
      opacity: 0;
    }
  }

  @keyframes dark {
    100% {
      background-color: #000000;
    }
  }
</style>

<template>

  <section class="src-components-home">
    <div class="container">
      <h1>{{ title }}</h1>
      <h4>You are inside now. Write your message</h4>
      <div class="input-wrapper">
        <input type="text" placeholder="You decide" v-on:keydown="handleWrite($event)">
        <div class="input-see">{{achivedText}}</div>
      </div>
    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-home',
    props: ['title'],
    mounted () {
      this.textArr = this.textGoal.split('');
    },
    data () {
      return {
        achivedText: '',
        textGoal: 'Please, let me out!!',
        textArr: [],
        count: -1
      }
    },
    methods: {
      handleWrite($event) {
        if($event.keyCode !== 8) {
          this.count++;
          this.achivedText = this.achivedText + this.textArr[this.count];
        } else {
          this.count--;
          this.achivedText = this.achivedText.substring(0,this.achivedText.length -1);
        }
        if(this.count < 0) this.count = -1;
        if(this.count === this.textArr.length) {
          this.count = this.textArr.length - 1;
          this.achivedText = this.textGoal;
          this.blur();
        }
        console.log(this.count);
      },
      blur() {
        const target = document.querySelector('.src-components-home');
        target.classList.add('animation');
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .animation {
    animation: blur 10s forwards;
  }

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
  }

  input {
    opacity: 0;
    z-index: 2;
    padding: 10px;
    border: none;
    border-radius: 6px;
    font-size: 20px;
    background-color: #ffffff44;
    color: white;
  }

  .container {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  @keyframes blur {
    0% {
      filter: blur(0px);
    }
    99% {
      filter: blur(50px);
      opacity: 0.4;
    }
    100% {
      opacity: 0;
    }
  }
</style>

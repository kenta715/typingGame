<template>
  <h1 class="title">Typing Game</h1>
  <div v-if="!isPlay">いずれかのキーを押したらスタート！</div>
  <div v-if="isPlay">
    <div class="timer">
      <div class="time">
        {{ formatTime }}
      </div>
    </div>
    <div class="theme">
      {{ wordList[0] }}	
    </div> 
    <div>
      <input v-model="userInput" type="text" id="inputWord">
    </div>
    <div v-if="result"> 
      <p>OK</p>  
      <button @click="restart">再挑戦！</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
    isPlay: false,
    wordList: ['apple','orange','banana','lemon','grape'],
    result:false,
    userInput:'',
    min:0,
    sec:0,
    secTenth:0,
    timerObj:null,
    }
  },
  methods: {
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
          let r = Math.floor(Math.random() * (i + 1))
          let tmp = array[i]
          array[i] = array[r]
          array[r] = tmp
      }
      return array
    },
    checkWord(newInput){
      if (this.wordList[0] === newInput) {
        clearInterval(this.timerObj)
        this.result = true
      }
    },
    start(){
      this.timerObj = setInterval(this.count, 10)
    },
    count(){
      if(this.secTenth >= 100){
        this.sec++
        this.secTenth = 0
      }
      if (this.sec >= 60) {
        this.min++
        this.sec = 0
      } else {
        this.secTenth ++
      }
    },
    restart(){
      this.min = 0
      this.sec = 0
      this.result = false
      this.wordList = this.shuffle(this.wordList)
      this.userInput = ''
      this.start()
    }
  },
  watch: {
    userInput(newInput) {
      this.checkWord(newInput)
    }
  },
  mounted() {
    addEventListener('keydown', e => {
      if(this.isPlay){
        console.log(e)
        return  
      }
      this.wordList = this.shuffle(this.wordList)
      this.start()
      this.isPlay = true
    })
  },
  computed: {
    formatTime() {
      let timeStrings = [
        this.min.toString(),
        this.sec.toString(),
         this.secTenth.toString()
      ].map(function(str) {
        if (str.length < 2) {
          return "0" + str
        } else {
          return str
        }
      })
      return timeStrings[0] + ":" + timeStrings[1] + "." + timeStrings[2]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.theme{
  font-size: 50px;
  margin-bottom: 30px;
}
#inputWord{
  font-size: 30px;
}
</style>

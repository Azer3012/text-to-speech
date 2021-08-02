<template>
  <Header />
  <div class="app-container">
    <select v-model="selectedVoice">
      <option v-for="voice in voiceList" value="" :key="voice.voiceUrI">
        {{voice.name}}

      </option>
    </select
    ><br />
    <div class="slider-container">
      <input type="range" class="slider" />
    </div>
    <span></span>
    <br />
    <textarea name="" id="" cols="30" rows="10" v-model="textToSpeech"></textarea>
    <div class="preview-container"></div>
    <br />
    <button class="btn red" @click="speak">
      <span>oxu</span>
    </button>
  </div>
</template>

<script>
import Header from "./components/Header.vue";

export default {
  components: { Header },
  name: "App",
  created(){
    this.getVoices().then(voices=>{
      this.voiceList=voices
      this.selectedVoice=""

    })
  },
  data() {
    return {
      tts:window.speechSynthesis,
      voiceList:null,
      selectedVoice:null,
      textToSpeech:"salam"
    }
  },
  methods:{
    getVoices(){
      let intervalId;
      return new Promise((resolve,reject)=>{
        intervalId=setInterval(()=>{

          this.tts.getVoices().length>0 && resolve(this.tts.getVoices())
          clearInterval(intervalId)
        },10)
      })
    },
    
    speak(){
     let toSpeak= new SpeechSynthesisUtterance(this.textToSpeech)

     const foundActiveVoice=this.voiceList.find(voice=>voice.name===this.selectedVoice) ||null

     toSpeak.voice=foundActiveVoice

     this.tts.speak(toSpeak)
    }
  }
};
</script>



<script setup>
import { ref, onMounted } from 'vue';

const speech = ref(null)
const text = ref("")
const voice = ref(0)
const voices = ref(null)
const pitch = ref("1")
const rate = ref("1")

onMounted(() => {
  speech.value = window.speechSynthesis;
  // getVoices()が非同期なので以下のイベントで取る必要があるようです。
  speech.value.onvoiceschanged = (event) => {
    voices.value = speech.value.getVoices()
  }
})

// 再生
function play() {
  const utterThis = new SpeechSynthesisUtterance(text.value)
  utterThis.lang = "ja-JP"
  utterThis.voice = voices.value[voice.value]
  utterThis.pitch = pitch.value
  utterThis.rate = rate.value
  speech.value.speak(utterThis)
}
</script>

<template>
  <input v-model="text" size="35"/> <button @click="play">再生</button><br/>
  <div>
    <span>voice</span> 
    <select v-model="voice">
      <option v-for="(item, index) in voices" :value="index">
        {{ item.name }} ({{ item.lang }})
      </option>
    </select>
  </div>
  <div>
    <span>pitch</span> <input type="range" v-model="pitch" min="0" max="2"/>
  </div>
  <div>
    <span>rate</span> <input type="range" v-model="rate" min="0.5" max="2" step="0.1"/>
  </div>
</template>

<style scoped>
span {
  display: inline-block;
  width: 50px;
}
</style>
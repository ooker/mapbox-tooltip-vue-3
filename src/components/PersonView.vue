<template>
  <div class="info-panel">
    <template v-if="props">
      <div class="emoji">{{props.emoji}}</div>
        <div>
          <h1>{{ props.name }}</h1>
          <h2 v-if="props.nick"><i>"{{ props.nick }}"</i></h2>
          <h4></h4>
        </div>
        <div>
          <InfoBar :value1="props.value1" :value2="props.value2" :value3="props.value3" />
        </div>
      </template>
  </div>
</template>

<script>
  // import { ref } from "vue";
  import InfoBar from "@/components/InfoBar.vue";
  export default {
    props : {
      name : {
        type: String,
        default: "Nipi Tiri"
      },
      nick : {
        type: String,
        default: "Näxikotte"
      },
      emoji : {
        type: String,
        default: "💩"
      },
      value1 : {
        type: Number,
        default: 1
      },
      value2 : {
        type: Number,
        default: 1
      },
      value3 : {
        type: Number,
        default: 1
      },
    },
    components : {
      InfoBar
    },
    setup(props) {
      return {props}
    },
  };
</script>

<style scoped>
  .info-panel {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    
    padding: 3vh 4vw 16vh 4vw;
    text-align: center;
    width: 100%;
    height: 100%;
  }
  .emoji {
    font-size: 12vh;
  }
</style>
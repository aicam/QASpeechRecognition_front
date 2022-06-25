<template>
  <v-container>
    <v-card style="padding: 30px">
      <h2 style="margin-bottom: 10px">{{ this.label }}</h2>
      <v-row class="d-flex" >
        <v-btn
          class="primary"
          :disabled="ActionTxtDisabled"
          @click="startNewRec"
        >
          {{ ActionTxt }}
        </v-btn>
        <v-btn
          class="error"
          :disabled="endButtonDisabled"
          @click="stopRec"
        >
          خاتمه
        </v-btn>
      </v-row>
      <v-row>
        <v-text-field
          class="pt-5 mr-2"
          :disabled="TextDisabled"
          v-model="Txt"
        >
        </v-text-field>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "RecordSpeech",
  props: ['label', 'setter'],
  data() {
    return {
      ActionTxt: 'شروع ضبط',
      endButtonDisabled: true,
      TextDisabled: true,
      ActionTxtDisabled: false,
      Txt: "متن ضبط شده در اینجا نوشته میشود",
      sr: null,
      recordedFirst: true
    }
  },
  mounted() {
    var SpeechRecognition = SpeechRecognition || webkitSpeechRecognition;
    this.sr = new SpeechRecognition();
    this.sr.lang = 'fa';
    this.sr.onstart = () => {
      this.ActionTxtDisabled = true;
      this.endButtonDisabled = false;
      this.TextDisabled = true;
    };
    this.sr.onspeechend = () => {
      this.ActionTxtDisabled = false;
      this.ActionTxt = 'ادامه ضبط';
    }
    this.sr.onresult = (event) => {
      var transcript = event.results[0][0].transcript;
      var confidence = event.results[0][0].confidence;
      if (this.recordedFirst) {
        this.Txt = transcript;
        this.recordedFirst = false;
      }
      else
        this.Txt += " " + transcript;
      this.endButtonDisabled = true;
      this.TextDisabled = false;
      this.ActionTxtDisabled = false;
      this.ActionTxt = 'ادامه ضبط';
      this.setter(this.Txt)
    }
  },
  methods: {
    startNewRec() {
      this.sr.start();
    },
    stopRec() {
      this.sr.stop();
    }
  }
}
</script>

<style scoped>

</style>

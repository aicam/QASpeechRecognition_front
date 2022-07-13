<template>
  <v-card style="padding: 30px;">
    <h1 style="font-size: 34px;">سوال خودرا مطرح کنید</h1>
    <RecordSpeech :setter="qTxtSetter" label="صورت سوال"></RecordSpeech>
    <v-row justify="center" style="padding: 25px;">
      <v-btn @click="askServer">جست و جو</v-btn>
    </v-row>
    <v-container
      style="border-width: 1px; border-style: dashed;"
      v-for="(content, i) in ans"
      :key="i">
      <h3>امتیاز پاسخ {{content.score.toFixed(3)}}</h3>
      <h3>{{content.question}}؟</h3>
      <prism-editor style="direction: ltr !important;" v-if="content.type === 'javascript'" lang="javascript" class="my-editor" v-model="content.answer" :highlight="highlighter" line-numbers></prism-editor>
      <v-btn class="primary" v-if="content.type === 'javascript'" @click="() => {
          vm.runInNewContext(content.answer)
        }">اجرای دستور</v-btn>
      <h3 v-if="content.type === 'simple_question'">{{content.answer}}</h3>
    </v-container>
  </v-card>
</template>

<script>
import { PrismEditor } from 'vue-prism-editor';
import 'vue-prism-editor/dist/prismeditor.min.css'; // import the styles somewhere

// import highlighting library (you can use any library you want just return html string)
import { highlight, languages } from 'prismjs/components/prism-core';
import 'prismjs/components/prism-clike';
import 'prismjs/components/prism-javascript';
import 'prismjs/themes/prism-tomorrow.css';

export default {
  components: {
    PrismEditor
  },
  name: "ask",
  data() {
    return ({
      qTxt: "",
      ans: null,
      vm: require('vm'),
    });
  },
  methods: {
    highlighter(code) {
      return highlight(code, languages.js); // languages.<insert language> to return html with markup
    },
    qTxtSetter(val) {
      this.qTxt = val;
    },
    askServer() {
      this.$axios.post("/answer", {question: this.qTxt}).then(response => {
        this.ans = response.data;
        console.log(this.ans);
      })
    }
  }
}
</script>

<style scoped>

</style>

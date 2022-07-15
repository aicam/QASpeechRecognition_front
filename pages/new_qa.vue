<template>
  <v-card style="padding: 30px;">
    <h1 style="font-size: 34px;">ثبت سوال جدید</h1>
    <RecordSpeech :setter="qTxtSetter" label="صورت سوال"></RecordSpeech>
    <v-radio-group
      v-model="qType">
      <v-row justify="space-around">
        <v-radio
          label="پاسخ متنی"
          value="simple_question"></v-radio>
        <v-radio
          label="کد JS"
          value="javascript"></v-radio>
      </v-row>
    </v-radio-group>
    <prism-editor style="direction: ltr !important;" v-if="qType === 'javascript'" lang="javascript" class="my-editor" v-model="aTxt" :highlight="highlighter" line-numbers></prism-editor>
    <RecordSpeech v-if="qType === 'simple_question'" :setter="aTxtSetter" label="پاسخ سوال"></RecordSpeech>
    <v-card-actions style="margin-top: 20px">
      <v-row>
        <v-text-field
          v-model="docName"
          placeholder="نام سند به انگلیسی مانند driving_test"
        ></v-text-field>
        <v-btn
          large
          class="light-blue"
          @click="submitQA"
        >
          ثبت
        </v-btn>
      </v-row>
    </v-card-actions>


    <!-- snackbar -->
    <v-snackbar
      v-model="snackbar"
    >
      {{ snackbarTxt }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          بستن
        </v-btn>
      </template>
    </v-snackbar>
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
  name: 'InspirePage',
  data() {
    return {
      qTxt: "",
      aTxt: "",
      qType: "simple_question",
      docName: "",
      snackbar: false,
      snackbarTxt: ""
    }
  },
  methods: {
    qTxtSetter(val) {
      this.qTxt = val;
    },
    aTxtSetter(val) {
      this.aTxt = val;
    },
    highlighter(code) {
      return highlight(code, languages.js); // languages.<insert language> to return html with markup
    },
    submitQA() {
      this.$axios.post("/send/question", {
        question: this.qTxt,
        answer: this.aTxt,
        doc_name: this.docName,
        type: this.qType
      }).then(response => {
        console.log(response.data);
        this.snackbarTxt = "سوال و جواب جدید با موفقیت ذخیره شد"
        this.snackbar = true
      })
    }
  }
}
</script>

<style scoped>
.my-editor {
  /* we dont use `language-` classes anymore so thats why we need to add background and text color manually */
  background: #2d2d2d;
  color: #ccc;

  /* you must provide font-family font-size line-height. Example: */
  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
  padding: 5px;
}
/* optional class for removing the outline */
.prism-editor__textarea:focus {
  outline: none;
}

</style>

<template>
  <v-container style="background: #757575; border-radius: 25px">
    <h2>اسناد ذخیره شده</h2>
    <v-row justify="center" style="margin: 10px;">
      <v-btn @click="downloadObjectAsJson(collections, 'full_collections.json')">دانلود تمامی اسناد</v-btn>
    </v-row>
    <v-card>
      <div class="text-center">
        <v-chip
          class="ma-2"
          v-for="(doc_name, i) in doc_names"
          :key="i"
          @click="selectedCollection = collections[doc_name]"
        >
          {{ doc_name }}
        </v-chip>
      </div>
    </v-card>
    <h2>محتوای سند</h2>
    <v-card>
      <v-container
        style="border-width: 1px; border-style: dashed;"
        v-for="(content, i) in selectedCollection"
        :key="i">
          <h3>{{content.question}}؟</h3>
        <prism-editor style="direction: ltr !important;" v-if="content.type === 'javascript'" lang="javascript" class="my-editor" v-model="content.answer" :highlight="highlighter" line-numbers></prism-editor>
        <v-btn class="primary" v-if="content.type === 'javascript'" @click="() => {
          vm.runInNewContext(content.answer)
        }">اجرای دستور</v-btn>
        <h3 v-if="content.type === 'simple_question'">{{content.answer}}</h3>
      </v-container>
    </v-card>
  </v-container>
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
  methods: {
    highlighter(code) {
      return highlight(code, languages.js); // languages.<insert language> to return html with markup
    },
    downloadObjectAsJson(exportObj, exportName){
      var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(exportObj));
      var downloadAnchorNode = document.createElement('a');
      downloadAnchorNode.setAttribute("href",     dataStr);
      downloadAnchorNode.setAttribute("download", exportName + ".json");
      document.body.appendChild(downloadAnchorNode); // required for firefox
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    }
  },
  name: "collections",
  data() {
    return ({
      vm: require('vm'),
      collections: null,
      doc_names: null,
      selectedCollection: null
    })
  },
  mounted() {
    this.$axios.get("/get/data/").then(resp => {
      this.collections = resp.data;
      this.doc_names = Object.keys(this.collections);
    })
  }
}
</script>

<style scoped>

</style>

<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
        <v-img :src="require('~/static/index.jpg')" max-width="300"/>
      </v-card>
      <v-card>
        <v-card-title class="headline">
          به نرم افزار دستیار هوشمند خوش آمدید
        </v-card-title>
        <v-card-text>
          <p>در این برنامه شما می توانید سوالات و جواب های خود را به کمک دستیار هوشمند صوتی ذخیره کنید</p>
          <p>با ذخیره سوالات و جواب ها می توانید سوالات جدید را به سیستم داده تا به شما پاسخ دهند یا به صورت یک فایل json دریافت کنید.</p>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            nuxt
            to="/new_qa"
          >
            طرح سوال و جواب
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  auth: false,
  name: 'IndexPage'
}
</script>

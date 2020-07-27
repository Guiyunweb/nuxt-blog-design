<template>
  <div>
    <h1>友链</h1>
    <div style="margin: 35px" class="text-body-1">
      申请友链请点击
      <v-btn @click="dialog = true" small color="primary">按钮</v-btn>
      申请添加友链
      <br>
    </div>
    <h2>我的网站信息</h2>
    <div style="margin: 20px;font-weight: bold">
      博客名称： 归云的个人网站<br>
      博客链接： https://guiun.com<br>
      博客头像： https://guiun.com/favicon.ico<br>
      博客描述： 归云的个人小站<br>
    </div>
    <h2>我的邻居们</h2>

    <v-row>
      <v-col cols="3" v-for="item in list" :key="item.id">
        <v-card dark v-ripple exact height="310">
          <v-img :src="item.cover" height="200px">
          </v-img>
          <v-card-title>
            <a :href="item.link" target="_blank">
              {{item.name}}
            </a>
          </v-card-title>
          <v-card-subtitle>
            {{item.describe}}
          </v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>

    <!-- 弹出框 -->
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title class="headline grey lighten-2">
          <span class="headline">添加友链</span>
        </v-card-title>
        <v-card-text>
          <v-col cols="12">
            <v-text-field v-model="links.name" label="友链名称" required></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-text-field v-model="links.link" label="网站链接" required></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-text-field v-model="links.cover" label="头像链接" required></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-text-field v-model="links.describe" label="简介" required></v-text-field>
          </v-col>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false">关闭</v-btn>
          <v-btn color="blue darken-1" text @click="addLinks()">添加</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-snackbar v-model="snackbar" color="success" top="true">
      添加友链成功
      <template v-slot:action="{ attrs }">
        <v-btn color="teal lighten-5" text v-bind="attrs" @click="snackbar = false">
          关闭
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
  import api from "../../api"

  export default {
    name: "index",
    data: () => ({
      dialog: false,
      links: {},
      snackbar: false,
      list: []
    }),
    asyncData({params}) {
      return api.SHOW_LINK().then(res => {
        return {list: res.data}
      })
    },
    created() {
      this.getList()
    },
    methods: {
      getList() {
        api.SHOW_LINK().then(res => {
          console.log(res)
          this.list = res.data
        })
      },
      addLinks() {
        this.links.show = false
        api.ADD_LINK(this.links).then(() => {
          this.dialog = false
          this.snackbar = true
          this.links = {}
        })
      }
    }
  }
</script>

<style scoped>

</style>

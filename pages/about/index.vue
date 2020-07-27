<template>
  <div>
    <h1>关于</h1>
    <br/>
    <div style="margin: 20px">
      <div class="text-body-2">
        <h2>关于我</h2>
        <p>95后,Java程序员,现居兰州,爱好动漫,
          B站重度依赖者</p>
        <p>
          自己是一个选择困难症,常常因为技术选型而纠结
        </p>
      </div>
      <div class="text-body-2">
        <h2>关于博客</h2>
        <p>
          基于前端Nuxt.js开发,后台使用的是spring boot框架
        </p>
        <p>
          自己学习前端也不久,可能样式和性能不是特别好,还请多多包含
        </p>
        <br>
      </div>
      <v-chip class="ma-2">
        评论
      </v-chip>
      <v-chip class="ma-2">
        关于我
      </v-chip>
      <h2 style="margin: 20px 0">评论</h2>
      <article-comment articleId="about" father=""></article-comment>
      <h2 style="margin: 20px 0">评论列表</h2>

      <v-list-item three-line v-for="(item,index) in commentList" :key="item.id">
        <v-list-item-avatar>
          <v-img :src="'https://sdn.geekzu.org/avatar/'+getMD5(item.email)"></v-img>
        </v-list-item-avatar>
        <v-list-item-content>
          <v-list-item-title>
            <h3>{{item.username}}&nbsp;<span style="font-size: 10px">{{timeFromNow(item.createDate)}}</span></h3>
            <br>
            {{getMD5()}}
          </v-list-item-title>
          <v-list-item-subtitle>
            {{item.comment}}
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </div>
  </div>
</template>
<script>
  import ArticleComment from "~/components/ArticleComment/index";
  import api from "~/api";
  import moment from "moment";

  export default {
    name: "index",
    components: {ArticleComment},
    data: () => ({
      article: {},
      comment: {},
      commentList: [],
      commentIndex: [],
      commentBoole: false
    }),
    created() {
      this.getList("about")
    },
    methods: {
      getList(id) {
        api.SHOW_COMMENT_LIST({id}).then(res => {
          console.log(res)
          this.commentList = res.data
        })
      },
      getMD5(email) {
        if (email) {
          let md5 = require('md5');
          return md5(email)
        } else {
          return ""
        }
      },
      timeFromNow(time) {
        const format = "YYYY-MM-DD HH:mm:ss";
        const formatDate = "YYYY-MM-DD";
        const formatTime = "HH:mm:ss";
        let timeStr = moment(time).format(format);
        if (moment(time).format(formatDate) === moment().format(formatDate)) {
          const fromNowStr = moment(time).fromNow(true);
          if (fromNowStr.indexOf("小时") > 0 && parseInt(fromNowStr) > 5) {
            timeStr = "今天 " + moment(time).format(formatTime);
          } else {
            timeStr = fromNowStr
          }
        }
        return timeStr
      }
    }
  }
</script>
<style scoped>
  p {
    font-size: 20px;
    margin: 30px;
  }
</style>

<template>
  <div style="margin: 30px">
    <h1>{{article.title}}</h1>
    <h5 style="margin: 20px 0">发布创建于&nbsp;{{article.createTime}}</h5>
    <hr class="design-hr"/>
    <article-content :content="article.content"></article-content>
    <v-chip class="ma-2" v-for="item in article.tags" :key="item">
      {{item}}
    </v-chip>
    <hr class="design-hr"/>
    <h2 style="margin: 20px 0">评论</h2>
    <article-comment :articleId="article.id" father=""></article-comment>
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
<!--          <article-comment v-if="commentBoole && index===commentIndex" style="margin: 20px 0" :articleId="article.id" father=""></article-comment>-->
        </v-list-item-subtitle>
      </v-list-item-content>
<!--      <v-list-item-action>-->
<!--        <v-btn text @click="addChildList(index)" color="primary">回复</v-btn>-->
<!--      </v-list-item-action>-->
    </v-list-item>
  </div>
</template>

<script>
  import api from '../../api'
  import moment from 'moment';
  import ArticleContent from "~/components/ArticleContent/index";
  import ArticleComment from "~/components/ArticleComment/index";

  export default {
    name: "index",
    components: {ArticleComment, ArticleContent},
    data: () => ({
      article: {},
      comment: {},
      commentList: [],
      commentIndex: [],
      commentBoole: false
    }),
    head() {
      return {
        title: this.article.title,
        meta: [
          {hid: 'description', name: 'description', content: this.article.summary}
        ]
      }
    },
    asyncData({params}) {
      let id = params.id
      return api.GET_ARTICLE_INFO({id}).then(res => {
        return {article: res.data}
      })
    },
    created() {
      this.getData(this.$route.params.id)
      this.getList(this.$route.params.id)
    },
    methods: {
      getData(id) {
        api.GET_ARTICLE_INFO({id}).then(res => {
          this.article = res.data
        })
      },
      getList(id) {
        api.SHOW_COMMENT_LIST({id}).then(res => {
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
      addChildList(index){
        this.commentIndex = index
        this.commentBoole = true
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
  .design-hr {
    border: 1px dashed #f0f0f0;
    margin: 20px 0;
  }
</style>

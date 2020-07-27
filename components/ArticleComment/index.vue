<template>
  <div>
    <v-form style="border-style:dashed;">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field v-model="comment.username" label="昵称" required>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field v-model="comment.email" label="邮箱" required>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field v-model="comment.site" label="网址(http://)" required>
            </v-text-field>
          </v-col>
          <v-col cols="12">
            <v-textarea
              outlined
              v-model="comment.comment"
              name="input-7-4"
              label="评论"
            ></v-textarea>
          </v-col>
          <v-col cols="12">
            <v-btn style="float: right" @click="addComment" color="primary">评论</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>

<script>
  import api from "~/api"
  import article from "~/api/modules/article";

  export default {
    name: "ArticleComment",
    props: {
      articleId: {
        type: String
      },
      father: {
        type: String
      }
    },
    data: () => ({
      comment: {}
    }),
    methods: {
      addComment() {
        this.comment.articleId = this.articleId
        this.comment.createDate = new Date()
        api.ADD_COMMENT(this.comment).then(() => {
          this.comment = {}
          this.$parent.getList(this.articleId)
        })
      }
    }
  }
</script>

<style scoped>

</style>

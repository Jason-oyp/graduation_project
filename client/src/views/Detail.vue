<template>
  <div
    id="detail-container"
    v-loading="loading"
    element-loading-text="拼命加载中"
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.5)"
  >
    <Nav />
    <div class="content-detail">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>

        <el-breadcrumb-item
          :to="{path:'/articlelist/'+$route.params.about,query:{page:1,limit:10}}"
        >{{about}}</el-breadcrumb-item>
        <el-breadcrumb-item>文章详情</el-breadcrumb-item>
      </el-breadcrumb>
      <div class="detail">
        <div class="title-box">
          <h2>{{content.title}}</h2>
          <span class="news-time">{{new Date(content.ctime).toLocaleDateString()}}</span>
          <span class="news-from">{{content.from}}</span>
        </div>
        <div class="content" v-html="content.content"></div>
      </div>
    </div>
    <Comment :commentData="commentData" :articleId="content.id" :getDatas="getDatas" />
    <Footer />
    <el-backtop target=".body" :bottom="100">
      <div
        style="{
        height: 100%;
        width: 100%;
        background-color: #f2f5f6;
        box-shadow: 0 0 6px rgba(0,0,0, .12);
        text-align: center;
        line-height: 40px;
        color: #1989fa;
        border-radius:50%;
        font-size:12px;
      }"
      >顶部</div>
    </el-backtop>
  </div>
</template>

<script>
import Nav from "@/components/Nav";
import Footer from "@/components/Footer";
import Comment from "@/components/comment";

import { queryArticleDetailById } from "@/api/articles/articles";
import { queryCommentByArticleId } from "@/api/comments/comments";
export default {
  components: {
    Nav,
    Footer,
    Comment
  },
  data() {
    return {
      about: "",
      content: "",
      loading: false,
      commentData: []
    };
  },
  async mounted() {
    this.loading = true;
    if (this.$route.params.about === "jyxd") {
      this.about = "经验心得";
    } else if (this.$route.params.about === "fszd") {
      this.about = "复试备考";
    } else if (this.$route.params.about === "english") {
      this.about = "英语";
    } else if (this.$route.params.about === "math") {
      this.about = "数学";
    } else if (this.$route.params.about === "politics") {
      this.about = "政治";
    } else if (this.$route.params.about === "profession") {
      this.about = "专业课";
    }
    this.getDatas();
  },
  methods: {
    async getDatas() {
      const result = await queryArticleDetailById(this.$route.query.id);
      this.commentData = await queryCommentByArticleId(result.id);
      this.content = result;
      this.loading = false;
    }
  }
};
</script>

<style lang="less">
.content-detail {
  margin: auto;
  padding-top: 20px;
  width: 80%;
  .detail {
    margin-bottom: 20px;
  }
  .title-box {
    position: relative;
    padding-bottom: 10px;
    border-bottom: 1px solid #ddd;
    margin: 20px 0;
    span {
      color: #999;
    }
  }
  p {
    line-height: 30px;
  }
}
</style>
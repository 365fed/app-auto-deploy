<template>
  <div>
    <!-- banner -->
    <div class="tag-banner banner">
      <h1 class="banner-title">文章标签</h1>
    </div>
    <!-- 标签列表 -->
    <v-card class="blog-container">
      <div class="tag-cloud-title">标签（共{{ count }}条）</div>
      <div class="tag-cloud">
        <router-link
          :style="{ 'font-size': Math.floor(Math.random() * 10) + 18 + 'px' }"
          v-for="item of tagList"
          :key="item.id"
          :to="'/tags/' + item.id"
        >
          {{ item.tagName }}
        </router-link>
      </div>
    </v-card>
  </div>
</template>

<script>
export default {
  created() {
    this.listTags();
  },
  data: function() {
    return {
      tagList: [],
      count: 0
    };
  },
  methods: {
    listTags() {
      this.axios.get("/api/tags").then(({ data }) => {
        this.tagList = data.data.recordList;
        this.count = data.data.count;
      });
    }
  }
};
</script>

<style scoped>
.tag-banner {
  background: url(http://ipoodle.gitee.io/cdn/blog/banner7.jpg) center center /
    cover no-repeat;
  background-color: #49b1f5;
}
.tag-cloud-title {
  line-height: 2;
  font-size: 36px;
  text-align: center;
}
@media (max-width: 759px) {
  .tag-cloud-title {
    font-size: 25px;
  }
}
.tag-cloud {
  text-align: center;
}
.tag-cloud a {
  color: #616161;
  display: inline-block;
  text-decoration: none;
  padding: 0 8px;
  line-height: 2;
  transition: all 0.3s;
}
.tag-cloud a:hover {
  color: #03a9f4 !important;
  transform: scale(1.1);
}
</style>

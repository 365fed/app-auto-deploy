<template>
  <div>
    <!-- banner -->
    <div class="message-banner">
      <!-- 弹幕输入框 -->
      <div class="message-container">
        <h1 class="message-title">留言板</h1>
        <div class="animated fadeInUp message-input-wrapper">
          <input
            v-model="messageContent"
            @click="show = true"
            placeholder="说点什么吧"
          />
          <button
            class="ml-3 animated bounceInLeft"
            @click="addToList"
            v-show="show"
          >
            发送
          </button>
        </div>
      </div>
      <!-- 弹幕列表 -->
      <div class="barrage-container">
        <vue-baberrage :barrageList="barrageList" :loop="true">
          <template v-slot:default="slotProps">
            <span class="barrage-items">
              <img
                :src="slotProps.item.avatar"
                width="30"
                height="30"
                style="border-radius:50%"
              />
              <span class="ml-2">{{ slotProps.item.nickname }} :</span>
              <span class="ml-2">{{ slotProps.item.messageContent }}</span>
            </span>
          </template>
        </vue-baberrage>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    this.listMessage();
  },
  data() {
    return {
      show: false,
      messageContent: "",
      barrageList: [],
    };
  },
  methods: {
    addToList() {
      if (this.messageContent.trim() == "") {
        this.$toast({ type: "error", message: "留言不能为空" });
        return false;
      }
      const userAvatar = this.$store.state.avatar
        ? this.$store.state.avatar
        : "http://ipoodle.gitee.io/cdn/blog/user.jpg";
      const userNickname = this.$store.state.nickname
        ? this.$store.state.nickname
        : "游客";
      var message = {
        avatar: userAvatar,
        nickname: userNickname,
        messageContent: this.messageContent,
        time: Math.floor(Math.random() * 40 + 3)
      };
      this.barrageList.push(message);
      this.messageContent = "";
      this.axios.post("/api/messages", message);
    },
    listMessage() {
      this.axios.get("/api/messages").then(({ data }) => {
        if (data.flag) {
          this.barrageList = data.data;
          console.log(data,'-----data.data-----')
          setTimeout(() => {
            console.log(this.barrageList,'-----barrageList-----');    
          }, 2000);
        }
      });
    }
  }
};
</script>

<style scoped>
.message-banner {
  position: absolute;
  top: -58px;
  left: 0;
  right: 0;
  height: 100vh;
  background: url(http://ipoodle.gitee.io/cdn/blog/banner1.jpg) center center /
    cover no-repeat;
  background-color: #49b1f5;
  animation: header-effect 1s;
}
.message-title {
  color: #eee;
  animation: title-scale 1s;
}
.message-container {
  position: absolute;
  width: 360px;
  top: 35%;
  left: 0;
  right: 0;
  text-align: center;
  z-index: 5;
  margin: 0 auto;
  color: #fff;
}
.message-input-wrapper {
  display: flex;
  justify-content: center;
  height: 2.5rem;
  margin-top: 2rem;
}
.message-input-wrapper input {
  outline: none;
  width: 70%;
  border-radius: 20px;
  height: 100%;
  padding: 0 1.25rem;
  color: #eee;
  border: #fff 1px solid;
}
.message-input-wrapper input::-webkit-input-placeholder {
  color: #eeee;
}
.message-input-wrapper button {
  outline: none;
  border-radius: 20px;
  height: 100%;
  padding: 0 1.25rem;
  border: #fff 1px solid;
}
.barrage-container {
  position: absolute;
  top: 50px;
  left: 0;
  right: 0;
  bottom: 0;
  height: calc(100% -50px);
  width: 100%;
}
.barrage-items {
  background: rgb(0, 0, 0, 0.7);
  border-radius: 100px;
  color: #fff;
  padding: 5px 10px 5px 5px;
  align-items: center;
  display: flex;
}
</style>

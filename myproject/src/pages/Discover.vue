<template>
  <el-container>
    <el-aside width="200px">
      <el-menu :default-active="active" @select="handleSelect">
        <el-menu-item :index="item.path" v-for="item in cat" :key="item.name" @click="goto(item)">
          <i :class="item.icon"></i>
          <span slot="title">{{item.title}}</span>
        </el-menu-item>
      </el-menu>
    </el-aside>
    <el-container>
      <el-header>首页 > 发现 > 手机</el-header>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      cat: [
        {
          title: "手机",
          name: "Phone",
          icon: "el-icon-mobile-phone"
        },
        {
          title: "电脑",
          name: "Computer",
          icon: "el-icon-s-platform"
        },
        {
          title: "平板",
          name: "Tablet",
          icon: "el-icon-reading"
        },
        {
          title: "配件",
          name: "Acc",
          icon: "el-icon-bangzhu"
        }
      ],
      active: "/discover/phone"
    };
  },
  methods: {
    handleSelect(index, indexPath) {
      console.log("index:", index);
      this.active = index;
    },
    goto(item) {
      console.log("name:", item.path);
      this.$router.push({ name: item.name });
    }
  },
  //   beforeRouterUpdate() {
  //     next();
  //   },
  created() {
    console.log("this:", this);
    // 刷新保持高亮效果
    let hash = window.location.hash.slice(1);
    console.log("hash:", hash);
    this.active = hash;

    console.log("$route:", this.$route);
    let baseUrl = this.$router.path;
    this.cat = this.cat.map(item => {
      return {
        ...item,
        path: baseUrl + "/" + item.name.toLowerCase()
      };
    });
    if (this.$route.params.category) {
      this.cat = this.$route.params.category;
    }

    this.$axios.get("/goods").then(res => {
      console.log(res);
    });
  }
};
</script>

<style>
</style>

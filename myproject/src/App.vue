<template>
  <div class="app">
    <!-- <nav>
      <ul>
        <li v-for="item in pages" :key="item.name">
          <router-link :to="item.path" active-class="active" tag="span">{{item.title}}</router-link>
        </li>
      </ul>
    </nav>-->
    <el-row>
      <el-col :span="18">
        <el-menu
          :default-active="active"
          class="el-menu-demo"
          mode="horizontal"
          active-text-color="#58bc58"
          @select="handleSelect"
          router
        >
          <el-menu-item :index="item.path" v-for="item in pages" :key="item.name">{{item.title}}</el-menu-item>
        </el-menu>
      </el-col>
      <el-col :span="6" class="login" v-if="!logined">
        <el-link href="#" @click.native.prevent.stop="goto('Reg')">注册</el-link>
        <el-link href="#" @click.native.prevent.stop="goto('Login')">登录</el-link>
      </el-col>
      <el-col :span="6" class="login" v-else>
        <el-link href="#" @click.native.prevent.stop="logout()">退出</el-link>
      </el-col>
    </el-row>

    <!-- 路由配置匹配后，渲染对应的组件到router-view -->
    <router-view />
    <router-view name="bottom" />
  </div>
</template>

<script>
import Vue from "vue";

// 完整引入：引入了ElementUI下所有的组件和样式
// 会造成性能问题
// import ElementUI from "element-ui";
// import "element-ui/lib/theme-chalk/index.css";
// Vue.use(ElementUI);

//按需加载
// import { Menu, MenuItem } from "element-ui";
// import "element-ui/lib/theme-chalk/menu.css";
// import "element-ui/lib/theme-chalk/menu-item.css";

// 基于babel-plugin-component的按需加载
import {
  Menu,
  MenuItem,
  Row,
  Col,
  Link,
  Container,
  Aside,
  Main,
  Header,
  Form,
  FormItem,
  Input,
  Button
} from "element-ui";
Vue.use(Menu);
Vue.use(MenuItem);
Vue.use(Row);
Vue.use(Col);
Vue.use(Link);
Vue.use(Container);
Vue.use(Aside);
Vue.use(Main);
Vue.use(Header);
Vue.use(Form);
Vue.use(FormItem);
Vue.use(Input);
Vue.use(Button);

export default {
  name: "app",
  data() {
    return {
      pages: [
        {
          title: "首页",
          path: "/home",
          name: "Home"
        },
        {
          title: "发现",
          path: "/discover",
          name: "Discover"
        },
        {
          title: "购物车",
          path: "/cart",
          name: "Cart"
        },
        {
          title: "我的",
          path: "/mine",
          name: "Mine"
        }
      ],
      active: "/home",
      logined: false
    };
  },
  methods: {
    handleSelect(index, indexPath) {
      console.log(index, indexPath);
      this.active = index;
    },
    goto(name) {
      // .push({name,query: { username: "xiaobai", psw: "123456" }})  .replace({name})  都可以传参数
      this.$router.push({
        name,
        // path: "/" + namme.toLowerCase(),
        // query: { username: "xiaobai", psw: "123456" },
        params: { a: 100, b: 200 } //params只能通过name方式跳转时传参
      }); //.push() 有浏览记录
      //   this.$router.replace({
      //     name,
      //     query: { username: "xiaobai", psw: "123456" }
      //   }); //.replace() 无浏览记录
    },
    logout() {
      this.logined = false;
      localStorage.removeItem("Authorization");
    }
  },
  created() {
    //   刷新保持高亮效果
    let hash = window.location.hash.slice(1);
    this.active = hash;

    let token = localStorage.getItem("Authorization");
    this.logined = token ? true : false;
  }
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}
.active {
  font-weight: bold;
  color: pink;
}
.login {
  text-align: right;
  line-height: 58px;
  background: #fff;
  border-bottom: 1px solid #e6e6e6;
  a {
    margin-right: 10px;
  }
}
</style>

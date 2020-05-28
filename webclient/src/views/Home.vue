<template>
  <div class="home">
    
    <div class="layout">
      <SideBar 
        :openNames="openNames" 
        :activeIndex="activeIndex" 
        >
      </SideBar>
      
      <Layout :style="{marginLeft: '200px'}">
        <Header :style="{background: '#fff', boxShadow: '0 2px 3px 2px rgba(0,0,0,.1)'}">
          <div class="demo-avatar">
              <!-- <Avatar icon="ios-person" />
              <Avatar>U</Avatar>
              <Avatar>USER</Avatar>
              <Avatar src="https://i.loli.net/2017/08/21/599a521472424.jpg" />
              <Avatar style="color: #f56a00;background-color: #fde3cf">U</Avatar> -->
              <Avatar style="background-color: #87d068" icon="ios-person" />
              <router-link v-if="!isLogin" to="Login">Login</router-link>
              <span v-else>{{userName}}</span>
              
              <Dropdown trigger="click" style="margin-left: 20px">
                  <a href="javascript:void(0)">
                    <Icon class="icon-more" type="md-more" size="26" />
                  </a>
                  <DropdownMenu slot="list">
                      <DropdownItem ><div @click="handleLogout">logout</div></DropdownItem>
                  </DropdownMenu>
              </Dropdown>
          </div>
        </Header>
        <Content :style="{padding: '0 16px 16px'}">
          <Breadcrumb :style="{margin: '16px 0'}">
            <BreadcrumbItem v-if="fisrtBread">{{fisrtBread}}</BreadcrumbItem>
            <BreadcrumbItem v-if="secondBread">{{secondBread}}</BreadcrumbItem>
          </Breadcrumb>
          <Card  style="min-height: 600px; ">
            <!-- 当 `currentView` 改变时，组件也跟着改变 -->
            <component v-bind:is="currentView"></component>
          </Card>
        </Content>
      </Layout>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import SideBar from '@/components/sidebar.vue'
import NEReboot from './NE-Reboot.vue'
import DBRestore from './DB-Restore.vue'
import DBStart from './DB-Start.vue'
import Form from './form.vue'
import { getCookie, delCookie } from '../common/tools'


export default {
  name: 'Home',
  data(){
    return{
      fisrtBread: "Home",
      secondBread: "",
      currentView: "NEReboot",
      activeIndex: '5-1',
      openNames: ['5'],

      userName: "",
    }
  },
  created() {
    this.init()
  },
  mounted() {
    // this.openNames = ['5'];
    // this.activeIndex = '5-1';
    
  },
  methods:{
    init(){
      this.changeBread(this.$route)
      this.isLogin = getCookie("userMsg");
      this.userName = JSON.parse(unescape(getCookie("userMsg"))).userName;
      console.log(this.isLogin);
      console.log(this.userName);
    },
    // updateSidebar(){
    //   this.openNames
    // },
    changeBread(to){
      if(to.query.childNode){
        this.secondBread = to.query.childNode;
        this.changeView(this.secondBread);
      }
    },
    changeView(secondBread){
      console.log("changeView: "+ secondBread);
      
      this.currentView = secondBread
    },
    handleLogout(){
      console.log("handleLogout");
      delCookie("userMsg");
      this.$router.push('/login')
    }
  },
  watch:{   //监听路由变化
    $route( to , from ){
      // console.log( to , from )
        // to , from 分别表示从哪跳转到哪，都是一个对象
        // to.path  ( 表示的是要跳转到的路由的地址 eg: /home )
        this.changeBread(to, from);
    },
  },
  components: {
    // HelloWorld,
    SideBar,
    NEReboot,
    DBRestore,
    DBStart,
    Form
  }
}
</script>

<style lang="scss">
.demo-avatar{
  float: right;
}
.icon-more{
  vertical-align: -0.225em!important;
}
</style>
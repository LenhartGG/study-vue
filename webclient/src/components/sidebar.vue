<template>
    <Sider :style="{position: 'fixed', height: '100vh', left: 0, overflow: 'auto'}">
      <!-- <Menu  ref="menus"  theme="dark" width="auto"
      :active-name="$route.query.activeIndex"
      :open-names="[$route.query.openNames]"
      @on-select="onSelect" 
      @on-open-change="onOpenChange">
        <Submenu v-for="(item, index) in sidebarData" :key="index" :name="index+1">
          <template slot="title">
            <span class="item-text">{{item.title}}</span>
          </template>
          <MenuItem :to="index+1+'-'+1" :name="index+1+'-'+1">Option 1</MenuItem>
          <MenuItem :to="index+1+'-'+2" :name="index+1+'-'+2">Option 2</MenuItem>
          <MenuItem :to="index+1+'-'+3" :name="index+1+'-'+3">Option 3</MenuItem>
        </Submenu>
        <MenuItem name="1-1">Option 1</MenuItem>
        <Submenu name="1">
          <template slot="title">
            <span class="item-text">Log Collector</span>
          </template>
          <MenuItem name="1-1">Option 1</MenuItem>
          <MenuItem name="1-2">Option 2</MenuItem>
          <Submenu name="1-3">
            <template slot="title">
              <span class="item-text">Option 3</span>
            </template>
            <MenuItem name="1-3-1">Item 1</MenuItem>
            <MenuItem name="1-3-2">Item 2</MenuItem>
            <MenuItem name="1-3-3">Item 3</MenuItem>
          </Submenu>
        </Submenu>
        <Submenu name="2">
          <template slot="title">
            <span class="item-text">Network Monitor</span>
          </template>
          <MenuItem name="2-1">Option 1</MenuItem>
          <MenuItem name="2-2">Option 2</MenuItem>
        </Submenu>
        <Submenu name="3">
          <template slot="title">
            <span class="item-text">Visualization file configuration</span>
          </template>
          <MenuItem name="3-1">Option 1</MenuItem>
          <MenuItem name="3-2">Option 2</MenuItem>
        </Submenu>
        <Submenu name="4">
          <template slot="title">
            <span class="item-text">CLI Quick Verification</span>
          </template>
          <MenuItem name="4-1">Option 1</MenuItem>
          <MenuItem name="4-2">Option 2</MenuItem>
          <MenuItem name="4-3" :to='{path: "/home", query: {childNode: "NE-Reboot", openNames: "4", activeIndex: "4-3"}}'>NE Reboot</MenuItem>
        </Submenu>
        <Submenu name="5">
          <template slot="title">
            <span class="item-text">Common Operation</span>
          </template>
          <MenuItem name="5-1" :to='{path: "/home", query: {childNode: "NE-Reboot", openNames: "5", activeIndex: "5-1"}}'>Operation</MenuItem>
          <MenuItem name="5-2" :to='{path: "/home", query: {childNode: "DB-Restore", openNames: "5", activeIndex: "5-2"}}'>DB Restore</MenuItem>
          <MenuItem name="5-3" :to='{path: "/home", query: {childNode: "DB-Start", openNames: "5", activeIndex: "5-3"}}'>DB Start</MenuItem>
          <MenuItem name="5-4" :to='{path: "/home", query: {childNode: "Form", openNames: "5", activeIndex: "5-4"}}'>Form</MenuItem>
        </Submenu>
      </Menu> -->

      
      <Menu  ref="menus"  theme="dark" width="auto"
      :active-name="$route.query.activeIndex"
      :open-names="[$route.query.openNames]"
      @on-select="onSelect" 
      @on-open-change="onOpenChange">
        <Submenu v-for="(item, index) in sidebarData" :key="index" :name="index+1">
          <template slot="title">
            <span class="item-text">{{item.title}}</span>
          </template>
          <MenuItem :to="index+1+'-'+1" :name="index+1+'-'+1">Option 1</MenuItem>
          <MenuItem :to="index+1+'-'+2" :name="index+1+'-'+2">Option 2</MenuItem>
          <MenuItem :to="index+1+'-'+3" :name="index+1+'-'+3">Option 3</MenuItem>
        </Submenu>
        <MenuItem name="1">Log Collector</MenuItem>
        <MenuItem name="2">Network Monitor</MenuItem>
        <MenuItem name="3"><span class="item-text">Visualization file configuration</span></MenuItem>
        <MenuItem name="4"><span class="item-text">CLI Quick Verification</span></MenuItem>
        <Submenu name="5">
          <template slot="title">
            <span class="item-text">Common Operation</span>
          </template>
          <!-- <MenuItem name="5-1" :to='{path: "NE-Reboot", query: {openNames: ["5"], activeIndex: "5-1"}}'>NE Reboot</MenuItem>
          <MenuItem name="5-2" :to='{path: "DB-Restore", query: {openNames: ["5"], activeIndex: "5-2"}}'>DB Restore</MenuItem> -->
          <MenuItem name="5-1" :to='{path: "/home", query: {childNode: "NE-Reboot", openNames: "5", activeIndex: "5-1"}}'>Common Operation</MenuItem>
          <!-- <MenuItem name="5-2" :to='{path: "/home", query: {childNode: "DB-Restore", openNames: "5", activeIndex: "5-2"}}'>DB Restore</MenuItem>
          <MenuItem name="5-3" :to='{path: "/home", query: {childNode: "DB-Start", openNames: "5", activeIndex: "5-3"}}'>DB Start</MenuItem>
          <MenuItem name="5-4" :to='{path: "/home", query: {childNode: "Form", openNames: "5", activeIndex: "5-4"}}'>Form</MenuItem> -->
        </Submenu>
      </Menu>

    </Sider>
</template>

<script>
export default {
    props:[
      // 'openNames', 'activeIndex'
    ],
    data(){
        return {
          sidebarData: [
          ],
          activeIndex: '5-1',
          openNames: ['5']
        }
    },
    created(){
      
      // console.log(this.$route.query.openNames);
      
    },
    mounted(){
    },
    methods:{
      onSelect(select){
        // console.log(select);
        
      },
      onOpenChange(list) {
        // console.log(list)
      },
    },
    computed:{
      newOpenNames(){
        // if(typeof this.$route.query.openNames == 'number'){
        //   // return [this.$route.query.openNames.toString];
        //   return ['5']
        // }
        return this.$route.query.openNames;
      }
    },
    watch:{   //监听路由变化
      $route( to , from ){   
        // console.log( to , from )
          // to , from 分别表示从哪跳转到哪，都是一个对象
          // to.path  ( 表示的是要跳转到的路由的地址 eg: /home );
      },
    }
}
</script>
<style >

/* .item-icon{
    font-size: 20px;
    vertical-align: middle;
}
.item-text{
    display: inline-block;
    width: 120px;
    height: 100%;
    
} */
</style>
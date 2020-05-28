<template>
  <div class style="margin:0 auto; ">
    <!-- content -->
    <!-- <p>当前正在进行第 {{ current + 1 }} 步</p> -->
    <Steps :current="currentStep">
      <Step title="Choose NE-list"></Step>
      <Step title="Operation"></Step>
      <Step title="Confirm"></Step>
    </Steps>

    <!-- step0 Choose NE-list -->
    <div v-if="currentStep==0" style="margin-top: 30px;">
      <p>Please choose the network:</p>
      <Table border 
        :columns="neListColumns" 
        :data="currentNeList" 
        @on-selection-change="onSelectionChange"
        @on-select="onSelect"
      >
      </Table>
      <div style="margin: 10px;overflow: hidden">
          <Button style="float: right; margin-left: 50px;" type="primary" @click="createGroup">Group</Button>

          <div style="float: right;">
              <Page :total="10" :page-size="5" :current="currentPage" show-elevator @on-change="changePage"></Page>
          </div>
      </div>

      <div class="clearfix" style="margin-top: 40px;">
        
        <Card v-for="(group, groupKey) in groupList" :key="groupKey" style="float: left ; width:26%; margin-top: 20px; margin-left: 3%; margin-right: 3%;">
            <p slot="title" style="color: #1890FF;">
              {{group.title}}
            </p>
            <span slot="extra" style="color: #9B9B9B; fontSize:14px">
                <!-- <Icon type="ios-loop-strong"></Icon> -->
                05/11-12:16更新
            </span>

            <Tag v-for="(neItem, neIndex) in group.neArray" :key="neIndex">{{neItem}}</Tag>
            
            <ul v-if="true" class="clearfix group-card">
              <li><img src="@/assets/images/icon-delete.png" alt="delete"></li>
              <li><img src="@/assets/images/icon-edit.png" alt="edit"></li>
              <li><img src="@/assets/images/icon-add.png" alt="add"></li>
            </ul>
        </Card>

      </div>
    </div>

    <!-- step1 operation -->
    <div v-if="currentStep==1" style="margin-top: 30px; ">
      <div>
        <p style="color:rgba(24,144,255,1);">
          <span style="color:rgba(24,144,255,1); font-size:24px;">Operation</span> &nbsp;
          <span style="font-size:14px;">Drag to the table</span> 
        </p>
        <ul class="tools clearfix">
          <li v-for="(item, index) in operationList" :key="index"
            draggable="true"
            @dragstart="dragStart($event, index)"
            style="cursor: move;">
            <!-- <img src="@/assets/images/operation-neporv.png"> -->
            <img :src="item.src">
            <p>{{item.text}}</p>
          </li>
        </ul>
      </div>

      <div style="margin-top: 50px; position: relative;" @dragover.prevent="allowDrop($event)" @drop="drop($event)">
        <Table border :columns="operationColumns" 
        :data="data1" >
          <template slot-scope="{ row, index }" slot="group">
            <Select v-model="row.group"
              @on-change="selected => selectChangeGroup(selected, index)">
                <Option v-for="group in groupList" :value="group.title" :key="group.title"
                  >
                  {{ group.title }}
                </Option>
            </Select>
          </template>
          <template slot-scope="{ row, index }" slot="loop">
            <InputNumber :max="10" :min="0" v-model="data1[index].loop"></InputNumber>
          </template>
          <template slot-scope="{ row, index }" slot="trafficmonitor">
            <Checkbox v-model="data1[index].trafficmonitor">{{data1[index].trafficmonitor}}</Checkbox>
          </template>
          <template slot-scope="{ row, index }" slot="action">
            <Button
              type="primary"
              size="small"
              style="margin-right: 5px"
              @click="showView(index)"
            >View</Button>
            <Button type="error" size="small" @click="remove(index)">Delete</Button>
          </template>
        </Table>
        <div v-if="showRedDrag" :class="{'red-border': true, 'margin-top-0px': isMargintop0}">
          Drag to the table
        </div>
      </div>
    </div>

    <!-- step2 Confirm -->
    <div v-if="currentStep==2" style="margin-top: 30px;">
      <h3>Group list:</h3>
      <Table border 
        :columns="groupListColumns" 
        :data="selectedNelist" width="auto"
        v-merge-cell="{cell, groupListColumns, selectedNelist}"
      ></Table>
      <h3>Group Operation:</h3>
      <Table border :columns="columns1" :data="data1" width="auto"></Table>
    </div>

    <!-- button -->
    <div class="clearfix">
      <div style="float:right; margin-top: 60px; height: 80px;">
        <Button v-if="currentStep!=0" type="primary" style="margin-right:50px" @click="back">Back</Button>
        <Button v-if="currentStep!=2" type="primary" @click="next">Next</Button>
        <Button v-if="currentStep==2" type="primary" @click="confirm">Confirm</Button>
      </div>
    </div>

    <Alert v-if="showAlert">
      An info prompt
      <template
        slot="desc"
      >Content of prompt. Content of prompt. Content of prompt. Content of prompt.</template>
    </Alert>
    <!-- content/ -->
  </div>
</template>

<script>
// import HelloWorld from '@/components/HelloWorld.vue'
import MergeCell from "ivu-table-merge";

export default {
  props: {},
  data() {
    return {
      currentStep: 0,

      neList: [
        {
          NeName: "Setup1",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup2",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup3",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup4",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup5",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup6",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup7",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup8",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup9",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
        {
          NeName: "Setup10",
          NeType: "TPS24",
          IP: "135.242.243.36",
          TestSetList: "lxia_1",
          DBServer: "135.252.217.71",
          DBUser: "root",
          DBPassword: "hermes123",
          DBPath: "/home/svtftp/database/",
          SWSeerver: "135.252.219.139",
          SWUser: "nei",
          SWPassword: "12345",
          SWPath: "/home/ci/loads/delivery/V01.00/latest"
        },
      ],
      currentPage: 1,

      neListColumns: [
        {
          type: 'selection',
          width: 60,
          align: 'center'
        },
        {
          title: "NeName",
          key: "NeName",
          width: 100,
        },
        {
          title: "NeType",
          key: "NeType",
          width: 100,
        },
        {
          title: "IP",
          key: "IP",
          width: 140,
        },
        {
          title: "TestSetList",
          key: "TestSetList",
          minWidth: 120,
        },
        {
          title: "DBServer",
          key: "DBServer",
          width: 140,
        },
        {
          title: "DBUser",
          key: "DBUser",
          width: 90,
        },
        {
          title: "DBPassword",
          key: "DBPassword",
          width: 130,
        },
        {
          title: "DBPath",
          key: "DBPath",
          minWidth: 180,
        },
        {
          title: "SWSeerver",
          key: "SWSeerver",
          width: 160,
        },
        {
          title: "SWUser",
          key: "SWUser",
          width: 100,
        },
        {
          title: "SWPassword",
          key: "SWPassword",
          width: 128,
        },
        {
          title: "SWPath",
          key: "SWPath",
          // width: 200,
          minWidth: 200,
        }
      ],
      selectionList:[],

      groupList:[
        // {
        //   title: "Group-1",
        //   neArray: ["Setup1", "Setup2"]
        // }
      ],

      operationList: [
        {
          text: "NEPorv",
          src: require("@/assets/images/operation-neporv.png")
        },
        {
          text: "WarmReset",
          src: require("@/assets/images/operation-warmreset.png")
        },
        {
          text: "ColdReset",
          src: require("@/assets/images/operation-coldreset.png")
        },
        {
          text: "ColdForce",
          src: require("@/assets/images/operation-coldforce.png")
        },
        {
          text: "DBRestored",
          src: require("@/assets/images/operation-dbrestored.png")
        },
        {
          text: "Upgrade",
          src: require("@/assets/images/operation-upgrade.png")
        },
        {
          text: "Power",
          src: require("@/assets/images/operation-power.png")
        },
      ],
      operationColumns: [
        {
          title: "Group Name",
          slot: "group"
        },
        {
          title: "Operation",
          key: "operation"
          // slot: "operation"
        },
        {
          title: "Loop Time",
          slot: "loop"
        },
        {
          title: "Traffic Monitor",
          // key: "trafficmonitor"
          slot: "trafficmonitor"
        },
        {
          title: "Action",
          slot: "action"
        }
      ],
      columns1: [
        {
          title: "Group",
          key: "group"
        },
        {
          title: "operation",
          key: "operation"
        },
        {
          title: "loop",
          key: "loop"
        },
        {
          title: "traffic monitor",
          key: "trafficmonitor"
        }
      ],
      data1: [
        // {
        //   operation: "NEPorv",
        //   loop: 1,
        //   trafficmonitor: true,
        // },
      ],
      showAlert: false,
      showRedDrag: false,
      
      groupListColumns: [
        {
          title: "Group",
          key: "group",
          width: 100,
          align: 'center'
        },
        {
          title: "NeName",
          key: "NeName",
          width: 100,
        },
        {
          title: "NeType",
          key: "NeType",
          width: 100,
        },
        {
          title: "IP",
          key: "IP",
          width: 140,
        },
        {
          title: "TestSetList",
          key: "TestSetList",
          minWidth: 120,
        },
        {
          title: "DBServer",
          key: "DBServer",
          width: 140,
        },
        {
          title: "DBUser",
          key: "DBUser",
          width: 90,
        },
        {
          title: "DBPassword",
          key: "DBPassword",
          width: 130,
        },
        {
          title: "DBPath",
          key: "DBPath",
          minWidth: 180,
        },
        {
          title: "SWSeerver",
          key: "SWSeerver",
          width: 160,
        },
        {
          title: "SWUser",
          key: "SWUser",
          width: 100,
        },
        {
          title: "SWPassword",
          key: "SWPassword",
          width: 128,
        },
        {
          title: "SWPath",
          key: "SWPath",
          // width: 200,
          minWidth: 200,
        }
      ],

      cell: 'group',

    };
  },
  mounted() {},
  created() {},
  methods: {
    back() {
      if (this.currentStep == 0) {
        // this.currentStep = 2;
        return;
      } else {
        this.currentStep -= 1;
      }
    },
    next() {
      if (this.currentStep == 3) {
        // this.currentStep = 0;
        return;
      } else {
        this.currentStep += 1;
      }
    },
    confirm() {
      alert("确认");
      // this.showAlert = true;
    },
    changePage(currentPage){
      console.log(currentPage);
      this.currentPage = currentPage;
    },
    onSelect(selection, row){
      // console.log(selection);
      // console.log(row);
    },
    onSelectionChange(selection){
      console.log(selection);
      let newArray = []
      selection.forEach(item => {
        newArray.push(item.NeName);
      });
      this.selectionList = newArray;
    },
    createGroup(){
      if(this.selectionList.length == 0){
        alert("您需要先选择网元才能对网元进行分组。");
        return;
      }
      console.log('createGroup');
      this.groupList.push({
        title: "Group-"+(this.groupList.length+1),
        neArray: this.selectionList
      });

    },
    dragStart(event, index) {
      console.log("dragStart: " + index);
      this.showRedDrag = true;
      event.dataTransfer.setData("Text", index);
    },
    allowDrop($event) {
      // $event.preventDefault();
    },
    drop($event) {
      $event.preventDefault();
      var index = $event.dataTransfer.getData("Text");
      // console.log("drop: " + index);
      // console.log(this.operationList[index]);
      console.log(Number(index));
      if(isNaN(Number(index))){
        return;
      }
      this.showRedDrag = false;

      this.data1.push({
        group: '',
        operation: this.operationList[index].text,
        loop: 1,
        trafficmonitor: true,
        showSelect: false
      });
    },
    showView(index) {
      this.$Modal.info({
        title: "Operation Info",
        content: `operation: ${this.data1[index].operation}<br>loop: ${this.data1[index].loop}<br>trafficmonitor: ${this.data1[index].trafficmonitor}`
      });
    },
    remove(index) {
      this.data1.splice(index, 1);
    },
    selectChangeGroup(selected, index){
      console.log(selected);
      console.log(index);
      this.data1[index].group = selected;

    }
  },
  computed: {
    currentNeList: function(){
      let start = this.currentPage * 5 - 5;
      let end = this.currentPage * 5;
      
      return this.neList.slice(start, end)
    },
    isMargintop0(){
      if(this.data1.length==0){
        return false;
      } else {
        return true;
      }
    },
    selectedNelist: function(){
      var newArray = [];
      this.groupList.forEach(group => {
        console.log(group.title);
        
        
        group.neArray.forEach(item => {
          
          // var selecteditem = null;
          this.neList.forEach(element => {
            if(item == element.NeName){

              var selecteditem = element;
              selecteditem.group = group.title;
              newArray.push(selecteditem);
            }
          });
        });
      });

      // for (const key1 in this.neList) {
      //   const element1 = this.neList[key1];
        
      //   for (const key2 in this.groupList) {
      //     const element2 = this.groupList[key2];
      //     let neArray = element2.neArray;
      //     for (const key3 in neArray) {
      //       const element3 = neArray[key3].title;
      //       if(element3 == element1.NeName){
              
      //         var selecteditem = element1;
      //         selecteditem.group = element2.title;
      //         newArray.push(selecteditem);
      //       }
      //     }
      //   }
      // }

      return newArray;
    }
  },
  watch: {},
  components: {},
  directives: {
    MergeCell
  }
};
</script>

<style scoped lang="scss">
.ivu-table {
  overflow: initial;
}
.ivu-table-wrapper {
  overflow: initial;;
}

ul.group-card {
  margin-top: 20px;
  li{
    float: left;
    width: 33%;
    height: 16px;
    padding-left: 15%;
    border-right: 1px solid #E9E9E9;

    img{
      cursor: pointer;
    }
  }
  li:last-of-type{
    border-right: none;
  }
}

ul.tools{
  margin-top: 20px;

  li{
    float: left;
    margin-right: 4%;
    width: 10%;
    
    img{
      display: block;
      margin: 0 auto;
    }

    p{
      text-align: center;
      color: #1890FF;
      font-weight:400;
    }
  }
}

.red-border{
  position: absolute;
  z-index: 10000;
  margin-top: -48px;
  width: 100%;
  height: 48px;
  line-height: 48px;
  text-align: center;
  border: 1px solid red;
  background: #fff;
}
.margin-top-0px{
  margin-top: 0;
}
</style>
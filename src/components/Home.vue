<template>
  <div class="layout">
    <Layout>
      <Header>
        <!-- <TopHead></TopHead>
        <Menu mode="horizontal" theme="dark" active-name="1">
          <div class="layout-logo">
            <img src="../assets/img/rpa.jpg" alt />
          </div>
          <div class="layout-nav">咩都有</div>
        </Menu>-->
      </Header>
      <Layout>
        <Sider hide-trigger :style="{background: '#fff'}">
          <Aside></Aside>
          <!-- <Menu active-name="1-2" theme="light" width="auto" :open-names="['1']">
            <Submenu name="1">
              <template slot="title">
                <Icon type="ios-navigate"></Icon>Item 1
              </template>
              <MenuItem name="1-1">Option 1</MenuItem>
              <MenuItem name="1-2">Option 2</MenuItem>
              <MenuItem name="1-3">Option 3</MenuItem>
            </Submenu>
            <Submenu name="2">
              <template slot="title">
                <Icon type="ios-keypad"></Icon>Item 2
              </template>
              <MenuItem name="2-1">Option 1</MenuItem>
              <MenuItem name="2-2">Option 2</MenuItem>
            </Submenu>
            <Submenu name="3">
              <template slot="title">
                <Icon type="ios-analytics"></Icon>Item 3
              </template>
              <MenuItem name="3-1">Option 1</MenuItem>
              <MenuItem name="3-2">Option 2</MenuItem>
            </Submenu>
          </Menu>-->
        </Sider>
        <Layout :style="{padding: '0 24px 24px'}">
          <Breadcrumb :style="{margin: '24px 0'}">
            <BreadcrumbItem>Home</BreadcrumbItem>
            <BreadcrumbItem>Components</BreadcrumbItem>
            <BreadcrumbItem>Layout</BreadcrumbItem>
          </Breadcrumb>
          <Content :style="{padding: '24px', minHeight: '280px', background: '#fff'}">
            Content
            <TimePicker type="time" placeholder="Select time" style="width: 168px"></TimePicker>
            <table cellspacing="0" class="tb">
              <thead>
                <tr>
                  <th>编号</th>
                  <th>品牌名称</th>
                  <th>创建时间</th>
                  <th>操作</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in dataList">
                  <td>{{index + 1}}</td>
                  <td>{{item.name}}</td>
                  <td>{{item.time}}</td>
                  <td>
                    <a href="#">删除</a>
                  </td>
                </tr>
              </tbody>
            </table>
          </Content>
        </Layout>
      </Layout>
    </Layout>
  </div>
</template>

<script>
import Header from "./common/Header";
import Aside from "./common/Aside";

export default {
  data() {
    return {
      dataList: [
        { name: '华为', time: Date.now() },
        { name: '小米', time: Date.now() },
        { name: '魅族', time: Date.now() }
      ]
    };
  },
  methods: {},
  components: {
    Header,
    Aside
  }
};
</script>

<style scoped>
.tb {
  border-collapse: collapse;
  /* width: 100%; */
  margin-top: 10px;
}
.tb th {
  background-color: #0094ff;
  color: white;
}
.tb td,
.tb th {
  padding: 5px;
  border: 1px solid black;
  text-align: center;
}
.add {
  padding: 5px;
  border: 1px solid black;
  margin-bottom: 10px;
}
</style>
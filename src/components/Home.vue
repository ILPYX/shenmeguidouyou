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
    return {};
  },
  methods: {},
  components: {
    Header,
    Aside
  }
};
</script>

<style scoped>
</style>
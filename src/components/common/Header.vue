<template>
  <Header>
    <Menu mode="horizontal" theme="dark" active-name="1">
      <div class="layout-logo">
        <img src="../../assets/img/rpa.jpg" alt />
      </div>
      <div class="layout-nav">借鉴(什么鬼都有)</div>
    </Menu>
  </Header>
  
</template>

<script>
export default {
  props: {}
};
</script>

<style scoped>
.layout-logo {
  width: 200px;
  height: 100%;
  background: #5b6270;
  border-radius: 3px;
  float: left;
  position: relative;
  top: 2px;
  left: 20px;
}
.layout-nav {
  color: #fff;
  font-size: 24px;
  margin-left: 240px;
  /* width: 420px;
  margin: 0 auto;
  margin-right: 20px; */
}
.layout-logo {
  width: 200px;
  height: 100%;
  background: #5b6270;
  border-radius: 3px;
  float: left;
  position: relative;
  top: 2px;
  left: 20px;
}
.layout-logo img {
  width: 100%;
  height: 100%;
}
</style>

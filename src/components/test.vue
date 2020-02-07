<template>
  <div>
    <Content :style="{padding: '24px', minHeight: '280px', background: '#fff'}">
      Content
      <TimePicker type="time" placeholder="Select time" style="width: 168px"></TimePicker>
      <Button type="primary" @click="toEcharts">自定义图表</Button>
      <Button type="primary" @click="showModal">选择资源</Button>
      <Modal v-model="addShow" :closable="false" :mask-closable="false">
        <div class="resource">
          <div class="resLeft">
            <p class="fontWeight">关联资源</p>
            <Table
              :width="280"
              :height="270"
              ref="resLeft"
              :columns="resCol"
              :data="addData.resLists"
              @on-selection-change="resLeftSelect"
              style="margin-top:30px"
            ></Table>
          </div>
          <div class="resCon">
            <Button type="primary" icon="arrow-left-a" @click="resAdd">选择</Button>
            <Button type="primary" icon="arrow-right-a" @click="resRemove">移除</Button>
            <Button type="primary" icon="close-circled" @click="resReset">重置</Button>
          </div>
          <div class="resRight">
            <p class="fontWeight">可选资源</p>
            <i-input></i-input>
            <Table
              :width="280"
              :height="270"
              ref="resRight"
              :columns="resCol"
              :data="addData.resListsCho"
              @on-selection-change="resRightSelect"
            ></Table>
          </div>
        </div>
      </Modal>
    </Content>
  </div>
</template>

<script>
export default {
  data() {
    return {
      addShow: false,
      resCol: [
        {
          type: "selection",
          width: 60
        },
        {
          title: "资源名称",
          key: "rseBpNam"
        }
      ],
      addData: { addLists: [], addListsCho: [] },
      moveToLeftRes: [],
      moveToRightRes: [],
      moveToLeftPro: [],
      moveToRightPro: []
    };
  },
  methods: {
    resLeftSelect(data) {
      this.moveToRightRes = data;
    },
    resRightSelect(data) {
      this.moveToLeftRes = data;
    },
    resAdd() {
      this.$Message.destroy();
      this.$refs.resLeft.selectAll(false);
      const toLeftRes = [];
      if (this.moveToLeftRes.length > 0) {
        var toRightId1 = [];
        this.moveToLeftRes.forEach(item => {
          toRightId1.push(item.uId);
        });
        this.addData.resListsCho = this.addData.resListsCho.filter(items => {
          return toRightId1.indexOf(items.uId) < 0;
        });
        this.moveToLeftRes.forEach(item => {
          var moveState1 = false;
          this.addData.resLists.forEach(obj => {
            if (obj.uId == item.uId) {
              moveState1 = true;
              return;
            }
          });
          if (!moveState1) {
            toLeftRes.push(item);
          }
        });
        if (toLeftRes.length > 0) {
          this.addData.resLists = this.addData.resLists.concat(toLeftRes);
          this.moveToLeftRes = [];
        } else {
          this.$Message.info({
            content: "资源已添加, 请勿重复添加",
            duration: 2
          });
        }
      } else {
        this.$Message.info({ content: "请选择要添加的资源", duration: 2 });
      }
    },
    resRemove() {},
    resReset() {},
    showModal() {
      this.addShow = true;
    },
    toEcharts() {
      this.$router.push({
        name: 'robot'
      })
    }
  },
};
</script>

<style scoped>
.layout {
  border: 1px solid #d7dde4;
  background: #f5f7f9;
  position: relative;
  border-radius: 4px;
  overflow: hidden;
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
.layout-nav {
  /* width: 420px; */
  /* margin: 0 auto; */
  /* margin-right: 20px; */
  color: #fff;
  font-size: 24px;
  margin-left: 240px;
}
.top {
  height: 64px;
}
.resource{margin:0 5px 10px 5px;padding:3px 5px;text-align: center}
.resLeft{padding:5px 10px;border: 1px solid #09f;border-radius: 4px;}
.resRight{padding:5px 10px;border: 1px solid #09f;border-radius: 4px;}
.resRight:after{display: block;content: "";clear: both;}
.resource>div{float: left;}
.fontWeight{font-weight: bolder;font-size: 14p;}
.resCon{width:100px;margin-top: 20px}
.resCon button{margin-top: 40px;}
.addDetail{width:100%;margin: 0 auto;overflow-y:auto;text-align: center;height: 400px;}
.addDetail:after{display: block;content: "";clear: both;}
</style>

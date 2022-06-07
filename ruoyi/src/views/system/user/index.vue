<template>
  <div class="app-container">
    <el-row
      :gutter="20"
      v-loading="loading1 | loading2"
      element-loading-text="页面加载中。。。"
      style="min-height: 300px"
    >
      <!--部门数据-->
      <el-col :span="4" :xs="24">
        <div v-if="ruoyi2Success">
          <micro-app
            name="ruoyi2"
            :data="dataForRuoyi2Child"
            url="http://localhost:82/"
            baseroute="/"
            @datachange="ruoyi2Mounted"
            @mounted="ruoyi2Mounted"
            @error="ruoyi2Error"
            @created="mounted2"
          ></micro-app>
        </div>
        <el-alert
          v-else
          title="此部分渲染出错"
          type="error"
          :closable="false"
          center
          show-icon
        >
        </el-alert>
      </el-col>
      <!--用户数据-->
      <el-col :span="20" :xs="24">
        <div v-if="ruoyi1Success">
          <micro-app
            name="ruoyi1"
            :data="dataForRuoyi1Child"
            url="http://localhost:81/"
            baseroute="/"
            @datachange="ruoyi1Mounted"
            @error="ruoyi1Error"
            @created="ruoyi1Created"
            @mounted="mounted1"
          ></micro-app>
        </div>
        <el-alert
          v-else
          title="此部分渲染出错"
          type="error"
          :closable="false"
          center
          show-icon
        >
        </el-alert>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import microApp from "@micro-zoe/micro-app";
export default {
  data() {
    return {
      dataForRuoyi2Child: { deptId: null },
      dataForRuoyi1Child: { deptId: null },
      ruoyi2Success: true,
      ruoyi1Success: true,
      loading1: false,
      loading2: false,
    };
  },
  methods: {
    // 数据改变
    ruoyi2Mounted() {
      let ruoyi2 = microApp.getData("ruoyi2");
      console.log("ruoyi2", ruoyi2);
      this.dataForRuoyi1Child = ruoyi2;
    },
    ruoyi1Mounted() {
      let ruoyi1 = microApp.getData("ruoyi1");
      console.log("ruoyi1", ruoyi1);
      this.dataForRuoyi1Child = ruoyi1;
      this.dataForRuoyi2Child = ruoyi1;
    },
    // 渲染出错
    ruoyi2Error() {
      this.ruoyi2Success = false;
      console.log("加载出错");
    },
    ruoyi1Error() {
      this.ruoyi1Success = false;
    },
    // 创建过程
    ruoyi1Created() {
      this.loading1 = true;
    },
    ruoyi2Created() {
      this.loading2 = true;
    },
    // 加载完成
    mounted2() {
      this.loading2 = false;
    },
    mounted1() {
      this.loading1 = false;
    },
  },
};
</script>

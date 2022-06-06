<template>
  <div class="app-container">
    <el-row :gutter="20">
      <!--部门数据-->
      <micro-app
        name="ruoyi2"
        :data="dataForRuoyi2Child"
        url="http://localhost:82/"
        baseroute="/"
        @datachange="ruoyi2Mounted"
      ></micro-app>
      <!--用户数据-->
      <micro-app
        name="ruoyi1"
        :data="dataForRuoyi1Child"
        url="http://localhost:81/"
        baseroute="/"
        @datachange="ruoyi1Mounted"
      ></micro-app>
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
    };
  },
  methods: {
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
  },
};
</script>

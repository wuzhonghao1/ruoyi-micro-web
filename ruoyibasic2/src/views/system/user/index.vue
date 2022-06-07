<template>
  <!--部门数据-->
  <div>
    <div class="head-container">
      <el-input
        v-model="deptName"
        placeholder="请输入部门名称"
        clearable
        size="small"
        prefix-icon="el-icon-search"
        style="margin-bottom: 20px"
      />
    </div>
    <div class="head-container">
      <el-tree
        :data="deptOptions"
        :props="defaultProps"
        v-model="deptId"
        :expand-on-click-node="false"
        :filter-node-method="filterNode"
        ref="tree"
        default-expand-all
        :highlight-current="deptId"
        @node-click="handleNodeClick"
      />
    </div>
  </div>
</template>

<script>
import { treeselect } from "@/api/system/dept";
import Treeselect from "@riophae/vue-treeselect";
import "@riophae/vue-treeselect/dist/vue-treeselect.css";

export default {
  name: "User",
  components: { Treeselect },
  data() {
    return {
      // 部门树选项
      deptOptions: undefined,
      // 查询参数
      deptId: undefined,
      defaultProps: {
        children: "children",
        label: "label",
      },
      // 部门名称
      deptName: undefined,
    };
  },
  watch: {
    // 根据名称筛选部门树
    deptName(val) {
      this.$refs.tree.filter(val);
    },
    deptId: {
      handler(val) {
        this.$forceUpdate();
      },
      deep: true,
      immediate: true,
    },
  },
  created() {
    this.getTreeselect();
    if (window.__MICRO_APP_ENVIRONMENT__) {
      window.microApp.addDataListener(this.dataListener);
    }
  },
  methods: {
    // 监听基座的数据传送事件
    dataListener(data) {
      console.log("data-ruoyi2", data);
      this.deptId = null;
    },
    /** 查询部门下拉树结构 */
    getTreeselect() {
      treeselect().then((response) => {
        this.deptOptions = response.data;
      });
    },
    // 筛选节点
    filterNode(value, data) {
      if (!value) return true;
      return data.label.indexOf(value) !== -1;
    },
    // 节点单击事件
    handleNodeClick(data) {
      this.deptId = data.id;
      // this.handleQuery();
      if (window.__MICRO_APP_ENVIRONMENT__) {
        window.microApp.dispatch({ deptId: data.id });
      }
    },
  },
};
</script>

<template>
  <div class="main" :style="{height: hideSidebar ? '100vh' : 'calc(100vh - 50px)'}">
    <el-button
      :style="{left: khoangCach + 'px'}"
      circle
      size="small"
      class="primary-button buttonAdd"
      icon="el-icon-plus"
      @click="addTab(editableTabsValue)"
    ></el-button>
    <el-tabs v-model="editableTabsValue" type="card" closable @tab-remove="removeTab">
      <el-tab-pane
        v-for="item in editableTabs"
        :key="item.name"
        :label="item.title"
        :name="item.name"
      >
        <tao-don></tao-don>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
import TaoDon from "./taodon2";
import { getInfor } from "@/api/taikhoan";

export default {
  components: {
    TaoDon,
  },
  data() {
    return {
      editableTabsValue: "2",
      hideSidebar: false,
      editableTabs: [
        {
          title: "HĐ 1",
          name: "1",
        },
        {
          title: "HĐ 2",
          name: "2",
        },
      ],
      tabIndex: 2,
      khoangCach: 204,
    };
  },
  created() {
    getInfor().then((res) => {
      this.role_id = res.data.role_id;
      if (
        this.$route.path == "/quanlydonhang/taodondathang" &&
        this.role_id == 2
      ) {
        this.hideSidebar = true;
      } else {
        this.hideSidebar = false;
      }
    });
  },
  watch: {},
  methods: {
    addTab(targetName) {
      let newTabName = ++this.tabIndex + "";
      let soTab = this.editableTabs.length + 1;
      if (soTab < 10) {
        this.khoangCach = soTab * 82 + 40;
      } else {
        this.khoangCach = 9 * 82 + (soTab - 9) * 90 + 40;
      }
      this.editableTabs.push({
        title: "HĐ " + (this.editableTabs.length + 1),
        name: newTabName,
      });
      this.editableTabsValue = newTabName;
    },
    removeTab(targetName) {
      let tabs = this.editableTabs;
      let activeName = this.editableTabsValue;
      if (activeName === targetName) {
        tabs.forEach((tab, index) => {
          if (tab.name === targetName) {
            let nextTab = tabs[index + 1] || tabs[index - 1];
            if (nextTab) {
              activeName = nextTab.name;
            }
          }
        });
      }
      this.editableTabsValue = activeName;
      this.editableTabs = tabs.filter((tab) => tab.name !== targetName);
      let soTab = this.editableTabs.length;
      if (soTab < 10) {
        this.khoangCach = soTab * 82 + 40;
      } else {
        this.khoangCach = 9 * 82 + (soTab - 9) * 90 + 40;
      }
    },
  },
};
</script>
<style scoped>
.buttonAdd {
  position: absolute;
  top: 17px;
  z-index: 1;
}
.main {
  overflow: hidden;
  padding-top: 15px;
  padding-left: 10px;
}
</style>
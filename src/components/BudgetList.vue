<template>
  <div class="budget-list-wrap">
    <ElCard :header="header">
      <el-row class="btns">
        <el-button type="primary" size="mini" plain @click="showAll">all</el-button>
        <el-button type="primary" size="mini" plain @click="sortList('INCOME')">coming</el-button>
        <el-button type="primary" size="mini" plain @click="sortList('OUTCOME')">consumption</el-button>
      </el-row>
      <template v-if="!isEmpty">
        <div class="list-item" v-for="(item, prop) in filterList" :key="prop">
          <span class="budget-comment">{{ item.comment }}</span>
          <template v-if="item.value > 0">
            <span class="budget-value green">
              <i class="el-icon-top"></i>
              {{ item.value }}
            </span>
          </template>
          <template v-else>
            <span class="budget-value red">
              <i class="el-icon-bottom"></i>
              {{ item.value }}
            </span>
          </template>

          <ElButton type="danger" size="mini" @click="dialogVisible = true">Delete</ElButton>
          <el-dialog title="Delete item" :visible.sync="dialogVisible" width="30%">
            <span>Yes / No ?</span>
            <span slot="footer" class="dialog-footer">
              <el-button @click="dialogVisible = false">NO</el-button>
              <el-button type="primary" @click="deleteItem(item.id)">YES</el-button>
            </span>
          </el-dialog>
        </div>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false"/>
    </ElCard>
  </div>
</template>

<script>
export default {
  name: "BudgetList",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    header: "Budget List",
    emptyTitle: "Empty List",
    dialogVisible: false,
    filterList: null,
    sortName: "INCOME"
  }),
  filteredList: {},
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    },
    sort() {
      let result = {};
      for (let item in this.list) {
        let sort = this.list[item];
        if (this.list[item].type === this.sortName) {
          result[item] = sort;
        }
      }
      return result;
    }
  },
  created() {
    this.filterList = this.list;
  },
  methods: {
    deleteItem(id) {
      this.$emit("deleteItem", id);
      this.dialogVisible = false;
    },
    sortList(sortName) {
      this.sortName = sortName;
      this.filterList = this.sort;
    },
    showAll() {
      return (this.filterList = this.list);
    }
  }
};
</script>

<style scoped>
.list-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}
.btns {
  margin-bottom: 30px;
}
.budget-value.red {
  color: red;
}

.budget-value.green {
  color: green;
}
</style>



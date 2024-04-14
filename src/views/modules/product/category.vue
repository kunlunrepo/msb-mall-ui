<template>
  <el-tree
    :data="data"
    :props="defaultProps"
    :expand-on-click-node="false"
    show-checkbox
    node-key="catId"
    @node-drop="handleDrop"
    :default-expanded-keys="expandKeys"
    :allow-drop="allowDrop"
    :draggable="draggable"
    ref="tree"
  >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button
            v-if="data.catLevel <= 2"
            type="text"
            size="mini"
            @click="() => append(data)"
          >
            添加
          </el-button>
          <el-button type="text" size="mini" @click="() => edit(data)">
            更新
          </el-button>
          <el-button
            v-if="data.children && data.children.length == 0"
            type="text"
            size="mini"
            @click="() => remove(node, data)"
          >
            删除
          </el-button>
        </span>
      </span>
  </el-tree>
</template>

<script>
export default {
  data () {
    return {
      data: [],
      defaultProps: {
        children: 'children',
        label: 'name'
      }
    }
  },
  methods: {
    getCategory () {
      this.$http({
        url: this.$http.adornUrl('/product/category/listTree'),
        method: 'get',
        params: this.$http.adornParams()
      }).then(({data}) => {
        console.log('成功获取的类别数据', data.data)
        this.data = data.data
      })
    }
  },
  created () {
    this.getCategory()
  }
}
</script>

<style scoped>

</style>

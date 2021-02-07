<template>
  <div class="app-container">

    <el-radio-group v-model="radio" @change="handleSingChoose">
      <el-radio-button :label="0">全部</el-radio-button>
      <el-radio-button :label="1">在线</el-radio-button>
      <el-radio-button :label="2">离线</el-radio-button>
    </el-radio-group>

    <el-tree
      ref="tree2"
      :data="curData"
      :default-expanded-keys="expandedList"
      node-key="nodeName"
      @node-expand="nodeExpand"
      @node-collapse="nodeCollapse"
      :props="defaultProps"
      class="filter-tree"
      @node-click="hanldeClickNode"
    >
      <span slot-scope="{ node, data }" class="custom-tree-node">
        <span>{{ data.nodeName }}</span>
        <span v-if="data.type === 'device' && data.status == 1">(在线)</span>
        <span v-if="data.type === 'device' && data.status != 1">(离线)</span>
      </span>
    </el-tree>

  </div>
</template>

<script>
import treeData from '@/assets/data/tree.json'
import lodash from 'lodash'

function filterTreeOn(arr, status) {
  const a = arr.map(item => {
    if (item.children) {
      item.children = item.children.filter(item0 => {
        if (item0.type !== 'device') {
          return true
        } else {
          if (status === 'on' ? (item0.status != 1) : (item0.status == 1)) {
            return false
          } else {
            return true
          }
        }
      })
      filterTreeOn(item.children, status)
    }
    return item
  })
  return a
}

export default {

  data() {
    return {
      allData: [],
      onData: [],
      offData: [],
      curData: [],
      expandedList: [],
      defaultProps: {
        children: 'children',
        label: 'nodeName'
      },
      radio: 0
    }
  },
  created() {
    this.curData = this.allData = treeData
  },
  computed: {},
  watch: {
    expandedList(v) {
      console.log('%c🌼(v)🚭', 'font-size:20px;border:3px inset #42b983', v)
    }
  },
  methods: {
    handleSingChoose(e) {
      if (e === 1) { // 在线
        this.curData = this.onData = filterTreeOn(lodash.cloneDeep(this.allData), 'on')
      } else if (e === 2) {
        this.curData = this.offData = filterTreeOn(lodash.cloneDeep(this.allData), 'off')
      } else {
        this.curData = this.allData
      }
    },
    hanldeClickNode(e) {
      console.log('%c🌼(e)🚭', 'font-size:20px;border:3px inset #42b983', e)
    },
    nodeExpand(data) {
      this.expandedList.push(data.nodeName)
    },
    nodeCollapse(data) {
      this.expandedList.splice(this.expandedList.indexOf(data.nodeName), 1)
    }
  }
}
</script>


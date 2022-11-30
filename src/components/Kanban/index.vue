<!--
 * @Author : your Name 
 * @Date : Do not edit 
 * @LastEditors : your Name 
 * @LastEditTime : Do not edit 
 * @Description : 
-->

<template>
  <div class="board-column">
    <div class="board-column-header">
      {{ headerText }}
    </div>
    <draggable :list="list" class="board-column-content" :set-data="setData" item-key="id">
      <template #default="{element}">
        <div  class="board-item">
          {{ element.name }}
        </div>
      </template>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'DragKanbanDemo',
  components: {
    draggable
  },
  props: {
    headerText: {
      type: String,
      default: 'Header'
    },
    options: {
      type: Object,
      default() {
        return {}
      }
    },
    list: {
      type: Array,
      default() {
        return []
      }
    },
  },
  methods: {
    setData(dataTransfer) {
      dataTransfer.setData('Text', '')
    },
  }
}
</script>
<style lang="less" scoped>
.board-column {
  overflow: hidden;
  border-radius: 3px;
  border: 1px solid #e6ebf5;
  .board-column-header {
    height: 40px;
    line-height: 40px;
    overflow: hidden;
    padding: 0 20px;
    text-align: center;
    background: #F5F7FA;
    color: #606266;
    border-radius: 3px 3px 0 0;
  }
  .board-column-content{
    margin-bottom: 15px;
    .board-item{
      background-color: white;
      border: 1px solid #e6ebf5;
      box-shadow: 0px 0px 4px 0px #e6ebf5;
      border-radius: 4px;
      padding: 15px;
      margin:15px 15px 0px 15px;
    }
  }
}
</style>


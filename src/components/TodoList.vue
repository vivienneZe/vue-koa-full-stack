<template>
  <el-row>
    <el-col :xs="{span:20,offset: 2}" :sm="{span:8, offset: 8}">
      <span>
        欢迎：{{name}}！你的代办事项是：
      </span>
      <el-input placeholder="请输入代办事项" v-model="todos" @keyup.enter.native="addTodos"></el-input>
      <el-tabs v-model="activeName">
        <el-tab-pane label="待办事项" name="first">
          <el-col :xs="24">
            <div v-if="!Done">
              <div v-for="(item, index) in list" :key="index">
                <div class="todo-list" v-if="item.status === false">
                  <span class="item">
                    {{index + 1}}.{{item.content}}
                  </span>
                  <span class="pull-right">
                    <el-button size="small" type="primary" @click="finished(index)">完成</el-button>
                    <el-button size="small" :plain="true" type="danger" @click="remove(index)">删除</el-button>
                  </span>
                </div>
              </div>
            </div>
            <div v-else-if="Done">
              暂无代办事项
            </div>
          </el-col>
        </el-tab-pane>
        <el-tab-pane label="已完成事项" name>
          <div v-if="count > 0">
            <div v-for="(item, index) in list" :key="index">
              <div class="todo-list" v-if="item.status == true">
                <span class="item finished">{{index + 1}}. {{item.content}}</span>
                <span class="pull-right">
                  <el-button size="small" type="primary" @click="restore(index)">还原</el-button>
                </span>
              </div>
            </div>
          </div>
          <div v-else>
            暂无已完成事项
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-col>
  </el-row>
</template>
<script>
export default {
  data () {
    return {
      name: 'Vivienne',
      todos: '',
      activeName: '',
      list: [],
      count: 0
    }
  },
  computed: {
    Done () {
      let count = 0
      let length = this.list.length
      for (let i in this.list) {
        this.list[i].status === true ? count += 1 : ''
      }
      this.count = count
      if (count === length || length === 0) {
        return true
      } else {
        return false
      }
    }
  },
  methods: {
    addTodos () {
      if (this.todos === '') {
        return
      }
      let obj = {
        status: false,
        content: this.todos
      }
      this.list.push(obj)
      this.todos = ''
    },
    finished (index) {
      this.$set(this.list[index], 'status', true)
      this.$message({
        type: 'success',
        message: '任务完成'
      })
    },
    remove (index) {
      this.list.splice(index, 1)
      this.$message({
        type: 'info',
        message: '任务删除'
      })
    },
    restore (index) {
      this.$set(this.list[index], 'status', false)
      this.$message({
        type: 'info',
        message: '任务还原'
      })
    }
  }
}
</script>
<style>
.el-input {
  margin: 20px auto;
}
.todo-list{
  width: 100%;
  margin-top: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  overflow: hidden;
  text-align: left;
}
.item {
  font-size: 20px;
}
.finished {
  text-decoration: line-through;
  color: #ddd;
}
.pull-right {
  float: right;
}
</style>

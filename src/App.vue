<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <input placeholder="Todo List" v-model="newItem" v-on:keyup.enter="addNew()" class="inputStyle">
    <ul v-bind:style="cleanStyle">
      <li v-for="item in items" v-bind:class="[{ finished : item.isFinished}, liPublic]"
          v-on:click="toggleFinish(item)">
        {{ item.label }}
      </li>
    </ul>
  </div>
</template>

<script>
  // 引入store.js
  import Store from './store'
  export default {
    data () {
      return {
        // 标题
        title: 'This is a Todo List!',
        // ul初始化样式
        cleanStyle: {
          listStyleType: 'none',
          margin: '10px auto',
          padding: '0 10px 0 0',
          width: '230px',
          fontSize: '18px'
        },
        // li左对齐
        liPublic: 'liPublic',
        // 取出localStorage的数据（Array类型）
        items: Store.fetch(),
        // 初始数据
        newItem: ''
      }
    },
    watch: {
      // 监听items的变化
      items: {
        // 深度 watcher
        handler (items) {
          // 存入localStorage
          Store.save(items)
        },
        deep: true
      }
    },
    methods: {
      toggleFinish (item) {
        // 取反isFinished
        item.isFinished = !item.isFinished
      },
      addNew () {
        // 添加数据
        this.items.push({
          label: this.newItem,
          isFinished: false
        })
        // 清空input
        this.newItem = ''
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #ffd28a;
    margin-top: 60px;
  }

  .liPublic {
    text-align: left;
    text-indent: 15px;
    color: #666666;
    margin: 2px 0;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    position: relative;
  }

  .liPublic:before{
    content: '';
    display: block;
    width: 10px;
    height: 10px;
    overflow: hidden;
    background: url(assets/logo.png) no-repeat;
    background-size: 10px;
    position: absolute;
    left: 0;
    top: 9px;
  }
  
  .finished {
    text-decoration: line-through;
    color: #ddd;
  }

  .inputStyle{
    background:none;
    outline:none;
    width: 230px;
    border: 2px solid #bbbbbb;
    border-radius: 20px;
    height: 25px;
    padding: 0 10px;
  }
</style>

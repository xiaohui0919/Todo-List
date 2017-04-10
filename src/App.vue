<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <input v-model="newItem" v-on:keyup.enter="addNew()">
    <ul v-bind:style="cleanPoint">
      <li v-for="item in items" v-bind:class="{ finished : item.isFinished}" v-on:click="toggleFinish(item)">
        {{ item.label }}
      </li>
    </ul>
  </div>
</template>

<script>
  import Store from './store'
  console.log(Store)
  export default {
    data () {
      return {
        title: 'This is a todo List!',
        cleanPoint: {
          listStyleType: 'none',
          margin: '10px 0',
          padding: 0
        },
        items: Store.fetch(),
        newItem: ''
      }
    },
    watch: {
      items: {
        handler (items) {
          Store.save(items)
        },
        deep: true
      }
    },
    methods: {
      toggleFinish (item) {
        item.isFinished = !item.isFinished
      },
      addNew () {
        this.items.push({
          label: this.newItem,
          isFinished: false
        })
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
    color: #2c3e50;
    margin-top: 60px;
  }

  .finished {
    text-decoration: underline;
  }

</style>

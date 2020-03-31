<template>
    <div id="app">
    <p>
      <input type="text"
        placeholder="TODOを入力しましょう！"
        v-model="newItemTitle"
        v-on:keyup.enter="addTodo(newItemTitle)">
      <button v-on:click="deleteTodo()">clean</button>
    </p>
    <transition-group name="list-complete" tag="ul" class="todos">
      <TodoItem v-for="item in items" v-bind="item" v-bind:key="item.id" v-on:delete="updateCheck"></TodoItem>
    </transition-group>
    
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue'
export default {
  data: function() {
    return {
			items: [],
			newItemTitle: '',
		}
	},
	components: {
		TodoItem,
	},
  methods: {
    updateCheck: function(childChecked, childIndex){  //updateCheckメソッドをまるっと追加
      for (let i = 0; i < this.items.length; i++) {
        if (this.items[i].id === childIndex) {
          this.items[i].isChecked = childChecked;
          break;
        }
      }
    },
    addTodo: function(newTitle){
      let date = Date.now();
      this.items.push({
        title: newTitle,
        isChecked: false,
        id: date  //idを追加
      });
      this.newItemTitle = '';
      this.saveTodo();
    },
    deleteTodo: function(){
      this.items = this.items.filter(function (item) {
        return item.isChecked === false;
      });
      this.saveTodo();
    },
    saveTodo: function(){
      localStorage.setItem('items', JSON.stringify(this.items));
    },
    loadTodo: function(){
      this.items = JSON.parse( localStorage.getItem('items') );
      if( !this.items ){
        this.items = [];
      }
    },
  },
  mounted: function(){
    this.loadTodo();
  },
}
</script>

<style scoped>
.done {
    text-decoration: line-through;
}
.list-complete-item {
    transition: all 1s;
    list-style:none;
    padding:20px;
    border:1px black solid;
    max-width:50%;
  }
  .list-complete-enter, .list-complete-leave-to
  /* .list-complete-leave-active for below version 2.1.8 */ {
    opacity: 0;
    transform: translateX(30px);
  }
  .list-complete-leave-active {
    position: absolute;
  }

 .todos{
   text-align:center;
 }
</style>
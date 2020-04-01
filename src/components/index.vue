<template>
    <div id="app">
    <p>
      <input type="text"
        placeholder="買ったものを入力しよう！"
        v-model="newItemTitle">
      <Datepicker
        v-model="date"
        :format="DatePickerFormat"
        :language="ja"
        name="datepicker"></Datepicker>
			<button v-on:click="addTodo(newItemTitle, date)">add</button>
      <button v-on:click="deleteTodo()">clean</button>
    </p>
    <transition-group name="list-complete" tag="ul" class="todos">
      <TodoItem v-for="item in items" v-bind="item" v-bind:key="item.id" v-on:delete="updateCheck"></TodoItem>
    </transition-group>
    
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue'
import Datepicker from 'vuejs-datepicker'
import moment from 'moment/moment'
export default {
  data: function() {
    return {
			items: [],
			newItemTitle: '',

      //Datapicker
      date: new Date(),
        DatePickerFormat: 'yyyy-MM-dd',
        ja: {
            language: 'Japanese',
            months: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
            monthsAbbr: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
            days: ['日', '月', '火', '水', '木', '金', '土'],
            rtl: false,
            ymd: 'yyyy-MM-dd',
            yearSuffix: '年'
        }
		}
	},
	components: {
		TodoItem,
    Datepicker
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
    addTodo: function(newTitle, bb){
			if(this.newItemTitle !== '') {
        let date = Date.now();
        bb = this.customformat(bb);
				this.items.push({
					title: newTitle,
					isChecked: false,
          id: date,  //idを追加
          bb: bb,
				});
				this.newItemTitle = '';
				this.saveTodo();
			}
    },
    deleteTodo: function(){
      this.items = this.items.filter(function (item) {
        return item.isChecked === false;
      });
      this.saveTodo();
    },
    saveTodo: function(){
      this.items.sort(function(a, b) {
        return (a.bb > b.bb ? 1 : -1);
      });
      localStorage.setItem('items', JSON.stringify(this.items));
    },
    loadTodo: function(){
      this.items = JSON.parse( localStorage.getItem('items') );
      if( !this.items ){
        this.items = [];
      }
    },
    customformat: function(value) {
      return moment(value).format('YYYY-MM-DD');
    },
  },
  mounted: function(){
    localStorage.clear();
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
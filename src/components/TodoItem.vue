<template>
	<li class="list-complete-item">
    <label v-bind:class="{ done: isChecked }">
      <input type="checkbox" v-model="childisChecked" v-on:change="deleteCheck">
      {{ title + " 賞味期限:" + bb + "(あと" + bbDays + "日)"}}
    </label>
  </li>
</template>

<script>
import moment from 'moment/moment'
export default {
	props: ['title','isChecked','id', 'bb'],
  data: function(){
    return {
      childisChecked: this.isChecked,  //v-modelでisCheckedを指定できないため追加
      bbDays: (moment(this.bb).diff(moment(), 'days') + 1)
    }
  },
  methods: {
    deleteCheck: function(){
      this.$emit('delete', this.childisChecked, this.id);
    }
  }
}
</script>
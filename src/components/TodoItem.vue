<template>
	<li class="list-complete-item" :class="isExpired(bbDays)">
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
      bbDays: (moment(this.bb).diff(moment(), 'days')),

      // isExpired
      ClassDanger: 'danger',
      ClassWarn: 'warn'
    }
  },
  methods: {
    deleteCheck: function(){
      this.$emit('delete', this.childisChecked, this.id);
    },
    isExpired: function(day) {
      if(day < 0) {
        return this.ClassDanger;
      } else if(day <= 5) {
        return this.ClassWarn;
      }
    }
  }
}
</script>

<style scoped>
.danger {
  background-color: rgba(255, 0, 0, 0.5);
}
.warn {
  background-color: rgba(255, 255, 0.5);
}
</style>
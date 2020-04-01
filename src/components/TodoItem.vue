<template>
	<li class="list-complete-item">
    <label v-bind:class="{ done: isChecked }">
      <input type="checkbox" v-model="childisChecked" v-on:change="deleteCheck"> {{ title + " " + bb }}
    </label>
  </li>
</template>

<script>
export default {
	props: ['title','isChecked','id', 'bb'],
  data: function(){
    return {
      childisChecked: this.isChecked  //v-modelでisCheckedを指定できないため追加
    }
  },
  methods: {
    deleteCheck: function(){
      this.$emit('delete', this.childisChecked, this.id);
    }
  }
}
</script>
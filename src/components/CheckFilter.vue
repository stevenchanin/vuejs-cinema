<template>
  <div class="check-filter" v-on:click="checkFilter" v-bind:class="{active: checked}">
    <span class="checkbox"></span>
    <span class="check-filter-title">{{ title }}</span>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        checked: false
      }
    },

    props: [ 'title' ],

    methods: {
      checkFilter() {
        this.checked = !this.checked;
        this.$emit('check-filter', 'genre', this.title, this.checked);
      }
    }
  }
</script>

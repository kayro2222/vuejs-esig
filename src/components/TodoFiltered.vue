<template>
  <div>
    <button :class="{ active: filter == 'all' }" @click="changeFilter('all')">All</button>
    <button :class="{ active: filter == 'active' }" @click="changeFilter('active')">Active</button>
    <button :class="{ active: filter == 'completed' }" @click="changeFilter('completed')">Completed</button>
    <transition name="fade">
      <button class="clear" v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
    </transition>
  </div>
</template>

<script>


export default {
  name: 'todo-filtered',
  computed: {
    filter() {
      return this.$store.state.filter
    },
    showClearCompletedButton() {
      return this.$store.getters.showClearCompletedButton
    }
  },
  methods: {
    changeFilter(filter) {
      this.$store.dispatch('updateFilter', filter)
    },
    clearCompleted() {
      this.$store.dispatch('clearCompleted')
    }
  }
}
</script>
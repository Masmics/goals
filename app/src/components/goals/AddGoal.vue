<template>
  <form @submit.prevent="handleSubmit">
    <label>Goal:</label>
    <input v-focus v-model="goal.title" require>
    <label>Start Date:</label>
    <input v-focus v-model="goal.startdate" require>
    <button>Add</button>
  </form>
</template>

<script>

function initGoal() {
  return {
    title: '',
    startdate: ''
  };
}


export default {
  props: {
    onAdd: Function
  },
  data() {
    return {
      goal: initGoal()
    };
  },
  methods: {
    handleSubmit() {
      this.onAdd(this.goal)
        .then(() => {
          this.goal = initGoal();
        });
    }
  }
};
</script>

<style lang="postcss" scoped>

form {
  border: 1px solid #aaa;
  padding: 10px;
}

input, button {
  margin: 0 5px;
}

input {
  width: 125px;
  font-size: 1.05em;
}
</style>

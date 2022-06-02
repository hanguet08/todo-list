<template>
  <div id="app">

  <todo-table></todo-table>
  </div>
</template>

<script>
import TodoTable from "@/components/TodoList.vue";


export default {
  name: "app",
  components: {
    TodoTable,

  },
};
</script>

<style>
@import url("./css/common/main.css");
@import url("./css/common/content.css");
@import url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css);
</style>
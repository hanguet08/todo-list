<template>
<!-- New Task -->
  <div class="container-left">
    <div class="content-betwwen-element">
      <div class="new-task-title">New Task</div>
      <input
        type="text"
        class="input-dialog-new-task"
        v-model="title"
        placeholder="Add new task"
      />
    </div>
    <div class="content-betwwen-element">
      <div class="description-title">Description</div>
      <textarea
        class="description-new-task"
        v-model="description"
        rows="4"
        cols="50"
        name="comment"
        form="usrform"
      >
Enter text here...</textarea
      >
    </div>
    <div class="content-betwwen-element priority-date">
      <div class="due-date">
        <div class="due-date-title">Due date</div>
        <input
          v-model="myDate"
          type="date"
          id="dueDate"
          name="due-date"
          class="input-due-date"
          tabindex=""
        />
      </div>
      <div class="priority_date">
        <div class="priority_date_title">Priority</div>
        <select class="priority-select" v-model="priority">
          <option
            v-for="option in priorities"
            :value="option.name"
            :key="option.id"
          >
            {{ option.name }}
          </option>
        </select>
      </div>
    </div>
    <div id="btnAdd">
      <button class="m-btn add-button" @click="addTodo">Add</button>
    </div>
  </div>
</template>
<script>
import Enumeration from "@/js/Common/Enumeration";
export default {
  name: "TodoAddModal",
  props: {
    priorities: {
      type: Array,
      required: true,
    },
    /*  
        Add : 1 
        Edit : 2 
    */
    formMode: Number,
  },
  data() {
    return {
      priority: "Normal",
      title: "",
      description: "",
      myDate: new Date().toISOString().slice(0, 10),
    };
  },

  methods: {
    // Thêm một task mới
    addTodo() {
      this.formMode == Enumeration.FormMode.Add;
      const payload = {
        priority: this.priority,
        title: this.title,
        description: this.description,
        myDate: this.myDate,
      };
      this.$emit("add-todo", payload);
      this.title = "";
      this.description = "";
    },
    
  },
};
</script>

<style scoped>
</style> 
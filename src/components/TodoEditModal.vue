<template>
  <!-- Dialog binding dữ liệu và update -->
  <div class="edit-todo-modal dialog" v-bind:class="{ open: isShow }">
    <div class="container-left edit-modal">
      <div class="content-betwwen-element">
        <div class="new-task-title">Edit Task</div>
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
      <!-- -->
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
        <!--  -->
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
      <div id="btnEdit" class="content-betwwen-element">
        <button class="m-btn edit-button" @click="editTodo">Update</button>
      </div>
    </div>
  </div>
</template>
<script>
import Enumeration from "@/js/Common/Enumeration";
export default {
  name: "TodoEditModal",
  props: {
    // ẩn hiện form
    isShow: {
      type: Boolean,
      default: false,
    },
    priorities: {
      type: Array,
      required: true,
    },
    /*  
        Add : 1 
        Edit : 2 
    */
    formMode: Number,
    // là dữ liệu nhận được khi formMode là Edit
    todo: {
      type: Object,
      required: true,
    },
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
    // lấy thông tin hiện form chi tiết
    showTodoDetail(item) {
      this.title = item.todo;
      this.priority = item.priority;
      this.description = item.description;
      this.date = item.date;
    },
    // Update dữ liệu

    editTodo() {
      this.formMode == Enumeration.FormMode.Edit;
      const payload = {
        id: this.todo.id,
        todo: this.title,
        priority: this.priority,
        description: this.description,
        myDate: this.myDate,
      };
      this.$emit("edit-todo", payload);
    },
  },
};
</script>

<style scoped>
</style> 
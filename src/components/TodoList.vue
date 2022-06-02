<template>
  <div class="container">
    <!-- container-left -->
    <todo-add-modal
      ref="todoModal"
      :priorities="priorities"
      :formMode="formMode"
      @add-todo="onAddTodo"
    ></todo-add-modal>
    <!-- contain-right -->
    <div class="container-right">
      <div class="container-right-title">To Do List</div>
      <div class="search-by-title element-right">
        <input
          type="text"
          v-model="search"
          class="search-box input-search"
          placeholder="Tìm kiếm"
        />
      </div>

      <div class="content-table">
        <table id="tableListTask" class="table">
          <thead>
            <tr></tr>
          </thead>
          <tbody>
            <div class="element-right">
              <tr v-for="item in filterTodos" :key="item.id">
                <td>
                  <input
                    type="checkbox"
                    class="toggle"
                    @click="checkBox(item)"
                  />
                </td>
                <td style="width: 350px">{{ item.todo }}</td>

                <td>
                  <button
                    class="m-btn m-btn-bg detail-button"
                    @click="openEditModal(item)"
                  >
                    Detail
                  </button>
                </td>
                <td>
                  <button
                    class="m-btn m-btn-bg delete-button"
                    @click="deleteTodo(item)"
                  >
                    Remove
                  </button>
                </td>
              </tr>
            </div>
          </tbody>
        </table>
      </div>

      <div class="bulk-action" :style="{ display: showBulkAction }">
        <div class="bulk-action-title">Bulk Action:</div>
        <div class="bulk-action-button">
          <button class="m-btn bulk-action-due">Due</button>
          <button class="m-btn bulk-action-remove" @click="deleteAllTodos">
            Remove
          </button>
        </div>
      </div>
    </div>
  </div>
  <todo-edit-modal
    ref="todoModal"
    :todo="selectedTodo"
    :isShow="isShowDialog"
    :priorities="priorities"
    :formMode="formMode"
    @closeOnClick="showEditDialog"
    @edit-todo="onEditTodo"
  />
</template>
<script>
import TodoAddModal from "./TodoAddModal.vue";
import TodoEditModal from "./TodoEditModal.vue";
import Enumeration from "@/js/Common/Enumeration";
export default {
  name: "TodoTable",
  components: { TodoAddModal, TodoEditModal },
  props: {},
  data() {
    return {
      initialTodos: [],
      todos: [],
      // phục vụ cho hiển form thông tin chi tiết
      isShowDialog: false,
      selectedTodo: {},
      count: 0,
      // trạng thái ô checkbox false: không chọn, true: chọn
      checked: false,
      // tìm kiếm
      search: "",
      formMode: Enumeration.FormMode.Add,
      priorities: [
        { id: 1, name: "High" },
        { id: 2, name: "Low" },
        { id: 3, name: "Normal" },
      ],
    };
  },
  computed: {
    // kiểm tra khi nào hiện hộp thoại bulk action
    showBulkAction() {
      if (this.count == 0) {
        return "none";
      }
      return "block";
    },
    // tìm kiếm dữ liệu
    filterTodos() {
      return this.todos.filter((todo) => {
        return todo.todo.toLowerCase().includes(this.search.toLowerCase());
      });
    },
  },
  mounted() {
    if (localStorage.getItem("todos")) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    } else {
      this.todos = this.initialTodos;
    }
  },
  methods: {
    // tính tổng số ô checkbox được tick
    totalChecked() {
      this.count = 0;
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].checked == true) {
          this.count++;
        }
      }
    },
    // Kiểm tra trạng thái của ô checkbox
    checkBox(item) {
      item.checked = !item.checked;
      this.totalChecked();
      console.log(this.count);
    },
    // hiện form thông tin chi tiết
    openEditModal(todo) {
      this.formMode = Enumeration.FormMode.Edit;
      this.showEditDialog(true);
      this.$refs.todoModal.showTodoDetail(todo);
      this.selectedTodo = todo;
    },
    // Thêm một task mới
    onAddTodo(item) {
      // Lấy số id cao nhất
      const highestId = Math.max.apply(
        Math,
        this.todos.map((item) => item.id)
      );
      // Thêm item vào array
      this.todos.push({
        id: highestId + 1,
        todo: item.title,
        priority: item.priority,
        description: item.description,
        myDate: item.myDate,
      });
      // Lưu array đã cấp nhật vào localstorage
      this.saveLocalStorageTodos();
    },
    // Edit dữ liệu
    onEditTodo(item) {
      const todo = this.findTodo(item);
      // Apply the updated values
      todo.todo = item.todo;
      todo.priority = item.priority;
      todo.myDate = item.myDate;
      todo.description = item.description;
      // Lưu array đã cấp nhật vào localstorage
      this.saveLocalStorageTodos();
      // ẩn form
      this.showEditDialog(false);
    },
    // xóa task
    deleteTodo(item) {
      // tìm task cần xóa
      const index = this.todos.indexOf(item);
      this.todos.splice(index, 1);
      // Lưu array đã cấp nhật vào localStorage
      this.saveLocalStorageTodos();
    },
    // Xóa các ô checkbox được chọn
    deleteAllTodos() {
      this.todos = this.todos.filter(function (todo) {
        return !todo.checked;
      });
      this.count = 0;
      this.saveLocalStorageTodos();
    },
    // Hiện form thông tin chi tiết
    showEditDialog(isShow) {
      this.isShowDialog = isShow;
    },
    // Lưu dữ liệu vào localStorage
    saveLocalStorageTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
      this.todos = JSON.parse(localStorage.getItem("todos"));
    },
    // Tìm task theo id
    findTodo(item) {
      return this.todos.find((todo) => todo.id === item.id);
    },
  },
};
</script>

<style scoped>
.visible {
  display: block;
}
.bulk-action {
  display: none;
}
</style>


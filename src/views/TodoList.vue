<template>
  <div class="todo-list-main">
    <h1>Todo List Application</h1>
    <h4>
      {{
        taskRemaining == 0 && todoListItems.length > 0
          ? "You have finished all your tasks ✅"
          : todoListItems.length == 0
          ? "You have no task today"
          : `Not completed tasks : ${taskRemaining} `
      }}
    </h4>
    <div class="todo-list-container">
      <div class="todo-list-add">
        <CustomForm
          :type="'text'"
          :placeholder="'Add todo title...'"
          v-model="title"
          class="todo-form-add-element"
        />
        <CustomForm
          v-model="content"
          :type="'text'"
          :placeholder="'Add todo content...'"
          class="todo-form-add-element"
        />
        <CustomButtonVue
          :btnText="'Add'"
          :variant="'primary'"
          @click="addNewTodo"
          class="todo-form-add-element"
        />
      </div>
      <div
        class="todo-list-items"
        v-for="todo in todoListItems"
        v-bind:key="todo.id"
      >
        <TodoTask
          :id="todo.id"
          :title="todo.title"
          :content="todo.content"
          :isCompleted="todo.isCompleted"
          @delete-todo="deleteTodo"
          @mark-to-complete="markToComplete"
          @edit-todo="editTodo"
        />
      </div>
    </div>
    <CustomModal
      :id="modalId"
      :openModal="modalStatus"
      :titleValue="modalTitle"
      :contentValue="modalContent"
      @save-todo="saveTodo"
      @close-modal="() => (modalStatus = !modalStatus)"
    />
    <BAlert
      v-show="successAlert"
      :model-value="true"
      variant="success"
      class="alert"
      :interval="1000"
      :fade="true"
      >Todo task added successfully.</BAlert
    >
    <BAlert
      v-show="dangerAlert"
      :model-value="true"
      variant="danger"
      class="alert"
      :fade="true"
      :interval="1000"
      >Please check all fields!</BAlert
    >
  </div>
</template>

<script>
import CustomForm from "../components/base/CustomForm.vue";
import CustomButtonVue from "../components/base/CustomButton.vue";
import CustomModal from "../components/modals/CustomModal.vue";
import TodoTask from "../components/task/TodoTask.vue";

export default {
  name: "TodoList",
  components: { CustomButtonVue, TodoTask, CustomModal, CustomForm },

  data() {
    return {
      todoListItems: [],
      title: "",
      content: "",
      modalTitle: null,
      modalContent: null,
      modalId: null,
      modalStatus: false,
      taskRemaining: 0,
      dangerAlert: false,
      successAlert: false,
    };
  },
  mounted() {
    let todoListItems = [
      {
        id: 1,
        title: "Learn Vue Node JS",
        content: "We must learn Node JS...",
        isCompleted: false,
      },
      {
        id: 2,
        title: "Learn Vue JS",
        content: "We must learn Vue js...",
        isCompleted: true,
      },
      {
        id: 3,
        title: "Learn Mongo DB",
        content: "We must learn Mongo DB...",
        isCompleted: false,
      },
      {
        id: 4,
        title: "Learn Express JS",
        content: "We must learn Express JS...",
        isCompleted: false,
      },
    ];
    this.todoListItems = todoListItems;
    this.countingTaskRemaining(todoListItems);
  },

  methods: {
    addNewTodo() {
      if (this.title === "" || this.content === "") {
        this.successAlert = false;
        this.dangerAlert = true;
        return;
      }
      // add new todo to the todo list
      const todoLenght = this.todoListItems.length;
      this.todoListItems.push({
        id: todoLenght + 1,
        title: this.title,
        content: this.content,
        isCompleted: false,
      });

      //clear todo form
      this.title = "";
      this.content = "";
      this.dangerAlert = false;
      this.successAlert = true;

      //refresh todo list data
      this.countingTaskRemaining(this.todoListItems);
    },

    // count  not completed task
    countingTaskRemaining(todoListItems) {
      this.taskRemaining = 0;
      for (let task of todoListItems) {
        if (task.isCompleted === false) {
          this.taskRemaining++;
        }
      }
    },

    deleteTodo(id) {
      // find todo by id and remove from the current todo list
      this.todoListItems = this.todoListItems.filter((todo) => todo.id !== id);
      this.countingTaskRemaining(this.todoListItems);
    },
    markToComplete(id) {
      // find  todo by Id and update completed status
      let todo = this.todoListItems.find((todo) => todo.id === id);
      !todo.isCompleted
        ? (todo.isCompleted = true)
        : (todo.isCompleted = false);
      // refresh todo list data
      this.countingTaskRemaining(this.todoListItems);
    },

    editTodo({ id, title, content }) {
      //pass todo content to the editing modal
      this.modalStatus = true;
      this.modalId = id;
      this.modalTitle = title;
      this.modalContent = content;
    },
    saveTodo({ id, title, content }) {
      //save todo update
      let todo = this.todoListItems.find((todo) => todo.id === id);
      todo.title = title;
      todo.content = content;
    },
  },
};
</script>

<style lang="scss" scoped>
.todo-list-main {
  display: flex;
  align-items: center;
  flex-direction: column;
  margin-top: 2rem;
  margin-bottom: 2rem;
}
.todo-list-add {
  display: flex;
  flex-direction: column;
}
.todo-form-add-element {
  margin: 0.5rem 0;
}
.alert {
  position: fixed;
  bottom: 0.1em;
  right: 1em;
}
.todo-list-container {
  width: 30rem;
  margin: 0.5rem;
}

@media screen and (max-width: 450px) {
  .todo-list-container {
    width: 90%;
  }
}
</style>

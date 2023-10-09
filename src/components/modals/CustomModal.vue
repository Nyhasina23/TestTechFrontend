<template>
  <Teleport to="body">
    <div class="todo-modal" v-show="openModal">
      <h5>Edit - {{ titleValue }}</h5>
      <CustomForm
        class="custom-form"
        :type="'text'"
        :modelValue="titleValue"
        v-model="title"
      />
      <CustomForm
        class="custom-form"
        :type="'text'"
        :modelValue="contentValue"
        v-model="content"
      />
      <CustomButton
        :variant="'primary'"
        :btnText="'Save'"
        @click="saveTodo(id)"
        class="save-btn"
      />
      <CustomButton :btnText="'close'" @click="closeModal" />
    </div>
    <div v-show="openModal" class="modal-overlay"></div>
  </Teleport>
</template>

<script>
import { Teleport } from "vue";
import CustomButton from "../base/CustomButton.vue";
import CustomForm from "../base/CustomForm.vue";

export default {
  name: "CutomModal",
  props: {
    id: String,
    titleValue: String,
    contentValue: String,
    openModal: Boolean,
  },
  components: { CustomForm, CustomButton, Teleport },

  data() {
    return {
      title: null,
      content: null,
    };
  },
  methods: {
    saveTodo(id) {
      this.$emit("saveTodo", {
        id,
        title: this.title ? this.title : this.titleValue,
        content: this.content ? this.content : this.contentValue,
      });
      if (this.title) {
        this.title = "";
      }

      if (this.content) {
        this.content = "";
      }

      // close modal after submission
      this.closeModal();
    },
    closeModal() {
      this.$emit("closeModal", false);
    },
  },
};
</script>

<style lang="scss" scoped>
.custom-form {
  margin: 1em 0;
}

.save-btn {
  margin-right: 0.5em;
}

.todo-modal {
  z-index: 100;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 2rem;
  border-radius: 0.5rem;
  background: whitesmoke;
  backdrop-filter: blur(20px);
  box-shadow: -1px 3px 5px 0px rgba(0, 0, 0, 0.75);
  -webkit-box-shadow: -1px 3px 5px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: -1px 3px 5px 0px rgba(0, 0, 0, 0.75);
}

.modal-overlay {
  background: rgba(0, 0, 0, 0.486);
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
}
</style>

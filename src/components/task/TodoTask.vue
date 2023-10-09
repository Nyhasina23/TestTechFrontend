<template>
  <BCardGroup class="b-card-group" deck>
    <BCard
      class="b-card"
      :border-variant="isCompleted ? 'success' : 'primary'"
      :header="isCompleted ? title + ' - completed' : title + ''"
      :header-bg-variant="isCompleted ? 'success' : 'primary'"
      header-text-variant="white"
      align="center"
    >
      <BCardText>{{ content }}</BCardText>
      <div class="icon-btn">
        <svg
          width="24"
          class="svg-btn"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          @click="editTodo(id, title, content)"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="m3.99 16.854-1.314 3.504a.75.75 0 0 0 .966.965l3.503-1.314a3 3 0 0 0 1.068-.687L18.36 9.175s-.354-1.061-1.414-2.122c-1.06-1.06-2.122-1.414-2.122-1.414L4.677 15.786a3 3 0 0 0-.687 1.068zm12.249-12.63 1.383-1.383c.248-.248.579-.406.925-.348.487.08 1.232.322 1.934 1.025.703.703.945 1.447 1.025 1.934.058.346-.1.677-.348.925L19.774 7.76s-.353-1.06-1.414-2.12c-1.06-1.062-2.121-1.415-2.121-1.415z"
            fill="rgb(41, 38, 252)"
          />
        </svg>
        <svg
          :fill="isCompleted ? 'rgb(26, 161, 26)' : 'rgb(41, 38, 252)'"
          width="24"
          class="svg-btn"
          height="24"
          viewBox="0 0 1920 1920"
          xmlns="http://www.w3.org/2000/svg"
          @click="markToComplete(id)"
        >
          <path
            d="M854.344 1317.685 503.209 966.55l79.85-79.85 271.285 271.285 520.207-520.32 79.849 79.962-600.056 600.057ZM960.056 0c-529.355 0-960 430.645-960 960s430.645 960 960 960c529.243 0 960-430.645 960-960S1489.3 0 960.056 0Z"
            fill-rule="rgb(26, 161, 26)"
          />
        </svg>
        <svg
          width="24"
          class="svg-btn"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          @click="deleteTodo(id)"
        >
          <path
            d="M2.75 6.16667C2.75 5.70644 3.09538 5.33335 3.52143 5.33335L6.18567 5.3329C6.71502 5.31841 7.18202 4.95482 7.36214 4.41691C7.36688 4.40277 7.37232 4.38532 7.39185 4.32203L7.50665 3.94993C7.5769 3.72179 7.6381 3.52303 7.72375 3.34536C8.06209 2.64349 8.68808 2.1561 9.41147 2.03132C9.59457 1.99973 9.78848 1.99987 10.0111 2.00002H13.4891C13.7117 1.99987 13.9056 1.99973 14.0887 2.03132C14.8121 2.1561 15.4381 2.64349 15.7764 3.34536C15.8621 3.52303 15.9233 3.72179 15.9935 3.94993L16.1083 4.32203C16.1279 4.38532 16.1333 4.40277 16.138 4.41691C16.3182 4.95482 16.8778 5.31886 17.4071 5.33335H19.9786C20.4046 5.33335 20.75 5.70644 20.75 6.16667C20.75 6.62691 20.4046 7 19.9786 7H3.52143C3.09538 7 2.75 6.62691 2.75 6.16667Z"
            fill="rgb(255, 54, 54)"
          />
          <path
            d="M11.6068 21.9998H12.3937C15.1012 21.9998 16.4549 21.9998 17.3351 21.1366C18.2153 20.2734 18.3054 18.8575 18.4855 16.0256L18.745 11.945C18.8427 10.4085 18.8916 9.6402 18.45 9.15335C18.0084 8.6665 17.2628 8.6665 15.7714 8.6665H8.22905C6.73771 8.6665 5.99204 8.6665 5.55047 9.15335C5.10891 9.6402 5.15777 10.4085 5.25549 11.945L5.515 16.0256C5.6951 18.8575 5.78515 20.2734 6.66534 21.1366C7.54553 21.9998 8.89927 21.9998 11.6068 21.9998Z"
            fill="rgb(255, 54, 54)"
          />
        </svg>
      </div>
    </BCard>
  </BCardGroup>
</template>

<script>
export default {
  name: "TodoTask",
  components: {},
  props: {
    id: Number,
    title: String,
    content: String,
    isCompleted: Boolean,
  },
  methods: {
    deleteTodo(id) {
      // check user confirmation
      const alertChecking = confirm(
        "Are you sure to delete " + this.title + " ?"
      );
      if (alertChecking == true) {
        this.$emit("deleteTodo", id);
      }
    },
    markToComplete(id) {
      // mark task to complete
      this.$emit("markToComplete", id);
    },
    editTodo(id, title, content) {
      // edit to do task
      this.$emit("editTodo", { id, title, content });
    },
  },
};
</script>

<style lang="scss" scoped>
.b-card-group {
  display: flex;
  flex-direction: column;
}
.b-card {
  margin: 1rem 0;
}
.svg-btn {
  cursor: pointer;
}
.icon-btn {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>

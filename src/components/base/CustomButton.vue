<template>
  <BButton pill :variant="variant"> {{ btnText }}</BButton>
</template>

<script>
export default {
  name: "CustomButton",
  props: {
    btnText: Element,
    variant: String,
  },
  setup() {
    return {};
  },
};
</script>

<style lang="scss" scoped></style>

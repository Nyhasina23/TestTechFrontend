<template>
  <div>
    <BForm>
      <BFormInput
        :type="type"
        :placeholder="placeholder"
        :value="modelValue"
        @input="updateValue($event)"
      />
    </BForm>
  </div>
</template>

<script>
export default {
  name: "CustomForm",
  props: {
    type: String,
    placeholder: String,
    modelValue: String,
  },
  methods: {
    updateValue(event) {
      //update input value on change
      this.$emit("update:modelValue", event);
    },
  },
};
</script>

<style lang="scss" scoped></style>

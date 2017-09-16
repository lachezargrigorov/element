<template>
  <label
    class="el-radio-button"
    :class="[
      size ? 'el-radio-button--' + size : '',
      { 'is-active': model == value },
      { 'is-disabled': isDisabled }
    ]"
  >
    <input
      class="el-radio-button__orig-radio"
      :value="value"
      type="radio"
      v-model="model"
      :name="name"
      :disabled="isDisabled">
    <span class="el-radio-button__inner" :style="model == value ? activeStyle : null">
      <slot></slot>
      <template v-if="!$slots.default">{{label}}</template>
    </span>
  </label>
</template>
<script>
  export default {
    name: 'ElRadioButton',

    props: {
      value: {},
      label: {},
      disabled: Boolean,
      name: String
    },
    computed: {
        model: {
        get() {
          return this._radioGroup.value;
        },
        set(value) {
          this._radioGroup.$emit('input', value);
        }
      },
      _radioGroup() {
        let parent = this.$parent;
        while (parent) {
          if (parent.$options.componentName !== 'ElRadioGroup') {
            parent = parent.$parent;
          } else {
            return parent;
          }
        }
        return false;
      },
      activeStyle() {
        return {
          backgroundColor: this._radioGroup.fill || '',
          borderColor: this._radioGroup.fill || '',
          boxShadow: this._radioGroup.fill ? `-1px 0 0 0 ${this._radioGroup.fill}` : '',
          color: this._radioGroup.textColor || ''
        };
      },
      size() {
        return this._radioGroup.size;
      },
      isDisabled() {
        return this.disabled || this._radioGroup.disabled;
      }
    }
  };
</script>

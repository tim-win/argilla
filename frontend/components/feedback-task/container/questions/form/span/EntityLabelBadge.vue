<template>
  <div class="input-button">
    <input
      ref="inputRef"
      type="checkbox"
      :name="option.text"
      :id="option.id"
      :data-keyboard="keyboards[option.id]"
      v-model="option.isSelected"
      @change="$emit('on-selected')"
      @focus="$emit('on-focus')"
      @keydown.tab="$emit('on-expand-labels-on-tab')"
    />
    <label
      class="label-text"
      :class="[
        {
          'label-active': option.isSelected,
        },
        `label-${option.id}`,
      ]"
      :for="option.id"
      :title="option.text"
    >
      <span
        class="key"
        v-if="showShortcutsHelper"
        v-text="keyboards[option.id]"
      />
      <span>{{ option.text }}</span>
    </label>
  </div>
</template>

<script>
export default {
  props: {
    option: {
      type: Object,
      required: true,
    },
    showShortcutsHelper: {
      type: Boolean,
      default: false,
    },
    keyboards: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    optionColor() {
      return this.option.color;
    },
    optionDarkColor() {
      return this.optionColor.palette.dark;
    },
    optionLightColor() {
      return this.optionColor.palette.light;
    },
    optionVeryLightColor() {
      return this.optionColor.palette.veryLight;
    },
  },
};
</script>

<style lang="scss" scoped>
$label-color: palette(grey, 700);

input[type="checkbox"] {
  @extend %visuallyhidden;
  &:focus {
    & + .label-text {
      outline: 2px solid v-bind("optionDarkColor");
    }
  }
}
.input-button {
  input[type="checkbox"] {
    &:focus:not(:focus-visible) {
      & + .label-text {
        outline: none;
        &.label-active {
          outline: none;
        }
      }
    }
  }
}

.label-text {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: $base-space;
  width: 100%;
  min-height: $base-space * 4;
  min-width: 50px;
  text-align: center;
  padding-inline: $base-space;
  background: v-bind(optionVeryLightColor);
  color: v-bind(optionDarkColor);
  font-weight: 500;
  outline: none;
  border: 2px solid transparent;
  border-radius: $border-radius;
  cursor: pointer;
  span {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    min-width: 0;
    &:hover {
      direction: rtl;
    }
  }
  &:not(.label-active):hover {
    background: v-bind(optionLightColor);
  }
  &.label-active {
    background: v-bind(optionColor);
    color: $black-87;
  }
}

.key {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  height: $base-space * 2;
  aspect-ratio: 1;
  border-radius: $border-radius;
  border-width: 1px 1px 3px 1px;
  border-color: $black-20;
  border-style: solid;
  box-sizing: content-box;
  color: $black-87;
  background: palette(grey, 700);
  @include font-size(11px);
  font-family: monospace, monospace;
}
</style>

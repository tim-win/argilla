<template>
  <div class="title-area --body1">
    <BaseTooltip
      v-if="question.suggestion"
      :text="suggestionTooltipText"
      position="left"
      minimalist
    >
      <svgicon
        class="title-area__suggestion-icon"
        name="suggestion"
        width="14"
        height="14"
      />
    </BaseTooltip>
    <span
      class="suggestion-info"
      v-text="question.title"
      v-required-field="{ show: question.isRequired, color: 'red' }"
    />
    <BaseIconWithBadge
      class="icon-info"
      v-if="showIcon"
      icon="info"
      :id="`${question.id}QuestionHeader`"
      :show-badge="false"
      iconColor="#acacac"
      badge-vertical-position="top"
      badge-horizontal-position="right"
      badge-border-color="white"
      v-tooltip="{
        content: tooltipMessage,
        open: openTooltip,
        backgroundColor: '#FFF',
      }"
    />
  </div>
</template>

<script>
import "assets/icons/info";
import "assets/icons/suggestion";

export default {
  name: "QuestionHeader",
  props: {
    question: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      tooltipMessage: this.question.description,
      openTooltip: false,
      timer: null,
    };
  },
  computed: {
    showIcon() {
      return !!this.question.description?.length;
    },
    suggestionTooltipText() {
      const qAgent = this.question.suggestion.agent;
      const qScore = this.question.suggestion.score;
      return this.$t("suggestion.tooltip", {
        agent: qAgent ? this.$t("suggestion.agent", { agent: qAgent }) : "",
        score: qScore ? this.$t("suggestion.score", { score: qScore }) : "",
      });
    },
  },
  watch: {
    "question.description"() {
      if (this.timer) clearTimeout(this.timer);
      this.openTooltip = true;
      this.tooltipMessage = this.question.description;

      this.timer = setTimeout(() => {
        this.openTooltip = false;
      }, 2000);
    },
  },
};
</script>

<style lang="scss" scoped>
.title-area {
  color: $black-87;
  font-weight: 500;
  &__suggestion-icon {
    cursor: default;
    fill: $black-54;
  }
}

.icon {
  color: $black-37;
}

.info-icon {
  display: flex;
  flex-basis: 37px;
}

span {
  word-break: break-word;
  line-height: 1.2em;
}

.icon-info {
  display: inline-flex;
  width: 20px;
  height: 20px;
  margin: 0;
  padding: 0;
  vertical-align: middle;
  &[data-title] {
    position: relative;
    overflow: visible;
    &:before,
    &:after {
      margin-top: 0;
    }
  }
}
</style>

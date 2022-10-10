<template>
  <button v-bind="$attrs" class="button">
    <span class="button-content">
      {{ text }}
    </span>
  </button>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      required: true
    }
  }
}
</script>

<style>
.button {
  padding: 4px 12px;
  border: none;
  border-radius: 2px;
  background: var(--color-primary-blue2);
  color: var(--color-foreground3);
}

.button:hover {
  background: var(--color-primary-blue3);
}

.button:active {
  background: var(--color-primary-blue1);
}

.button:disabled {
  background: var(--color-primary-blue1);
}

.button:disabled .button-content {
  color: var(--app-foreground1);
  cursor: not-allowed;
}
</style>

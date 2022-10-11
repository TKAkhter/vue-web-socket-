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
.button-row {
  display: flex;
}

.button-row input,
.button-row button,
button {
  font-family: 'Urbanist', sans-serif;
  padding: 15px;
  border: 0px;
  background: #002244;
  color: #fff;
  outline: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 5px;
  font-size: 18px;
}

button * {
  color: #fff;
}

</style>

<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <div class="greetings">
    <h1 class="grey">{{ msg }}</h1>
    <h3>
      <a href="http://" target="_blank">Sanjana</a> ❤
      <a href="http://" target="_blank">Max</a>
      Begin Their Journey Together
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 600;
  font-size: 2rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>

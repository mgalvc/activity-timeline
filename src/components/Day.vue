<template>
  <div id="day-box" :class="[`level-${level}`]"></div>
</template>

<script>

export default {
  name: 'Day',
  props: {
    level: {
      type: Number,
      default: 0
    }
  }
}
</script>

<style scoped>
#day-box {
  width: 10px;
  height: 10px;
  border: 1px solid #d9d9d9;
  border-radius: 2px;
}

.level-0 {
  background-color: #ebedf0;
}

.level-1 {
  background-color: #9be9a8;
}

.level-2 {
  background-color: #40c463;
}

.level-3 {
  background-color: #30a14e;
}

.level-4 {
  background-color: #216e39;
}
</style>
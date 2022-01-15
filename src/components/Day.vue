<template>
  <fragment>
    <Popover v-if="date">
      <template slot="content">
        {{ contributions }} contributions on {{ parsedDate }}
      </template>
      <div class="container">
        <div
          :class="[`level-${level}`, 'is-bordered', 'box']"
        ></div>
      </div>
    </Popover>
    <div v-else class="container">
      <div class="box"></div>
    </div>
  </fragment>
</template>

<script>
import { Popover } from "ant-design-vue"
import { format } from 'date-fns'

export default {
  name: "Day",
  components: {
    Popover,
  },
  props: {
    level: {
      type: Number,
      default: 0,
    },
    date: {
      type: String,
      default: undefined,
    },
    contributions: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    parsedDate() {
      return format(new Date(this.date), 'MMM dd, yyyy')
    }
  }
};
</script>

<style scoped>
.container {
  display: inline-block;
  padding: 2px;
}

.box {
  width: 10px;
  height: 10px;
}

.is-bordered {
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
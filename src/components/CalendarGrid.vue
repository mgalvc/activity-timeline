<template>
  <div id="container">
    <div class="days-row" v-for="dayOfWeek of grid" :key="dayOfWeek[0]">
      <Day :level="parseInt(Math.random()*5)" :date="date" v-for="date of dayOfWeek" :key="date"/>
    </div>
  </div>
</template>

<script>
import Day from './Day.vue'

export default {
  name: 'CalendarGrid',
  components: {
    Day
  },
  data: () => ({
    grid: [],
  }),
  mounted() {
    const year = 2022;
    const firstDay = new Date(year, 0, 1);
    const lastDay = new Date(year, 11, 31);

    const weeks = [];
    let currentWeek = 0;

    for (
      let currentDay = firstDay;
      currentDay <= lastDay;
      currentDay.setDate(currentDay.getDate() + 1)
    ) {
      const weekDay = currentDay.getDay();

      if (weeks.length <= currentWeek) {
        weeks.push(Array(7).fill(undefined));
      }

      weeks[currentWeek][weekDay] = currentDay.toISOString();

      if (weekDay == 6) {
        currentWeek++;
      }
    }

    for (let dayOfWeek = 0; dayOfWeek < 7; dayOfWeek++) {
      this.grid.push(Array(weeks.length).fill(undefined));

      for (const [week] of this.grid[dayOfWeek].entries()) {
        this.grid[dayOfWeek][week] = weeks[week][dayOfWeek];
      }
    }
  },
};
</script>

<style scoped>
.days-row {
  line-height: 0.5;
}

#container {
  text-align: center;
  margin-top: 100px;
}
</style>
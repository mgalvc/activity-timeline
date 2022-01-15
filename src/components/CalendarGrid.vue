<template>
  <div id="container">
    <div class="days-row" v-for="dayOfWeek of grid" :key="dayOfWeek[0]">
      <Day
        :level="contributions[date] ? contributions[date].level : 0"
        :date="date"
        :contributions="contributions[date] ? contributions[date].count : 0"
        v-for="date of dayOfWeek"
        :key="date"
      />
    </div>
  </div>
</template>

<script>
import Day from "./Day.vue";
import rawData from "../resources/github-dataset.json";

export default {
  name: "CalendarGrid",
  components: {
    Day,
  },
  data: () => ({
    grid: [],
    year: 2012,
    contributions: {},
  }),
  methods: {
    generateGrid() {
      const firstDay = new Date(this.year, 0, 1);
      const lastDay = new Date(this.year, 11, 31);

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
    processContributions() {
      const contributionsOfYear = rawData.filter(
        (c) => new Date(c.created_at).getFullYear() == this.year
      );
      let contributionsMax = 0;
      const contributionsCount = {};
      contributionsOfYear.forEach((c) => {
        const createdAt = new Date(c.created_at);
        const day = new Date(
          createdAt.getFullYear(),
          createdAt.getMonth(),
          createdAt.getDate()
        ).toISOString();

        if (!contributionsCount[day]) {
          contributionsCount[day] = 0;
        }

        contributionsCount[day]++;

        if (contributionsCount[day] > contributionsMax)
          contributionsMax = contributionsCount[day];
      });

      const levels = [
        1,
        parseInt(contributionsMax * 0.3),
        parseInt(contributionsMax * 0.6),
        parseInt(contributionsMax * 1),
      ];

      this.contributions = Object.entries(contributionsCount).reduce(
        (result, current) => {
          const [date, count] = current;
          let level = 0;

          if (count >= levels[3]) level = 4;
          else if (count >= levels[2]) level = 3;
          else if (count >= levels[1]) level = 2;
          else if (count >= levels[0]) level = 1;

          return { ...result, [date]: { count, level } };
        },
        {}
      );
    },
  },
  mounted() {
    this.processContributions();
    this.generateGrid();
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
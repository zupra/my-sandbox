<template>
  <div class="ganttArea">
    <div class="gantt" :style="columnsStyleObject">
      <!-- МАСКА СЕТКИ -->
      <div class="mask" :style="columnsStyleObject">
        <div
          v-for="(_, idx) in Array(columns - 1)"
          :key="`1_${idx}`"
          class="mask__yLline"
        >
          <div
            v-if="idx === currMonth"
            class="currMonth-yLine"
            :style="`margin-left: ${(100 / 30) * NOW.getDate()}%`"
          >
            <div class="currMonth-currDate">
              {{
                new Date(NOW).toLocaleDateString('ru-RU', {
                  day: 'numeric',
                  month: 'long',
                })
              }}
            </div>
          </div>
        </div>
      </div>

      <div
        v-for="(It, idx) in mapOfYears"
        :key="`2_${idx}`"
        class="th_year"
        :style="`grid-column: ${It.start} / ${It.end}`"
      >
        {{ It.title }}
      </div>
      <div
        v-for="(th, idx) in _months"
        :key="`3_${idx}`"
        class="th_month"
        :class="{ currMonth_text: idx === currMonth }"
      >
        {{ th }}
      </div>
      <GanttItem
        v-for="(It, idx) in data"
        :key="`4_${idx}`"
        :It="It"
        :listOfYears="listOfYears"
      ></GanttItem>
    </div>
  </div>
</template>

<script>
const months = [
  'Янв',
  'Фев',
  'Мар',
  'Апр',
  'Май',
  'Июн',
  'Июл',
  'Авг',
  'Сен',
  'Окт',
  'Ноя',
  'Дек',
]

export default {
  props: {
    data: {
      type: Array,
      default: () => [
        {
          type: 'blue',
          title: 'Сходить в магазин',
          start: '2021-02-25',
          end: '2021-10-15',
        },
      ],
    },
  },
  data() {
    return {
      NOW: new Date(),
      months,
    }
  },
  computed: {
    listOfYears() {
      return [
        ...new Set(
          this.data.reduce(
            (acc, el) => [...acc, el.start.getFullYear(), el.end.getFullYear()],
            []
          )
        ),
      ].sort()
    },
    columns() {
      return this.listOfYears.length * 12
    },
    columnsStyleObject() {
      return {
        gridTemplateColumns: `repeat(${this.columns}, 62px)`,
      }
    },
    mapOfYears() {
      return this.listOfYears.map((el, idx) => ({
        title: el,
        start: idx * 12 + 1,
        end: idx * 12 + 1 + 12,
      }))
    },
    _months() {
      // months.concat(months)...
      return this.listOfYears.reduce((acc, el) => [...acc, ...this.months], [])
    },
    currMonth() {
      // номер колонки
      return (
        this.listOfYears.findIndex((el) => el === this.NOW.getFullYear()) * 12 +
        this.NOW.getMonth()
      )
    },
  },
}
</script>

<style>
.ganttArea {
  scroll-snap-type: x mandatory;
  overflow-x: scroll;
}
.gantt,
.mask {
  display: grid;
}
.gantt {
  grid-gap: 10px 0px;
  position: relative;
  padding-bottom: 0.5rem;
}
.mask {
  position: absolute;
  top: 2rem;
  width: 100%;
  bottom: 0;
}
.mask__yLline {
  border-right: 1px solid rgba(102, 102, 102, 0.2);
}
.mask__yLline:nth-child(12n + 13) {
  border-left: 1px solid;
}

.th_year,
.th_month {
  text-align: center;
}
.th_year {
  font-size: 1.2rem;
}
.th_month {
  border-bottom: 1px solid rgba(102, 102, 102, 0.2);
  line-height: 2.4rem;
  color: #777;
}

.currMonth-yLine {
  border-left: 1px dashed #ffa500;
  height: 100%;
  margin-top: 1.8em;
}
.currMonth-currDate {
  width: max-content;
  padding: 2px 4px;
  background: #ffa500;
  color: #fff;
  font-size: 0.7em;
  transform: translateX(-50%);
  border-radius: 4px;
  text-align: center;
}
</style>

<template lang="pug">
div
  div
    //- pre {{ listOfYears }}
    //- pre {{ mapOfYears }}
    //- pre {{ columns }}
    //- pre {{_months}}
  PreData
    pre {{ DATA }}

  .ganttArea
    .gantt(:style='columnsStyleObject')
      .mask(:style='columnsStyleObject')
        //- на 1 меньше 
        .vertical_line(v-for='(_, idx) in Array(columns-1)')
          .currMonth_line(
            v-if='idx === currMonth',
            :style='`margin-left: ${(100 / 30) * NOW.getDate()}%`'
          )
            .currMonth_currDate {{ new Date(NOW).toLocaleDateString("ru-RU", { day: "numeric", month: "long" }) }}

      .TH(
        v-for='It in mapOfYears',
        :style='`grid-column: ${It.start} / ${It.end}`'
      ) {{ It.title }}

      //-
        in months.concat(months).concat(months)
        TODO месяцы на каждый год
      .th(
        v-for='(th, idx) in _months',
        :class='{ currMonth_text: idx === currMonth }'
      ) {{ th }}

      GanttItem(v-for='It in DATA', :It='It', :listOfYears='listOfYears')
</template>

<script>
import faker from 'faker/locale/ru'
function randomInt(min, max) {
  let rand = min - 0.5 + Math.random() * (max - min + 1)
  rand = Math.round(rand)
  return rand
}
const days = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
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

const types = ['blue', 'red', 'green', 'yellow', 'purple']

/*
const DATA = [
  {
    type: 'blue',
    title: 'Сходить в магазин',
    start: '2021-02-25',
    end: '2021-10-15',
  },
]
*/

const DATA = Array.from({ length: randomInt(5, 10) }, (_, idx) => ({
  type: types[randomInt(0, 3)],
  title: faker.lorem.sentence(),
  start: faker.date.past(), // .substr(0, 10),
  end: faker.date.future(), // soon()
}))

export default {
  data() {
    return {
      DATA,
      NOW: new Date(),
      // days,
      months,
    }
  },
  computed: {
    listOfYears() {
      return [
        ...new Set(
          DATA.reduce(
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
      return this.listOfYears.map((el, idx, arr) => ({
        title: el,
        start: idx * 12 + 1,
        end: idx * 12 + 1 + 12,
      }))
    },
    _months() {
      return this.listOfYears.reduce((acc, el) => [...acc, ...this.months], [])
    },
    // currYear() {
    //   return this.NOW.getFullYear()
    // },
    currMonth() {
      // this.NOW.getMonth()
      return (
        this.listOfYears.findIndex((el) => el === this.NOW.getFullYear()) * 12 +
        this.NOW.getMonth()
      )
    },
  },
}
</script>

<style lang="stylus">
.ganttArea
  // margin 1rem
  scroll-snap-type: x mandatory;
  overflow-x: scroll;

.gantt, .mask
  display: grid;
  grid-template-columns: repeat(24, 60px);
  // min-width: 600px;

.gantt
  grid-gap: 10px 0px;
  position: relative;
  padding-bottom: 0.5rem;

.mask
  position: absolute;
  // z-index -1
  top: 2rem;
  width: 100%;
  height: 90%; // calc(100vh - 2em);
  // grid-auto-rows: 90vh // minmax(100vw, auto);

.TH
  text-align: center;
  font-size: 1.2rem;

.th
  text-align: center;
  border-bottom: 1px solid rgba(#666, 0.2);
  line-height: 2.4rem;
  color: #777;

.vertical_line
  border-right: 1px solid rgba(#666, 0.2);

  &:nth-child(12n+13)
    border-left: 1px solid;

.currMonth
  &_line
    border-left: 1px dashed orange;
    height: 100%;
    margin-top: 1.8em;

  &_currDate
    background: orange;
    color: #FFF;
    font-size: 0.7em;
    transform: translateX(-50%);
    padding: 2px 0;
    border-radius: 4px;
    text-align: center;

.currMonth_text
  font-weight: bold;
  color: #000;
</style>

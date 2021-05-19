<template lang="pug">
.FLEX
  .taskList
    .taskList__row(
      v-for="task in tasks"
    ) {{task}}

  .ganttArea
    .gantt(:style='columnsStyleObject')
      // МАСКА СЕТКИ
      .mask(:style='columnsStyleObject')
        .mask__yLline(
          v-for="(_, idx) in Array(columns - 1)"
          :key="`1_${idx}`"
        )
          // currMonth
          .currMonth-yLine(
            v-if="idx === currMonth"
            :style="`margin-left: ${(100 / 30) * NOW.getDate()}%`"
          )
            .currMonth-currDate {{ new Date(NOW).toLocaleDateString("ru-RU", { day: "numeric", month: "long" }) }}

      //
      .th_year(
        v-for="(It, idx) in mapOfYears"
        :key="`2_${idx}`"
        :style="`grid-column: ${It.start} / ${It.end}`"
      ) {{ It.title }}
      .th_month(
        v-for="(th, idx) in _months"
        :key="`3_${idx}`"
        :class="{ currMonth_text: idx === currMonth }"
      ) {{ th }}

      template(
        v-for="(It, idx) in data"
      )
        SecondSItem(
          :key="`4_${idx}`"
          :It="It"
          :listOfYears="listOfYears"
        )
        .ROW

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
    //
    tasks() {
      return this.data.map(el => el.title)
    },

    listOfYears() {

      // return [2021,2022,2023]

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


<style lang="stylus" scoped>

.FLEX
  display: flex;
.taskList
  // margin-top 77px
  padding-top 77px
  border-right 2px solid #777
  &__row
    height 58px
    line-height 1
    width 400px
    border-bottom 1px solid rgba(#999, 0.3)
    padding-left 1rem
    display:flex
    align-items center
    color #222

.ROW
  grid-column: 1 / -1;
  border-bottom 1px solid rgba(#999, 0.1)







.ganttArea
  scroll-snap-type x mandatory
  overflow-x scroll
  overflow-y: hidden;

.gantt,
.mask
  display grid

.gantt
  grid-gap 10px 0px
  position relative
  padding-bottom 0.5rem

.mask
  position absolute
  top 2rem
  width 100%
  bottom 0

.mask__yLline
  border-right 1px solid rgba(102, 102, 102, 0.2)
  &:nth-child(12n
    & + 13)
      border-left 1px solid

.th_year,
.th_month
  text-align center

.th_year
  font-size 1.2rem

.th_month
  border-bottom 1px solid rgba(102, 102, 102, 0.2)
  line-height 2.4rem
  color #777

.currMonth-yLine
  border-left 1px dashed #ffa500
  height 100%
  margin-top 1.8em

.currMonth-currDate
  width max-content
  padding 2px 4px
  background #ffa500
  color #fff
  font-size 0.7em
  transform translateX(-50%)
  border-radius 4px
  text-align center
</style>

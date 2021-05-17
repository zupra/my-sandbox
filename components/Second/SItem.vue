<template lang="pug">
.It2(
  :class="It.type || 'default'",
  :style='styleObject',
  @mouseover='showTooltip = true',
  @mouseout='showTooltip = false',
  @mousemove='onMove'
)
  .It2__title {{ It.title }}
  // .It__period {{ new Date(It.start).toLocaleDateString("ru-RU", localeDateFormat) }} – {{ new Date(It.end).toLocaleDateString("ru-RU", localeDateFormat) }}
  //- <!-- pre {{ start }} - {{ end }}-->

  .Popover(
    v-if="showTooltip"
    :style="{ top: `${Y}px`, left: `${X}px`, transform: `translateY(-100%)` }"
  )
    .Popover__title {{ It.title }}
    small.Popover__period {{ new Date(It.start).toLocaleDateString('ru-RU', localeDateFormat) }} – {{ new Date(It.end).toLocaleDateString('ru-RU', localeDateFormat) }}

</template>


<script>
/*
{
  type: 'blue',
  title: 'Сходить в магазин',
  start: '2021-02-25',
  end: '2021-10-15',
}
*/

export default {
  props: {
    listOfYears: {
      type: Array,
      default: () => [new Date().getFullYear()],
    },
    It: {
      type: Object,
      default: () => ({
        type: 'blue',
        title: 'Сходить в магазин',
        start: '2021-02-25',
        end: '2021-10-15',
      }),
    },
    localeDateFormat: {
      type: Object,
      default: () => ({ day: '2-digit', month: 'short', year: 'numeric' }),
    },
  },
  data() {
    return {
      showTooltip: false,
      X: 0,
      Y: 0,
    }
  },
  computed: {
    start() {
      return (
        this.listOfYears.findIndex(
          (el) => el === new Date(this.It.start).getFullYear()
        ) *
          12 +
        new Date(this.It.start).getMonth() +
        1
      )
    },
    end() {
      return (
        this.listOfYears.findIndex(
          (el) => el === new Date(this.It.end).getFullYear()
        ) *
          12 +
        new Date(this.It.end).getMonth() +
        1
      )
    },
    styleObject() {
      return {
        gridColumn: `${this.start} / ${this.end}`,
        marginLeft: `${new Date(this.It.start).getDate() * 2}px`,
        marginRight: `-${new Date(this.It.end).getDate() * 2}px`,
      }
    },
  },
  methods: {
    onMove(e) {
      // console.info(e)
      const { offsetX, offsetY } = e
      this.X = offsetX + 20
      this.Y = offsetY + 20
      // this.X = clientX - 150
      // this.Y = clientY
    },
  },
}
</script>


<style lang="stylus">
.It2
  cursor: pointer;
  position: relative;
  display: grid;
  /* grid-template-rows: 2rem 1rem; */
  align-items: center;
  border-radius: 8px;
  /* padding: 0 1rem 0.3rem; */
  /* border-left: 0.5em solid transparent; */

  padding: .5rem;
  &:not(:last-of-type):before
    content: '';
    position: absolute;
    width 20px
    height 20px
    left -20px
    bottom -36px
    background-image url('https://icongr.am/clarity/child-arrow.svg?size=20&color=81abee')
  // &:last-of-type
  //   background #000 !important

.It2__title,
.It2__period {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}




.default {
  background: #81ABEE;
  color: #fff;
  /* font-size: .8rem */
}


.blue {
  border-left-color: #3d75e4;
  background: #e4edfb;
}
.red {
  border-left-color: #e14761;
  background: #fbe9ec;
}
.green {
  border-left-color: #57a003;
  background: #e3f3d8;
}
.yellow {
  border-left-color: #d06e0b;
  background: #fbecdd;
}
.purple {
  border-left-color: #9954f2;
  background: #efe8fc;
}



.Popover {
  color: #222;

  position: absolute;
  z-index: 10;
  box-shadow: 0 0.2em 2em -0.1em rgba(0,0,0,0.4);
  background: #f7f7f7;
  padding: 0.7em;
  border-radius: 8px;
  width: 300px;
  line-height: 1;
}
.Popover__title {
  font-size: 0.9em;
  padding-left: 0.5em;
  margin-bottom: 1em;
  border-left: 3px solid #57a003;
}
.Popover__period {
  font-size: 0.7em;
  padding: 0.3em 0.5em;
  background: #e4edfb;
  border-radius: 4px;
  color: #3d75e4;
}
</style>
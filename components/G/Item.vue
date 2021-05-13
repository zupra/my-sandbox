<template lang="pug">
.It(
  :class='It.type',
  :style='styleObject',
  @mouseover='showTooltip = true',
  @mouseout='showTooltip = false',
  @mousemove='onMove'
)
  .It_title {{ It.title }}
  small.It_period {{ new Date(It.start).toLocaleDateString("ru-RU", localeDateFormat) }} – {{ new Date(It.end).toLocaleDateString("ru-RU", localeDateFormat) }}

  // pre {{ start }} - {{ end }}

  .Popover(v-if='showTooltip', :style='{ top: `${Y}px`, left: `${X}px` }')
    .Popover_title {{ It.title }}
    small.Popover_period {{ new Date(It.start).toLocaleDateString("ru-RU", localeDateFormat) }} – {{ new Date(It.end).toLocaleDateString("ru-RU", localeDateFormat) }}
</template>

<script>
// type
// title
// start
// end
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
      // return new Date(this.It.start).getMonth() + 1
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
      // return new Date(this.It.end).getMonth() + 1
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
      console.log(e)
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
textOverflow()
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

.It
  cursor: pointer;
  position: relative;
  display: grid;
  grid-template-rows: 2rem 1rem;
  align-items: center;
  padding: 0 1rem 0.3rem;
  // background: #EEE;
  border-radius: 8px;
  border-left: 0.5em solid transparent;

  &_title, &_period
    textOverflow();

.blue
  &, small
    border-left-color: #3D75E4;

  background: #E4EDFB;

.red
  border-left-color: #E14761;
  background: #FBE9EC;

.green
  border-left-color: #57A003;
  background: #E3F3D8;

.yellow
  border-left-color: #D06E0B;
  background: #FBECDD;

.purple
  border-left-color: #9954F2;
  background: #EFE8FC;

// .half
// margin-left: calc((100% / 3 / 2));
// margin-right: calc((-100% / 3 / 2));
// .It:hover .Popover
// display: block;
.Popover
  // display: none;
  position: absolute;
  z-index: 10;
  // left: 90%;
  box-shadow: 0 0.2em 2em -0.1em rgba(#000, 0.4);
  background: #f7f7f7;
  padding: 0.7em;
  border-radius: 8px;
  width: 300px;
  line-height: 1;

  &_title
    font-size: 0.9em;
    padding-left: 0.5em;
    margin-bottom: 1em;
    border-left: 3px solid #57A003;

  &_period
    font-size: 0.7em;
    padding: 0.3em 0.5em;
    background: #E4EDFB;
    border-radius: 4px;
    color: #3D75E4;
</style>
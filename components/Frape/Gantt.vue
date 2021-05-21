<template>
  <div ref="gantt"> 
    <!-- <svg ref="gantt" /> -->
  </div>
</template>

<script>
import Gantt from './src/index.js'
// import Gantt from 'frappe-gantt'
export default {
  name: 'FrappeGantt',
  props: {
    viewMode: {
      type: String,
      required: false,
      default: 'Month',
    },
    tasks: {
      type: Array,
      required: true,
      default: [],
    },
  },
  data() {
    return {
      gantt: {},
    }
  },
  watch: {
    viewMode() {
      this.updateViewMode()
    },

    tasks() {
      this.updateTasks()
    },
  },
  mounted() {
    this.setupGanttChart()
    // console.log(this.gantt)
  },
  methods: {
    setupGanttChart() {
      this.gantt = new Gantt(this.$refs.gantt, this.tasks, {
        language: 'ru',
        // header_height: 10,
        // column_width: 30,
        // step: 24,
        // view_modes: ['Quarter Day', 'Half Day', 'Day', 'Week', 'Month'],
        // bar_height: 20,
        // bar_corner_radius: 3,
        // arrow_curve: 5,
        // padding: 18,
        // view_mode: 'Day',
        // date_format: 'YYYY-MM-DD',
        // custom_popup_html: null
        bar_corner_radius: 5,
        popup_trigger: 'mouseover',
        /*
        custom_popup_html(task) {
          // the task object will contain the updated
          // dates and progress value
          const end_date = task._end.format('MMM D')
          return `
          <div class="details-container">
            <h5>${task.name}</h5>
            <p>Expected to finish by ${end_date}</p>
            <p>${task.progress}% completed!</p>
          </div>
          `
        },
        */

        on_click: (task) => {
          this.$emit('task-updated', task)
        },

        on_date_change: (task, start, end) => {
          this.$emit('task-date-updated', { task, start, end })
        },

        on_progress_change: (task, progress) => {
          this.$emit('task-progress-updated', { task, progress })
        },

        // режим просмотра
        on_view_change: (mode) => {
          this.$emit('view-mode-updated', mode)
        },
      })

      this.updateTasks()
      this.updateViewMode()
    },

    updateViewMode() {
      this.gantt.change_view_mode(
        this.viewMode[0].toUpperCase() + this.viewMode.substring(1)
      )
    },

    updateTasks() {
      this.gantt.refresh(this.tasks)
    },
  },
}
</script>

<style>
/* @import '~/node_modules/frappe-gantt/dist/frappe-gantt.css'; */

.gantt .grid-background {
  fill: none;
}

.gantt .grid-header {
  fill: #ffffff;
  stroke: #e0e0e0;
  stroke-width: 1.4;
}

.gantt .grid-row {
  fill: #ffffff;
}

.gantt .grid-row:nth-child(even) {
  fill: #f5f5f5;
}

.gantt .row-line {
  stroke: #ebeff2;
}

.gantt .tick {
  stroke: #e0e0e0;
  stroke-width: 0.2;
}
.gantt .tick.thick {
  stroke-width: 0.4;
}

.gantt .today-highlight {
  fill: #fcf8e3;
  opacity: 0.5;
}

.gantt .arrow {
  fill: none;
  stroke: #666;
  stroke-width: 1.4;
}

.gantt .bar {
  fill: #b8c2cc;
  stroke: #8d99a6;
  stroke-width: 0;
  transition: stroke-width 0.3s ease;
  user-select: none;
}

.gantt .bar-progress {
  fill: #8dc95e;
}

.gantt .bar-invalid {
  fill: transparent;
  stroke: #8d99a6;
  stroke-width: 1;
  stroke-dasharray: 5;
}
.gantt .bar-invalid ~ .bar-label {
  fill: #555;
}

.gantt .bar-label {
  fill: #fff;
  dominant-baseline: central;
  text-anchor: middle;
  font-size: 12px;
  font-weight: bold;
}
.gantt .bar-label.big {
  fill: #555;
  text-anchor: start;
}

.gantt .handle {
  fill: #ddd;
  cursor: ew-resize;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease;
}

.gantt .bar-wrapper {
  cursor: pointer;
  outline: none;
}
.gantt .bar-wrapper:hover .bar {
  fill: #a9b5c1;
}
.gantt .bar-wrapper:hover .bar-progress {
  fill: #8dc95e;
}
.gantt .bar-wrapper:hover .handle {
  visibility: visible;
  opacity: 1;
}
.gantt .bar-wrapper.active .bar {
  fill: #a9b5c1;
}
.gantt .bar-wrapper.active .bar-progress {
  fill: #8dc95e;
}

.gantt .lower-text,
.gantt .upper-text {
  font-size: 12px;
  text-anchor: middle;
}

.gantt .upper-text {
  fill: #555;
  font-size: 18px;
}

.gantt .lower-text {
  fill: #333;
}

.gantt .hide {
  display: none;
}

.gantt-container {
  position: relative;
  overflow: auto;
  font-size: 12px;
}
.gantt-container .popup-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.8);
  padding: 0;
  color: #959da5;
  border-radius: 3px;
}
.gantt-container .popup-wrapper .title {
  /* border-bottom: 3px solid #a3a3ff; */
  padding: 10px;
}
.gantt-container .popup-wrapper .subtitle {
  padding: 10px;
  color: #dfe2e5;
}
.gantt-container .popup-wrapper .pointer {
  position: absolute;
  height: 5px;
  margin: 0 0 0 -5px;
  border: 5px solid transparent;
  border-top-color: rgba(0, 0, 0, 0.8);
}
</style>

<style lang="stylus">
.bar-main
  .bar
    fill: #81ABEE !important;

.bar-2
  .bar
    fill: #E14761 !important;

.bar-3
  .bar
    fill: #E9A35D !important;

.bar-4
  .bar
    fill: #D06E0B !important;
</style>

<template>
  <div class="FRAPPE">
    <!-- <a-divider orientation="left"> TEST с перетаскиванием </a-divider> -->

    <!-- 
    <a-button-group>
      <a-button
        :type="mode === 'day' ? 'primary' : ''"
        @click="demoViewMode('day')"
        >По дням</a-button
      >
      <a-button
        :type="mode === 'week' ? 'primary' : ''"
        @click="demoViewMode('week')"
        >По неделям</a-button
      >
      <a-button
        :type="mode === 'month' ? 'primary' : ''"
        @click="demoViewMode('month')"
        >По месяцам</a-button
      >
    </a-button-group>
    -->

    <div class="flex x_sb">

      <a-button type="primary" size="large" @click="showModal = true"
        ><a-icon type="plus" />Добавить</a-button
      >

      <!-- ['Quarter Day', 'Half Day', 'Day', 'Week', 'Month'] -->
      <a-button-group>
        <a-button
          v-for="(V, K, idx) in {
            ['Quarter Day']: 'четверть дня',
            ['Half Day']: '1/2 дня',
            day: 'По дням',
            week: 'По неделям',
            month: 'По месяцам',
          }"
          :key="idx"
          :type="mode === K ? 'primary' : ''"
          @click="demoViewMode(K)"
          >{{ V }}</a-button
        >
      </a-button-group>
    </div>

    <br />
    <br />

    <FrappeGantt
      class="frappe-gantt"
      :view-mode="mode"
      :tasks="tasks"
      @task-updated="debugEventLog.push($event)"
      @task-date-updated="debugEventLog.push($event)"
      @task-progress-updated="debugEventLog.push($event)"
    />
    <a-button type="primary" @click="addRandomTask"
      >addRandomTask()</a-button
    >

    <!-- debugEventLog -->
    <ul>
      <li v-for="event in debugEventLog" :key="event.id">
        {{ event }}
      </li>
    </ul>

    <a-modal v-model="showModal" title="Создать Задачу" on-ok="">
      <template slot="footer">
        <a-button key="back" @click="showModal = !showModal"> Закрыть </a-button>
        <a-button
          key="submit"
          type="primary"
          @click=""
        >
          Создать
        </a-button>
      </template>
    </a-modal>
  </div>
</template>

<script>
import FrappeGantt from '~/components/Frape/Gantt.vue'
// const uuidv5 = require('uuid/v5');
// import uuidv4 from 'uuid/v4';

const tasks = [
  {
    id: '1',
    name: 'БОЛЬШАЯ ЗАДАЧА',
    start: '2020-11-04',
    end: '2021-06-05',
    progress: 70,
    custom_class: 'bar-main',
  },

  {
    id: '12',
    name: 'С просрочкой',
    start: '2020-11-05',
    end: '2020-12-20',
    progress: 0,
    dependencies: '1',
    custom_class: 'bar-main',
  },
  {
    custom_index: 1,
    id: '22',
    name: 'Просрочка',
    start: '2020-12-21',
    end: '2021-01-10',
    progress: 0,
    dependencies: '',
    custom_class: 'bar-2',
  },
  {
    custom_index: 2,
    id: '32',
    name: 'Прогноз 1',
    start: '2020-11-05',
    end: '2021-02-10',
    progress: 0,
    // dependencies: '12',
    custom_class: 'bar-3',
  },
  {
    custom_index: 3,
    id: '42',
    name: 'Прогноз 2',
    start: '2020-11-05',
    end: '2021-03-10',
    progress: 0,
    dependencies: '',
    custom_class: 'bar-4',
  },

  {
    // custom_index: 2,
    id: '2',
    name: 'полить цветы',
    start: '2020-11-05',
    end: '2020-12-10',
    progress: 50,
    dependencies: '1',
  },
  {
    // custom_index: 3,
    id: '3',
    name: 'покосить траву',
    start: '2020-11-10',
    end: '2020-12-15',
    progress: 30,
    dependencies: '2',
  },
  {
    // custom_index: 4,
    id: '4',
    name: 'покрасить забор',
    start: '2020-11-05',
    end: '2020-12-10',
    progress: 40,
    dependencies: ['1', '2', '3'],
  },
]

export default {
  name: 'App',
  components: {
    FrappeGantt,
  },
  data() {
    return {
      showModal: false,
      //
      mode: 'month',
      tasks,
      debugEventLog: [],

      newTaskModel: {
        name: '',
        start: '',
        end: '',
        progress: 0, // Math.random() * 100,
        dependencies: '',
      }
    }
  },
  methods: {
    addRandomTask() {
      // const id = uuidv4();
      this.tasks.push({
        id: this.tasks.length + 1, //id,
        name: 'NAME', //id,
        start: '2021-04-20',
        end: '2021-06-20',
        progress: Math.random() * 100,
        dependencies: '1',
      })
    },
    addTask() {
      // const id = uuidv4();
      this.tasks.push({
        id: this.tasks.length + 1,
        ...this.newTaskModel
      })
    },
    demoViewMode(viewMode) {
      this.mode = viewMode
    },
  },
}
</script>


<style lang="stylus" scoped></style>
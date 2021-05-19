<template>
  <div class="FRAPPE">
    <a-divider orientation="left"> TEST с перетаскиванием </a-divider>

    <!-- 
    <a-button-group>
      <a-button
      v-for="(V,K) in {'day': 'По дням', 'week': 'По неделям', 'month': 'По месяцам'}"
      :type="mode === 'day' ? 'primary' : ''"
      @click="demoViewMode('day')"
      >{{V}}</a-button
    </a-button-group>
    -->

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


    <br />
    <br />

    <FrappeGantt
      class="frappe-gantt"
      :view-mode="mode"
      :tasks="tasks"
      @task-updated="debugEventLog.push($event)"
      @task-date-updated="debugEventLog.push($event)"
      @task-progress-change="debugEventLog.push($event)"
    />
    <!--
      TODO: When the Frappe Chart component emits task changes from user
      clicking or dragging make sure to then update the task list passed in via
      prop (App.vue)
    -->
    <a-button type="primary" size="large" @click="addRandomTask">Добавить</a-button>

    <!-- 
      These are the events being emitted by the Vue.js component wrapper for
      Frappe Gantt
    -->
    <ul>
      <li v-for="event in debugEventLog" :key="event.id">
        {{ event }}
      </li>
    </ul>
  </div>
</template>

<script>
import FrappeGantt from '~/components/Frape/Gantt.vue'
// const uuidv5 = require('uuid/v5');
// import uuidv4 from 'uuid/v4';

export default {
  name: 'App',
  components: {
    FrappeGantt,
  },
  data() {
    return {
      mode: 'month',
      tasks: [
        {
          id: '1',
          name: 'БОЛЬШАЯ ЗАДАЧА',
          start: '2020-04-04',
          end: '2021-04-05',
          progress: 70,
          custom_class: 'bar-main'
        },



        {
          id: '12',
          name: 'С просрочкой',
          start: '2020-04-05',
          end: '2020-05-10',
          progress: 0,
          dependencies: '1',
          custom_class: 'bar-main'
        },
        {
          custom_index: 1,
          id: '22',
          name: 'Просрочка',
          start: '2020-05-11',
          end: '2020-06-10',
          progress: 0,
          dependencies: '',
          custom_class: 'bar-2'
        },
        {
          custom_index: 1,
          id: '32',
          name: 'Прогноз 2',
          start: '2020-06-11',
          end: '2020-07-10',
          progress: 0,
          dependencies: '',
          custom_class: 'bar-3'
        },
        {
          custom_index: 1,
          id: '42',
          name: 'Прогноз 3',
          start: '2020-07-11',
          end: '2020-08-10',
          progress: 0,
          dependencies: '',
          custom_class: 'bar-3'
        },




        {
          // custom_index: 2,
          id: '2',
          name: 'полить цветы',
          start: '2020-04-05',
          end: '2020-05-10',
          progress: 50,
          dependencies: '1',
        },
        {
          // custom_index: 3,
          id: '3',
          name: 'покосить траву',
          start: '2020-04-10',
          end: '2020-05-15',
          progress: 30,
          dependencies: '2',
        },
        {
          // custom_index: 4,
          id: '4',
          name: 'покрасить забор',
          start: '2020-04-05',
          end: '2020-05-10',
          progress: 40,
          dependencies: ['1', '2', '3'],
        },

        /*
        {
          id: '5',
          name: 'ПРОГНОЗ 2',
          start: '2020-04-04',
          end: '2021-04-05',
          // progress: 10,
          custom_class: 'bar-2'
        },
        {
          id: '6',
          name: 'ПРОГНОЗ 3',
          start: '2020-04-04',
          end: '2021-04-05',
          // progress: 10,
          custom_class: 'bar-3'
        }
        */
      ],
      debugEventLog: [],
    }
  },
  methods: {
    addRandomTask() {
      // const id = uuidv4();
      this.tasks.push({
        id: this.tasks.length + 1, //id,
        name: 'NAME', //id,
        start: '2020-01-01',
        end: '2020-01-05',
        progress: Math.random() * 100,
        dependencies: '1',
      })
    },
    demoViewMode(viewMode) {
      this.mode = viewMode
    },
  },
}
</script>


<style lang="stylus" scoped>


.bar-main
  background red

.FRAPPE
  margin: 1rem;

.frappe-gantt
  margin: 0 1rem;
</style>
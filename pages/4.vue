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

    <frappe-gantt
      class="frappe-gantt"
      :view-mode="mode"
      :tasks="tasks"
      @task-updated="debugEventLog.push($event)"
      @task-date-updated="debugEventLog.push($event)"
      @task-progress-change="debugEventLog.push($event)"
    />
    <!-- <h3>
      TODO: When the Frappe Chart component emits task changes from user
      clicking or dragging make sure to then update the task list passed in via
      prop (App.vue)
    </h3> -->
    <a-button type="primary" size="large" @click="addRandomTask">Добавить</a-button>

    <!-- <h5>
        These are the events being emitted by the Vue.js component wrapper for
        Frappe Gantt
      </h5> -->
    <ul>
      <li v-for="event in debugEventLog" :key="event.id">
        {{ event }}
      </li>
    </ul>
  </div>
</template>

<script>
import FrappeGantt from '~/components/Frappe.vue'
// const uuidv5 = require('uuid/v5');
// import uuidv4 from 'uuid/v4';

export default {
  name: 'App',
  components: {
    FrappeGantt,
  },
  data() {
    return {
      mode: 'day',
      tasks: [
        {
          id: '1',
          name: 'БОЛЬШАЯ ЗАДАЧА',
          start: '2020-01-01',
          end: '2021-01-05',
          progress: 10,
        },
        {
          id: '2',
          name: 'полить цветы',
          start: '2020-01-05',
          end: '2020-01-10',
          progress: 20,
          dependencies: '1',
        },
        {
          id: '3',
          name: 'покосить траву',
          start: '2020-01-10',
          end: '2020-01-15',
          progress: 30,
          dependencies: '2',
        },
        {
          id: '4',
          name: 'покрасить забор',
          start: '2020-01-15',
          end: '2020-01-20',
          progress: 40,
          dependencies: ['3', '2', '1'],
        },
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
.FRAPPE
  margin: 1rem;

.frappe-gantt
  margin: 0 1rem;
</style>
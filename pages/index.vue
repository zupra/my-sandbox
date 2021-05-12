<template lang="pug">
<GanttGrid :data="DATA"></GanttGrid>
</template>

<script>

import faker from 'faker/locale/ru'

function randomInt(min, max) {
  let rand = min - 0.5 + Math.random() * (max - min + 1)
  rand = Math.round(rand)
  return rand
}
const types = ['blue', 'red', 'green', 'yellow', 'purple']
const DATA = Array.from({ length: randomInt(5, 10) }, (_, idx) => ({
  type: types[randomInt(0, 3)],
  title: faker.lorem.sentence(),
  start: faker.date.past(),
  end: faker.date.future(),
}))
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



export default {
  data() {
    return {
      DATA,
    }
  },
}
</script>

<style></style>

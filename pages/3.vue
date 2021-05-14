<template lang="pug">
div
  PreData 
    pre {{DATA}}

  SecondSGrid(
    :data="DATA"
  )
</template>

<script>

import faker from 'faker/locale/ru'

function randomInt(min, max) {
  let rand = min - 0.5 + Math.random() * (max - min + 1)
  rand = Math.round(rand)
  return rand
}
// const types = ['blue', 'red', 'green', 'yellow', 'purple']
const randDATA = Array.from({ length: randomInt(10, 15) }, (_, idx) => ({
  type: '', // types[randomInt(0, 3)],
  title: faker.lorem.sentence(),
  start: new Date('2021-4-14'), // faker.date.past(),
  end: new Date(`2021-${randomInt(4, 12)}-14`) // faker.date.future(),
  // parent: '',
  // dependency: ''
}))

const first = {
  type: '',
  title: 'ОГРОМНАЯ ЗАДАЧА',
  start: new Date('2021-4-14'), // faker.date.past(),
  end: new Date('2023-3-20') // faker.date.future(),
  // parent: '',
  // dependency: ''
}
const DATA = [first, ...randDATA]

export default {
  data() {
    return {
      DATA,
    }
  },
}
</script>

<style></style>

<template>
  <div class="content main__content">
    <div class="content__container">
      <h2 class="content__date">Today tasks</h2>
      <div class="content__records">
        <div class="record content__record" v-for="item in tasks" v-bind:key="item.id">
      <h3 class="record__title">{{item.name}}</h3>
      <div class="record__info">
        <p class="record__text">{{item.desc}}</p>
      <p class="record__date">{{item.deadLine}}</p>
      </div>
    </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Tasks',
  data () {
    return {
      tasks: [
        {
          name: 'Design mockup',
          desc: 'Create full rendering of design for client\'s web-product. Client K&H Science',
          deadLine: '12.11',
          id: 111
        },
        {
          name: 'Analysis of requirements and outcomes',
          desc: 'Evaluate the product design and development against project requirements and outcomes',
          deadLine: '14.12',
          id: 222
        },
        {
          name: 'Application Testing',
          desc: 'Identify errors in a website, provide unit, system and functional testing',
          deadLine: '21.11',
          id: 333
        }
      ]
    }
  }
})
</script>

<style scoped lang="scss">
.record__title {
  padding: 10px;
  opacity: 0.5;
    font-size: 12px;
    text-transform: uppercase;
}
</style>

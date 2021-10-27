<template>
   <div class="content main__content">
        <div class="main__container">
          <div class="content__container">
            <h2 class="content__date">Today</h2>
            <div class="content__records">
              <div class="record content__record">
                <div class="record__info">
                  <p class="record__text record__done">
                    Darika Samak mark as done Listing on Product Hunt so that we
                    can reach as many potential users
                  </p>
                  <p class="record__date">8:40 PM</p>
                </div>
              </div>
              <div class="record content__record">
                <div class="record__info">
                  <p class="record__text record__comment">
                    Emilee Simchenko commented on Account for teams and personal
                    in bottom style
                  </p>
                  <p class="record__date">7:32 PM</p>
                </div>
                <div class="record__details">
                  <p class="record__details-text">
                    During a project build, it is necessary to evaluate the
                    product design and development against project requirements
                    and outcomes
                  </p>
                </div>
              </div>
              <div class="record content__record">
                <div class="record__info">
                  <p class="record__text record__upload">
                    Darika Samak uploaded 4 files on An option to search in
                    current projects or in all projects
                  </p>
                  <p class="record__date">6:02 PM</p>
                </div>
                <div class="record__album" >
                  <img v-for="item in images" v-bind:key="item.id"
                  v-bind:src="require('../assets/'+item.img)"
                  v-on:click="getItemIndex(item.id)"
                    alt="record-img"
                    class="record__album-img"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      </template>

<script lang="ts">
import { defineComponent } from 'vue'
import mitt from 'mitt'

declare module '@vue/runtime-core' {
  interface ComponentCustomProperties {
    emitter: ReturnType<typeof mitt>
  }
}

export default defineComponent({
  name: 'Activity',
  data () {
    return {
      images: [
        {
          img: 'record_img_1.png',
          id: 1
        },
        {
          img: 'record_img_2.png',
          id: 2
        },
        {
          img: 'record_img_3.png',
          id: 3
        },
        {
          img: 'record_img_4.png',
          id: 4
        }
      ]
    }
  },
  methods: {
    getItemIndex (id: number) {
      const index = this.images.findIndex(i => i.id === id)
      this.emitter.emit('getIndex', index)
    }
  }
})
</script>

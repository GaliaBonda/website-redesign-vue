<template>
<div class="all-content">
    <section class="sidebar">
      <header class="sidebar-header">
        <div class="sidebar-header__top">
          <div class="sidebar__container">
            <h2 class="sidebar-header__text">PROJECTUS</h2>
          </div>
        </div>
      </header>
      <div class="person-info sidebar__person-info">
        <div class="sidebar__container">
          <div class="person-info__main">
            <img v-bind:src="require('./assets/'+avatar)" alt="Person photo" class="person-info__photo" />
            <div class="person-info__titles">
              <h3 class="person-info__title">{{userName}}</h3>
              <h4 class="person-info__subtitle">{{status}}</h4>
            </div>
            <a href="#" class="person-info__more">...</a>
          </div>
        </div>
      </div>
      <div class="sidebar__tasks">
        <div class="sidebar__container">
          <ul class="tasks">
            <li class="tasks__item" id="completed_tasks" v-on:click="changeTasksCounter" v-on:mouseover="makeTasksActive" v-on:mouseleave="makeTasksNonActive">
              <span class="tasks__item-num">{{completedTasks}}</span>
              <span class="tasks__item-text">Completed Tasks</span>
            </li>
            <li class="tasks__item" id="open_tasks">
              <span class="tasks__item-num">{{openTasks}}</span>
              <span class="tasks__item-text">Open Tasks</span>
            </li>
          </ul>
        </div>
      </div>
      <div class="sidebar__navigation">
        <div class="sidebar__container">
          <nav class="nav-container">
            <ul class="sidebar-nav">
              <li class="sidebar-nav__item">
                <a href="#" class="sidebar-nav__link sidebar-nav__link--active" v-on:click='makeActive'
                  >Menu</a
                >
              </li>
              <li class="sidebar-nav__item">
                <a href="#" class="sidebar-nav__link" v-on:click='makeActive'>Home</a>
              </li>
              <li class="sidebar-nav__item">
                <a href="#" class="sidebar-nav__link" v-on:click='makeActive'>My Tasks</a>
              </li>
              <li
                class="sidebar-nav__item sidebar-nav__item--notifications"
                id="notifications"
                v-bind:data-after="notifications"
              >
                <a href="#" class="sidebar-nav__link" v-on:click='makeActive' v-on:getIndex="changeNotifications">Notifications</a>
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </section>
    <section class="main">
      <header class="header">
        <div class="main__container">
          <div class="header__top">
            <div class="title">
              <h1 class="title__text">Website Redesign</h1>
              <a href="#" class="title__more">...</a>
            </div>
            <div class="socials">
              <div class="socials__photos">
                <img  v-for="item in socialImages" v-bind:key="item.id"
                  v-bind:src="require('./assets/'+item.img)"
                  alt="socials photo"
                  class="socials__photo"
                />
              </div>
              <a href="#" class="socials__share">Share</a>
              <a href="#" class="socials__chat">Chat</a>
            </div>
          </div>
          <nav class="header__nav">
            <ul class="nav" id="nav">
              <router-link to="/tasks"><li class="nav__item"><a href="#" class="nav__link" v-on:click='makeActive'>Tasks</a></li></router-link>
              <li class="nav__item">
                <a href="#" class="nav__link" v-on:click='makeActive'>Kanban</a>
              </li>
             <router-link to="/"> <li class="nav__item">
                <a href="#" class="nav__link nav__link--active" v-on:click='makeActive'>Activity</a>
              </li></router-link>
              <li class="nav__item">
                <a href="#" class="nav__link" v-on:click='makeActive'>Calendar</a>
              </li>
              <li class="nav__item"><a href="#" class="nav__link" v-on:click='makeActive'>Files</a></li>
            </ul>
          </nav>
           <router-view/>
        </div>
      </header>
      </section>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Helvetica, sans-serif;
    color: #131313;
}
</style>

<style lang="scss" src="./styles/scss/styles.scss"></style>
<style lang="scss" src="./styles/scss/media.scss"></style>
<style src="./styles/css/normalize.css"></style>
<style src="./styles/css/reset.css"></style>

<script lang="ts">
import { defineComponent } from 'vue'
import mitt from 'mitt'

declare module '@vue/runtime-core' {
  interface ComponentCustomProperties {
    emitter: ReturnType<typeof mitt>
  }
}

export default defineComponent({
  data (): Record<string, unknown> {
    return {
      userName: 'Jean Gonzales',
      status: 'Product Owner',
      avatar: 'person.png',
      completedTasks: 372,
      openTasks: 11,
      notifications: 3,
      socialImages: [
        {
          img: 'socials_1.png',
          id: 1
        },
        {
          img: 'socials_2.png',
          id: 2
        },
        {
          img: 'socials_3.png',
          id: 3
        }
      ]
    }
  },

  methods: {
    makeActive (e: Event): void {
      const currentElement = e.target as HTMLElement
      const selectorName = currentElement.className
      const selectorNameActive = `${selectorName}--active`
      const currentActive: HTMLElement | null = document.querySelector(`.${selectorNameActive}`)
      if (currentActive) {
        currentActive.classList.remove(selectorNameActive)
        currentElement.classList.add(selectorNameActive)
      }
    },
    makeTasksActive (e: Event): void {
      const currentElement = e.target as HTMLElement
      if (currentElement.id === 'completed_tasks') {
        currentElement.style.cursor = 'pointer'
        currentElement.style.outline = '3px solid rgba(191, 191, 191, 0.2)'
        currentElement.style.outlineOffset = '4px'
      }
    },
    makeTasksNonActive (e: Event): void {
      (e.target as HTMLElement).style.outline = 'none'
    },
    changeTasksCounter (): void {
      if (this.openTasks as number > 0) {
        this.confirmChange()
      } else {
        const declineWindow = this.createWindow('You don\'t have any open tasks to set complete.')
        const okBtn = this.createButton('Ok')
        const btnBlock = this.createBtnBlock()
        btnBlock.appendChild(okBtn)
        declineWindow.appendChild(btnBlock)
        okBtn.onclick = function () {
          declineWindow.remove()
        }
      }
    },
    confirmChange (): void {
      const confirmWindow = this.createWindow('Are you sure you want to change the number of tasks?')
      const btnBlock = this.createBtnBlock()
      const yesBtn = this.createButton('Yep')
      const noBtn = this.createButton('Nope')
      btnBlock.appendChild(yesBtn)
      btnBlock.appendChild(noBtn)

      confirmWindow.appendChild(btnBlock)
      yesBtn.onclick = () => {
        this.completedTasks = this.completedTasks as number + 1
        this.openTasks = this.openTasks as number - 1
        confirmWindow.remove()
      }
      noBtn.onclick = () => confirmWindow.remove()
    },
    createWindow (message: string): HTMLDivElement {
      const confirmWindow = document.createElement('div')
      confirmWindow.classList.add('confirmWindowStyles')
      document.body.style.position = 'relative'
      const confirmMessage = this.createConfirmMessage(message)
      confirmWindow.appendChild(confirmMessage)
      document.body.appendChild(confirmWindow)
      return confirmWindow
    },
    createConfirmMessage (message: string): HTMLParagraphElement {
      const confirmMessage = document.createElement('p')
      confirmMessage.classList.add('confirmMessageStyles')
      confirmMessage.innerText = message
      return confirmMessage
    },
    createButton (btnName: string): HTMLButtonElement {
      const btn = document.createElement('button')
      btn.innerHTML = btnName
      btn.classList.add('btnStyles')
      return btn
    },
    createBtnBlock (): HTMLDivElement {
      const btnBlock = document.createElement('div')
      btnBlock.classList.add('btnBlockStyles')
      return btnBlock
    }
  },
  mounted () {
    this.emitter.on('getIndex', (index) => {
      this.notifications = index as number
    })
  }
})
</script>

<style lang="scss">
.btnBlockStyles {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 70px;
}
.btnStyles {
  width: 70px;
  height: 40px;
  color: black;
  background-color: white;
  border: white;
  border-radius: 5px;
  font-size: 20px;
}
.confirmMessageStyles {
padding: 30px 30px 50px 30px;
    font-size: 24px;
    color: white;
}
.confirmWindowStyles {
  position: fixed;
    top: 50vh;
    left: 50vw;
    width: 350px;
    height: 250px;
    transform: translate(-50%, -50%);
    background-color: black;
    border-radius: 5px;
}
</style>

<template>
  <div class="container">
    <h1>{{message}}</h1>
    <div>
      <input class="form-control" placeholder="Title" type="text" v-bind:value="value" @change="inputHandler">
    </div>
    <div>
      <input class="form-control" placeholder="Text" type="text" v-bind:value="textValue" @change="inputTextHandler">
    </div>
    <button class="btn btn-primary btn-lg" @click="addItem">Send</button>
    <h2>{{alertMessage}}</h2>
    <input class="form-check-input" type="checkbox" name="done" id="done" @change="show">
      <label class="form-check-label" for="done">
        Done
      </label>
    <ul>
      <li v-bind:key="item.id" v-for="item in list" :class="{show: item.show}">
        <div class="card mb-3" v-bind:class="item.style">
          <div class="card-header" @click="remove(item.id)">X</div>
          <div :class="{done: !item.done}">✔️</div>
          <div class="card-body">
            <h3 class="card-title"
              :class="item.style"
              @click="isDone(item.id)">{{item.title}}</h3>
            <p class="card-text">{{item.text}}</p>
          </div>
        </div>

      </li>
    </ul>
  </div>
</template>

<script>
import {random_item} from '../utils/utils'


export default {
  name: 'TodoList',
  props: {
    msg: String
  },
  data() {
    return {
      message: 'Hi',
      value: '',
      textValue: '',
      alertMessage: '',
      list: window.localStorage.getItem('todos')
        ? JSON.parse(window.localStorage.getItem('todos'))
        : [],
      cardStyles: ['text-white bg-primary', 'text-white bg-secondary', 'text-white bg-success', 'text-white bg-danger', 'text-white bg-dark', 'text-dark bg-warning', 'text-dark bg-info', 'text-dark bg-light'],
    }
  },
  methods: {
    inputHandler(e) {
      this.value = e.target.value
    },

    inputTextHandler(e) {
      this.textValue = e.target.value
    },

    setLS(list) {
      window.localStorage.setItem(
        'todos',
        JSON.stringify(list)
      );
    },

    addItem() {
      if (!this.value || !this.textValue) {
        this.alertMessage = 'Null input'
        return
      }
      const newItem = {
        title: this.value,
        text: this.textValue,
        id: Math.ceil(Math.random() * 10000),
        done: false,
        show: true,
        style: this.getRandomStyle()
      }
      this.list.push(newItem)
      this.setLS(this.list)
      this.value = ''
      this.textValue = ''
      this.alertMessage = ''
      console.log(newItem);
    },

    remove(id) {
      this.list = this.list.filter(i => i.id !== id)
      this.setLS(this.list)
    },

    isDone(id) {
      console.log(id);

      this.list = this.list.map(i => {
        if (i.id === id) i.done = !i.done
        return i
      })
      this.setLS(this.list)
    },

    showAll() {
      this.list = this.list.map(i => {
        i.show = true
        return i
      })
    },

    show(e) {
        if (e.target.checked === true) {
            this.showDone()
        } else {
            this.showAll()
        }
    },

    showDone() {
      this.list = this.list.map(i => {
        if (i.done) {
          i.show = true
        } else {
          i.show = false
        }
        return i
      })
    },

    getRandomStyle() {
      return random_item(this.cardStyles)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h2 {
  color: red;
}

ul {
  list-style-type: none;
  padding: 0;
  width: 500px;
  margin: 10px auto;
}
li {
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  /* width: 400px; */
  display: none;
}

input {
  margin-top: 10px;
}
label {
  margin: 6px;
}

.show {
  display: flex;
}
a {
  color: #42b983;
}

.text {
  padding: 15px;
  padding-right: 35px;
}

.close {
  padding-top: 15px;
  color: red;
  cursor: pointer;
}

.done {
  display: none;
}

.card {
  width: 400px;
  margin: 0 auto;
  box-shadow: 0 0 10px rgba(0,0,0,0.5);
  transition: all .2s linear;
}

.card:hover {
  transform: scale(1.01);
}

.btn {
  margin-top: 20px;
  min-width: 200px;
}
</style>

<template>
   <div>
    <header>
      <h2 calss="container">Todo</h2>
    </header>
 
    <div calss="container">
      <add-form v-bind:value="query" v-on:@submit="onInputTodo"></add-form>
      <!-- FormComponent.vue에서 inputValue에 넣어준 value를 v-bind가 App.vue의 query와 바인딩해줌 -->
      <!-- v-on:@submit => FormComponent.vue에서 $emit으로 전달한 @submit이라는 이벤트를 받아와서 onInputTodo 메서드와 연결 -->
     
      <tab v-bind:tabs="tabs" v-bind:selected-tab="selectedTab" v-on:@change="onClickTab"></tab>
 
    </div>
 
    <div>
      <list v-bind:selected-tab="selectedTab" v-bind:data="todoList"
        v-on:@finish="onClickFinish"
        v-on:@reset="onClickReset"></list>
    </div>
          <!-- <div id="app">
          <div id="nav">
            <router-link to="/">Home</router-link> |
            <router-link to="/about">About</router-link>
          </div>
          <router-view/>
          </div> -->
  </div>
</template>

<script>
import FormComponent from './components/FormComponent.vue'
import TabComponent from './components/TabComponent.vue'
import ListComponent from './components/ListComponent.vue' 
import TodoModel from './models/TodoModel.js'

export default {
  name: 'app',
  data () {
    return {
      query: '',
      tabs: ['todo', 'finish'],
      selectedTab: '',
      todoList: []
    }
  },
  created() { //vue 인스턴스가 생성된 후에 실행됨
    this.selectedTab = this.tabs[0] //todo 탭 선택
    this.search() //todo list 출력
  },
  components: { //사용할 컴포넌트 등록
    'add-form': FormComponent,
    'tab': TabComponent,
    'list': ListComponent
  },
  methods: {
     search() { //list 검색
      TodoModel.list(this.selectedTab).then(data => {
        this.todoList = data
      })
    },
    onClickTab(tab) { //tab 선택
      this.selectedTab = tab
      this.search()
    },
    onClickFinish(item) { //todo 완료
      TodoModel.finish(item)
      this.search()
    },
    onClickReset(item) { //완료된 todo 리셋
      TodoModel.reset(item)
      this.search()
    },
    onInputTodo(query) { //todo 입력
      TodoModel.add(query)
      this.selectedTab = this.tabs[0]
      this.search()
    }
  }
}
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
} */
</style>

<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-md-3">
          <div>
            <tasks-component/>
          </div>
        </div>
        <div class="col-md-3">
          <div>
            <completed-component/>
          </div>
        </div>
        <div class="col-md-3">
          <div>
            <deferred-component/>
          </div>
        </div>
        <div class="col-md-3">
          <div>
            <rejected-component/>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import DeferredComponent from './components/DeferredComponent.vue'
import TasksComponent from './components/TasksComponent.vue'
import CompletedComponent from './components/CompletedComponent.vue'
import RejectedComponent from './components/RejectedComponent.vue'

export default {
  name: 'App',
  components: {
    TasksComponent,
    CompletedComponent,
    DeferredComponent,
    RejectedComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}


.box-title {
  font-size: 1.2rem;
  /*text-align: left; */
  font-weight: 700;
  margin-bottom: 0.8rem;
}
.col-md-3{
  padding: 0;
}
ul {
  padding: 0 1rem;
  text-align: left;
}

ul li {
  list-style-type: none;
  font-size: 0.8rem;
}

li{
  padding: 0.5rem;
  margin-bottom: 0.5rem;
  border-radius: 8px;
}
.vm--modal{
  border-radius: 15px;
}

</style>

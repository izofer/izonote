<template>
  <div id="app">
      <section class="hero is-info">
        <div class="hero-body">
          <div class="container">
            <h1 class="title">
              IzoNote
            </h1>
            <h2 class="subtitle">
              By: @izofer
            </h2>
          </div>
        </div>
      </section>

      <div class="columns is-mobile hero is-success">

        <div id="panel" class="column is-three-fifths is-offset-one-fifth">
          <input type="text" class="tittle input" id="tittle" placeholder="Titulo de la tarea" v-model="newTask.title" autofocus>
          <input type="number" class="time input" id="time" placeholder="Horas invertidas" v-model="newTask.time">
          <button class="button is-primary" @click="addTask()">Agregar Tarea</button> 
          <button class="button is-danger" @click="cancelTask()">Cancelar</button>
        </div>

      </div>

      <div id="visor" class="columns is-mobile hero is-light is-fullheight">
        <div class="column is-three-fifths is-offset-one-fifth">
         
         <div class="columns">
            <div class="column">
              <strong>Tarea</strong>
            </div>
            <div class="column">
              <strong>Tiempo Invertido</strong> 
            </div>
            <div class="column">
              <strong>Borrar</strong>
            </div>
          </div>

          <div class="columns" v-for="(t,n) in task">
            <div class="column">
              {{t.title | upper}}
            </div>
            <div class="column">
              {{t.time}} Horas
            </div>
            <div class="column">
              <a href="#" class="delete" @click="del(n)">&times</a>
            </div>
          </div>
         
          
          <small v-if="task.length > 0"> Tiempo de horas invertidas: {{totalTime}} horas </small>
          <label v-else>Aun no hay Tareas grabadas, animate a crear tu tarea del dia :)</label>
        </div>  
      </div>

  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      task:[],
      newTask:{}
    }
  },
  created(){
    this.task = JSON.parse(localStorage.getItem('task')) || [];
  },
  methods:{
    addTask(){
      this.showTime = true;

      if(this.newTask.title && this.newTask.time)
      {
        this.task.push({ title:this.newTask.title, time:this.newTask.time });
        this.cancelTask();
        document.getElementById("tittle").focus();
        
        localStorage.setItem('task',JSON.stringify(this.task));
      }

    },
    cancelTask(){
      if(this.newTask.title && this.newTask.time)
      {
        this.newTask.title = '';
        this.newTask.time = NaN;
      } 
    },
    del(data){
      this.task.splice(data,1);

      localStorage.setItem('task',JSON.stringify(this.task));
    }
  },
  computed:{
    totalTime(){ 
      return this.task.reduce( (acum, t) => acum += parseInt(t.time), 0 );
    },
  },
  filters:{
    upper(str){
      return str.charAt(0).toUpperCase()+str.slice(1);
    }
  }
}
</script>
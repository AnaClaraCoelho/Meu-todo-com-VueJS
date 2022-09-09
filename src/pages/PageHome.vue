<template>
  <div id="app">
    <nav class="pink lighten-3">
      <div class="nav-wrapper"></div>
    </nav>

    <img alt="Vue logo" src="../assets/logo.png" />
    <div v-show="exibir.lista">
      <button class="btn" @click="mostrarCadastro" v-if="exibir.lista == true">
        Adicionar
      </button>
      <TarefaList
        msg="Welcome to Your Vue.js PageHome"
        :tasks="listadeTarefa"
      />
    </div>

    <!--FORM-->
    <div v-show="exibir.form">
      <h1>Cadastrar Tarefa</h1>
      <input
        type="text"
        name="title"
        id="title"
        placeholder="Entre com a tarefa"
        v-model="form.title"
      />
      <input
        type="text"
        name="project"
        v-model="form.project"
        placeholder="Entre com um projeto"
      />
      <button @click="salvarTarefa">Salvar</button>
    </div>
  </div>
</template>

<script>
import TasksApi from '../TasksApi.js'
import TarefaList from '../components/TarefaList.vue'

export default {
  components: {
    TarefaList,
  },
  data: () => {
    return {
      listadeTarefa: [],
      exibir: {
        lista: true,
        form: false,
      },
      form: {
        title: 'teste',
        project: '',
      },
    }
  },
  methods: {
    listarTarefas() {
      TasksApi.getTasks((data) => {
        this.listadeTarefa = data
      })
    },
    mostrarCadastro() {
      ;(this.exibir.form = true), (this.exibir.lista = false)
    },
    salvarTarefa() {
      ;(this.exibir.form = false), (this.exibir.lista = true)
      const novaTarefa = {
        title: this.form.title,
        project: this.form.project,
        date: new Date().toLocaleDateString('pt'),
      }
      TasksApi.createTask(novaTarefa, () => {
        this.listarTarefas()
      })
    },
  },
  created() {
    this.listarTarefas()
  },
}
</script>

<style></style>

<template>
  <div id="app">
    <nav class="pink lighten-3">
      <div class="nav-wrapper"></div>
    </nav>

    <div v-show="exibir.lista" style="padding: 20px">
      <button class="btn" @click="mostrarCadastro">Adicionar</button>
    </div>
    <!-- lista -->
    <div v-show="exibir.lista">
      <TarefaList
        :msg="'Lista de tarefas'"
        :tasks="listaDeTarefa"
        @editarClick="recebiEditar"
        @excluirClick="recebiExcluir"
      />
    </div>
    <!-- FORM -->
    <div v-show="exibir.form">
      <TarefaForm
        :id="form.id"
        :titulo="form.titulo"
        :title="form.title"
        :project="form.project"
        :btn="form.btn"
        @salvarClick="recebiSalvar"
        @alterarClick="recebiAlterar"
      ></TarefaForm>
    </div>
  </div>
</template>
<script>
import TasksApi from '../TasksApi.js'
import TarefaList from '../components/TarefaList.vue'
import TarefaForm from '../components/TarefaForm.vue'
export default {
  components: {
    TarefaList,
    TarefaForm,
  },
  data: () => {
    return {
      listaDeTarefa: [],
      exibir: {
        lista: true,
        form: false,
      },
      form: {
        id: 0,
        titulo: 'Cadastrar Tarefa',
        title: '',
        project: '',
        btn: 'Adicionar',
      },
    }
  },
  methods: {
    listarTarefas() {
      TasksApi.getTasks((data) => {
        this.listaDeTarefa = data
      })
    },
    mostrarCadastro() {
      this.form.btn = 'Adicionar'
      this.exibir.form = true
      this.exibir.lista = false
    },
    recebiSalvar(novaTarefa) {
      TasksApi.createTask(novaTarefa, () => {
        this.listarTarefas()
        this.exibir.form = false
        this.exibir.lista = true
      })
    },
    recebiAlterar(tarefa) {
      TasksApi.updateTasks(tarefa, () => {
        this.listarTarefas()
        this.exibir.form = false
        this.exibir.lista = true
      })
    },
    recebiEditar(tarefaId) {
      this.form.btn = 'Alterar'
      TasksApi.getTask(tarefaId, (task) => {
        this.form.id = task.id
        this.form.title = task.title
        this.form.project = task.project
        this.exibir.form = true
        this.exibir.lista = false
      })
    },
    recebiExcluir(tarefaId) {
      console.log('1', tarefaId)
      TasksApi.excludeTask(tarefaId, () => {
        //this.listaDeTarefa.slice(tarefaId - 1, 1)
        this.listarTarefas()
        this.exibir.form = false
        this.exibir.lista = true
      })
    },
  },
  created() {
    this.listarTarefas()
  },
}
</script>
<style></style>

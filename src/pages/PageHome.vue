<template>
  <div id="app">
    <nav class="pink lighten-3">
      <div class="nav-wrapper"></div>
    </nav>
    <img alt="Vue logo" src="../assets/logo.png" />
    <TarefaList msg="Welcome to Your Vue.js PageHome" :tasks="listadeTarefa" />
  </div>
</template>

<script>
import TasksApi from "../TasksApi.js";
import TarefaList from "../components/TarefaList.vue";

export default {
  components: {
    TarefaList,
  },
  data: () => {
    return {
      listadeTarefa: ["A", "B", "C"],
    };
  },
  created() {
    TasksApi.getTasks((data) => {
      this.listadeTarefa = data;
    });
  },
};
</script>

<style></style>

<template>
  <div class="container">
    <h1>vue.js + Github</h1>
    <p class="lead">
      Página que lista issues de um repositório do Github, usando Vue.js.
    </p>
    <div class="row">
      <div class="col">
        <div class="form-group">
          <input v-model="username" type="text" class="form-control"
          placeholder="github username">
        </div>
      </div>

      <div class="col">
        <div class="form-group">
          <input v-model="repository" type="text" class="form-control"
          placeholder="github repositório">
        </div>
      </div>

      <div class="col-3">
        <div class="form-group">
          <button @click.prevent.stop="getIssues()" class="btn btn-success">Go</button>

          <button @click.prevent.stop="reset()" class="btn btn-danger">LIMPAR</button>
        </div>
      </div>

    </div>

    <br>
    <hr><br>

    <table class="table table-sm table-bordered">
      <thead>
        <tr>
          <th width="100">Número</th>
          <th>Título</th>
        </tr>
      </thead>

      <tbody v-if="!!issues.length">
        <tr v-if="loader.getIssues">
          <td class="text-center" colspan="2"><img src="/static/loader.svg" alt=""></td>
        </tr>
        <tr
          v-for="issue in issues"
          :key="issue.number">
          <td>{{issue.number}}</td>
          <td>{{issue.title}}</td>
        </tr>
      </tbody>

      <tbody v-if="!!!issues.length && !loader.getIssues">
        <tr>
          <td class="text-center" colspan="2">Nenhum issues encontrado!</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// eslint-disable-next-line import/no-extraneous-dependencies
import axios from 'axios';

export default {
  name: 'GitHubIssues',
  data() {
    return {
      username: '',
      repository: '',
      issues: [],
      loader: {
        getIssues: false,
      },
    };
  },

  methods: {
    reset() {
      this.username = '';
      this.repository = '';
    },

    getIssues() {
      if (this.username && this.repository) {
        this.loader.getIssues = true;
        const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues`;
        axios.get(url).then((response) => {
          this.issues = response.data;
        }).finally(() => {
          this.loader.getIssues = false;
        });
      }
    },
  },
};

</script>

<style></style>

<template>
  <div id="app">
    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Produtos Front</a>
      </div>
    </nav>
    <div class="container">
      <form @submit.prevent="salvar">
          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="produto.nome">
          <label>Quantidade</label>
          <input type="number" placeholder="QTD" v-model="produto.quantidade">
          <label>Valor</label>
          <input type="text" placeholder="Valor" v-model="produto.valor">

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>
      </form>
      <table>
        <thead>
          <tr>
            <th>NOME</th>
            <th>QTD</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(produto, index) in produtos" :key="index">
            <td>{{produto.nome}}</td>
            <td>{{produto.quantidade}}</td>
            <td>{{produto.valor}}</td>
            <td>
              <button class="waves-effect btn-small blue darken-1"@click="editar(produto)"><i class="material-icons">create</i></button>
              <button class="waves-effect btn-small red darken-1" @click="deletar(produto)"><i class="material-icons">delete_sweep</i></button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Produtos from './services/produtos'

export default {
  data: () => ({
    produto: {
      id: '',
      nome: '',
      quantidade: '',
      valor: ''
    },
    produtos: []
  }),
  mounted () {
    this.getAllProducts ()
  },
  methods: {
    async getAllProducts () {
      try {
      const res = await Produtos.listar()
      console.log(res.data)
      this.produtos = res.data
      } catch(error){
        console.log(error)
      }
    },
    async salvar () {
      if (!this.produtos.id) {
        try {
          Produtos.salvar(this.produto)
          await this.getAllProducts()
          this.produto = {}
          alert('salvou')
        } catch(error){
          console.log(error)
        }
      } else {
        try {
          Produtos.atualizar(this.produto)
          await this.getAllProducts()
          this.produto = {}
          alert('atualizou')
        } catch(error){
          console.log(error)
        }
      }
    },
    editar (produto) {
        this.produto = produto
    },
    async deletar (produto) {
      try {
        await this.getAllProducts()
        Produtos.deletar(produto)
        alert('deletou')
      } catch(error){
        console.log(error)
      }
    }
  }
}
</script>

<style>



</style>

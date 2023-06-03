<template>
  <div class="row">
    <div class="col-12">
      <card :title="table1.title" :subTitle="table1.subTitle">
        <div slot="raw-content" class="table-responsive">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Username</th>
                <th>Nome</th>
                <th>Email</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in utilizadoresLocalStorage" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.username }}</td>
                <td>{{ item.nome }}</td>
                <td>{{ item.email }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      table1: {
        title: "Lista dos Utilizadores",
        subTitle: "Lista de Utilizadores logados",
      },
      utilizadoresLocalStorage: [], // Utilizadores carregados do Local Storage
    };
  },
  mounted() {
    this.carregarDadosLocalStorage();
  },
  methods: {
    carregarDadosLocalStorage() {
      const dadosExistentes = localStorage.getItem("utilizadores");

      if (dadosExistentes) {
        this.utilizadoresLocalStorage = JSON.parse(dadosExistentes);
      }
    },
  },
};
</script>

<style>
/* Estilos personalizados para o componente */
</style>

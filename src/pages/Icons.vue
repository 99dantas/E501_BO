<template>
  <div>
    <table>
      <!-- Aulas -->
      <thead>
        <tr>
          <th>Username</th>
          <th>Nome</th>
          <th>Idade</th>
          <th>Contacto Telefónico</th>
          <th>Plano de Aulas</th>
          <th>Vezes por Semana</th>
          <th>Horário</th>
          <th>Opções</th>
        </tr>
      </thead>
      <!-- Corpo da tabela -->
      <tbody>
        <tr v-for="aula in aulas" :key="aula.id">
          <td>{{ aula.username }}</td>
          <td>{{ aula.nome }}</td>
          <td>{{ aula.idade }}</td>
          <td>{{ aula.contactotelefonico }}</td>
          <td>{{ aula.planoaulas }}</td>
          <td>{{ aula.vezesSemana }}</td>
          <td>{{ aula.horario }}</td>
          <td>{{ aula.contacto }}</td>
          <td>{{ aula.estado }}</td>
          <td>
            <template v-if="aula.estado !== 'Aceite'">
              <button @click="aceitarAula(aula)">Aceitar</button>
              <button @click="rejeitarAula(aula)">Rejeitar</button>
            </template>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      aulas: []
    };
  },
  mounted() {
    this.carregarAulas();
  },
  methods: {
    carregarAulas() {
      const aulasLocalStorage = localStorage.getItem('aulas');
      if (aulasLocalStorage) {
        this.aulas = JSON.parse(aulasLocalStorage);
      }
    },
    aceitarAula(aula) {
      // Lógica para aceitar a aula
      aula.estado = 'Aceite';

      // Atualizar a aula no array de aulas
      const index = this.aulas.indexOf(aula);
      if (index > -1) {
        this.aulas.splice(index, 1, aula);
        this.atualizarLocalStorage();
      }

      // Adicionar a aula aceite ao localStorage em "aulaAceite"
      const aulaAceite = {
        nome: aula.nome,
        horario: aula.horario,
        vezesSemana: aula.vezesSemana
      };
      localStorage.setItem('aulaAceite', JSON.stringify(aulaAceite));

      // Chamar o método para adicionar a aula ao horário
      this.adicionarAulaAoHorario(aula);

      // Atualizar o localStorage com as informações atualizadas
      localStorage.setItem('aulas', JSON.stringify(this.aulas));
    },
    rejeitarAula(aula) {
      // Lógica para rejeitar a aula
      aula.estado = 'Rejeitada';

      // Atualizar a aula no array de aulas
      const index = this.aulas.indexOf(aula);
      if (index > -1) {
        this.aulas.splice(index, 1, aula);
        this.atualizarLocalStorage();
      }
    },
    atualizarLocalStorage() {
      // Atualizar o localStorage com as informações atualizadas
      localStorage.setItem('aulas', JSON.stringify(this.aulas));
    },
    adicionarAulaAoHorario(aula) {
      // Lógica para adicionar a aula ao horário
      // Implemente essa lógica de acordo com suas necessidades
      console.log('Aula adicionada ao horário:', aula);
    },
  }
};
</script>

<style>
/* Estilos da tabela */
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
}
</style>

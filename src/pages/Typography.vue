<template>
  <div>
    <table>
      <!-- Reservas -->
      <thead>
        <tr>
          <th>ID Cliente</th>
          <th>Nome Cliente</th>
          <th>Contacto Cliente</th>
          <th>Número de Raquetes</th>
          <th>Campo</th>
          <th>Data Reserva</th>
          <th>Hora</th>
          <th>Estado</th>
          <th>Opções</th>
        </tr>
      </thead>
      <!-- Corpo da tabela -->
      <tbody>
        <tr v-for="reserva in reservas" :key="reserva.id_marcacao">
          <td>{{ reserva.username }}</td>
          <td>{{ reserva.nome }}</td>
          <td>{{ reserva.contacto }}</td>
          <td>{{ reserva.numRaquetes }}</td>
          <td>{{ reserva.campo }}</td>
          <td>{{ reserva.data }}</td>
          <td>{{ reserva.hora }}</td>
          <td>{{ reserva.estado }}</td>
          <td>
            <template v-if="reserva.estado !== 'Aceite'">
              <button @click="aceitarReserva(reserva)">Aceitar</button>
              <button @click="removerReserva(reserva)">Remover</button>
            </template>
            <span v-else>Aceite</span>
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
      reservas: [],
    };
  },
  mounted() {
    this.carregarReservas();
  },
  methods: {
    carregarReservas() {
      const reservasLocalStorage = localStorage.getItem('reservas');
      if (reservasLocalStorage) {
        this.reservas = JSON.parse(reservasLocalStorage);
      }
    },
    aceitarReserva(reserva) {
      // Lógica para aceitar a reserva
      reserva.estado = 'Aceite';

      // Atualizar a reserva no array de reservas
      const index = this.reservas.indexOf(reserva);
      if (index > -1) {
        this.reservas.splice(index, 1, reserva);
        this.atualizarLocalStorage();
      }

      // Adicionar a reserva aceita ao localStorage "reservaAceite"
      const reservaAceite = {
        id_cliente: reserva.username,
        campo: reserva.campo,
        hora: reserva.hora,
      };
      this.adicionarReservaAceite(reservaAceite);
    },

    removerReserva(reserva) {
      // Lógica para remover a reserva
      const index = this.reservas.indexOf(reserva);
      if (index > -1) {
        this.reservas.splice(index, 1);
        this.atualizarLocalStorage();
      }
    },

    atualizarLocalStorage() {
      localStorage.setItem('reservas', JSON.stringify(this.reservas));
    },

    adicionarReservaAceite(reservaAceite) {
      const reservasAceitesLocalStorage = localStorage.getItem('reservaAceite');
      let reservasAceites = [];
      if (reservasAceitesLocalStorage) {
        reservasAceites = JSON.parse(reservasAceitesLocalStorage);
      }
      reservasAceites.push(reservaAceite);
      localStorage.setItem('reservaAceite', JSON.stringify(reservasAceites));
    },
  },
};
</script>

<style>
/* Estilos da tabela */
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
}
</style>

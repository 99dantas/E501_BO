<template>
  <div class="MapaCampos">
    <h2>Lista de Reservas</h2>
    <div v-if="currentField !== null">
      <h3>{{ currentField.name }}</h3>
      <ul>
        <li v-for="reservation in currentFieldReservations" :key="reservation.id">
          <div class="reservation-info">
            <span class="bullet"></span>
            <span class="user"><strong>ID Cliente:</strong> {{ reservation.id_cliente }}</span>
            <span class="time"><strong>Hora:</strong> {{ reservation.hora }}</span>
          </div>
        </li>
      </ul>
    </div>
    <div>
      <button @click="previousField" :disabled="currentFieldIndex === 0">Anterior</button>
      <button @click="nextField" :disabled="currentFieldIndex === fields.length - 1">Próximo</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fields: [
        { id: 1, name: 'Campo 1', reservations: [] },
        { id: 2, name: 'Campo 2', reservations: [] },
        { id: 3, name: 'Campo 3', reservations: [] },
        { id: 4, name: 'Campo 4', reservations: [] }
      ],
      currentFieldIndex: 0,
      reservations: {}
    };
  },
  computed: {
    currentField() {
      if (this.currentFieldIndex !== null) {
        return this.fields[this.currentFieldIndex];
      }
      return null;
    },
    currentFieldReservations() {
      if (this.currentField !== null) {
        const fieldId = this.currentField.id;
        return this.reservations[fieldId] || [];
      }
      return [];
    }
  },
  methods: {
    previousField() {
      if (this.currentFieldIndex > 0) {
        this.currentFieldIndex--;
      }
    },
    nextField() {
      if (this.currentFieldIndex < this.fields.length - 1) {
        this.currentFieldIndex++;
      }
    },
    setCurrentField(index) {
      this.currentFieldIndex = index;
    },
    fetchReservationsFromLocalStorage() {
      const savedReservations = localStorage.getItem('reservaAceite');
      if (savedReservations) {
        const reservations = JSON.parse(savedReservations);
        reservations.forEach(reservation => {
          const fieldNumber = reservation.campo; // Número do campo
          const field = this.fields.find(f => f.id === fieldNumber);
          if (field) {
            field.reservations.push({
              id: reservation.id_cliente, // ID da reserva
              id_cliente: reservation.id_cliente,
              hora: reservation.hora // Hora de início da reserva
            });
          }
          if (this.reservations.hasOwnProperty(fieldNumber)) {
            this.reservations[fieldNumber].push(reservation);
          } else {
            this.reservations[fieldNumber] = [reservation];
          }
        });
      }
    }
  },
  mounted() {
    this.fetchReservationsFromLocalStorage();
  }
};
</script>

<style scoped>
.MapaCampos {
  max-width: 600px;
  margin: 0 auto;
}

.map-container {
  margin-bottom: 20px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}

.bullet {
  display: inline-block;
  width: 4px;
  height: 4px;
  background-color: #000;
  margin-right: 5px;
}

h2,
h3 {
  margin: 0;
}

.reservation-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.time {
  font-weight: bold;
}

.user {
  color: #666666;
}
</style>

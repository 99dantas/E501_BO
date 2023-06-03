<template>
  <div class="MapaCampos">
    <h2>Lista de Reservas</h2>
    <div v-if="currentField !== null">
      <h3>{{ currentField.name }}</h3>
      <ul>
        <li v-for="reservation in currentFieldReservations" :key="reservation.id">
          <div class="reservation-info">
            <span class="bullet"></span>
            <span class="time"><strong>Horário:</strong> {{ reservation.horario }} - Noite</span>
            <span class="user"><strong>ID Cliente:</strong> {{ reservation.nome }}</span>
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
    fetchReservationsFromLocalStorage() {
      const savedReservation = localStorage.getItem('aulaAceite');
      if (savedReservation) {
        const reservation = JSON.parse(savedReservation);
        const fieldId = 3; // ID do Campo 3
        const field = this.fields.find(f => f.id === fieldId);
        if (field) {
          field.reservations.push(reservation);
        }
        this.reservations = { [fieldId]: [reservation] };
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

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}

.bullet {
  display: inline-block;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: black;
  margin-right: 5px;
}

h2, h3 {
  margin: 0;
}

.reservation-info {
  display: flex;
  align-items: center;
}

.time, .user {
  margin: 5px;
}

.time strong, .user strong {
  margin-right: 5px;
}

.user {
  color: #666666;
}
</style>

<template>
  <div>
    <!-- Stats cards -->
    <div class="row">
      <div class="col-md-6 col-xl-3" v-for="stats in statsCards" :key="stats.title">
        <stats-card>
          <div class="icon-big text-center" :class="`icon-${stats.type}`" slot="header">
            <i :class="stats.icon"></i>
          </div>
          <div class="numbers" slot="content">
            <p>{{ stats.title }}</p>
            {{ stats.value }}
          </div>
          <div class="stats" slot="footer">
            <i :class="stats.footerIcon"></i> {{ stats.footerText }}
          </div>
        </stats-card>
      </div>
    </div>

    <!-- Gráficos -->
    <div class="row">
      <div class="col-12">
        <chart-card
          title="Raquetes"
          sub-title="Desempenho nas últimas 24 horas"
          :chart-data="usersChart.data"
          :chart-options="usersChart.options"
        >
          <span slot="footer">
            <i class="ti-reload"></i> Atualizado há 3 minutos
          </span>
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> Aberto
            <i class="fa fa-circle text-danger"></i> Clique
            <i class="fa fa-circle text-warning"></i> Segundo Clique
          </div>
        </chart-card>
      </div>

      <div class="col-md-6 col-12">
        <chart-card
          title="Estatísticas Campo"
          sub-title="Last campaign performance"
          :chart-data="preferencesChart.data"
          chart-type="Pie"
        >
          <span slot="footer">
            <i class="ti-timer"></i> Campaign set 2 days ago
          </span>
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> Campo 1
            <i class="fa fa-circle text-danger"></i> Campo 2
            <i class="fa fa-circle text-warning"></i> Campo 3
            <i class="fa fa-circle text-wa"></i> Campo 4
          </div>
        </chart-card>
      </div>

      <div class="col-md-6 col-12">
        <chart-card
          title="Horas Mais Escolhidas"
          sub-title="All products including Taxes"
          :chart-data="activityChart.data"
          :chart-options="activityChart.options"
        >
          <span slot="footer">
            <i class="ti-check"></i> Data information certified
          </span>
          <div slot="legend">
            <i class="fa fa-circle text-info"></i> Clientes
          </div>
        </chart-card>
      </div>
    </div>
  </div>
</template>

<script>
import { StatsCard, ChartCard } from "@/components/index";
import Chartist from "chartist";

export default {
  components: {
    StatsCard,
    ChartCard,
  },
  data() {
    return {
      statsCards: [
        {
          type: "warning",
          icon: "ti-server",
          title: "Reservas Campo",
          value: 0,
          footerText: "Updated now",
          footerIcon: "ti-reload",
        },
        {
          type: "success",
          icon: "ti-wallet",
          title: "Dinheiro de Reservas",
          value: 0,
          footerText: "Last day",
          footerIcon: "ti-calendar",
        },
        {
          type: "danger",
          icon: "ti-pulse",
          title: "Aulas",
          value: 0,
          footerText: "In the last hour",
          footerIcon: "ti-timer",
        },
        {
          type: "info",
          icon: "ti-twitter-alt",
          title: "Utilizadores",
          value: 0,
          footerText: "Updated now",
          footerIcon: "ti-reload",
        },
      ],
      usersChart: {
        data: {
          labels: [], // Utilizador names
          series: [], // Number of raquetes per utilizador
        },
        options: {
          axisX: {
            showGrid: false,
          },
          axisY: {
            showGrid: true,
          },
          lineSmooth: Chartist.Interpolation.simple({
            divisor: 3,
          }),
          showLine: true,
          showPoint: false,
        },
      },
      activityChart: {
    data: {
    labels: [],
    series: [[]],
    },
    options: {
    seriesBarDistance: 10,
    axisX: {
      showGrid: false,
    },
    height: "245px",
  },
},

      preferencesChart: {
        data: {
          labels: ["Campo 1", "Campo 2", "Campo 3", "Campo 4"],
          series: [0, 0, 0, 0],
        },
        options: {},
      },
    };
  },
  
  mounted() {
    this.updateReservasAceite();
    this.carregarNumeroReservasCampo();
    this.calcularDinheiroReservas();
    this.calcularNumeroAulas();
    this.updateReservasAceite();
     this.updateActivityChart();
    this.carregarDadosLocalStorage();
    this.carregarReservas();
    this.updateUsersChart();
  const numberOfRaquetesPorUtilizador = this.getNumberOfRaquetesPorUtilizador();
   this.usersChart.data.series = [Object.values(numberOfRaquetesPorUtilizador)];
  },
  methods: {
    updateReservasAceite() {
      const reservasAceiteLocalStorage = localStorage.getItem("reservaAceite");
      if (reservasAceiteLocalStorage) {
        const reservasAceite = JSON.parse(reservasAceiteLocalStorage);
        const reservasCampoCard = this.statsCards.find(
          (card) => card.title === "Reservas Campo"
        );
        if (reservasCampoCard) {
          reservasCampoCard.value = reservasAceite.length;
        }
      }
    },
    calcularDinheiroReservas() {
      const reservasLocalStorage = JSON.parse(localStorage.getItem("reservas"));
      if (reservasLocalStorage) {
        const dinheiroReservasCard = this.statsCards.find(
          (card) => card.title === "Dinheiro de Reservas"
        );
        if (dinheiroReservasCard) {
          const totalDinheiro = reservasLocalStorage.reduce(
            (total, reserva) => total + reserva.valor,
            0
          );
          dinheiroReservasCard.value = totalDinheiro.toFixed(2);
        }
      }
    },
    calcularNumeroAulas() {
      const aulasLocalStorage = JSON.parse(localStorage.getItem("aulas"));
      if (aulasLocalStorage) {
        const aulasCard = this.statsCards.find((card) => card.title === "Aulas");
        if (aulasCard) {
          aulasCard.value = aulasLocalStorage.length;
        }
      }
    },
    carregarNumeroReservasCampo() {
      const reservasLocalStorage = JSON.parse(localStorage.getItem("reservas"));
      if (reservasLocalStorage) {
        const reservasCampoCard = this.statsCards.find(
          (card) => card.title === "Reservas Campo"
        );
        if (reservasCampoCard) {
          const numeroReservasCampo = reservasLocalStorage.filter(
            (reserva) => reserva.tipo === "campo"
          ).length;
          reservasCampoCard.value = numeroReservasCampo;
        }
      }
    },
    carregarDadosLocalStorage() {
      const utilizadoresLocalStorage = JSON.parse(
        localStorage.getItem("utilizadores")
      );
      if (utilizadoresLocalStorage) {
        const utilizadoresCard = this.statsCards.find(
          (card) => card.title === "Utilizadores"
        );
        if (utilizadoresCard) {
          utilizadoresCard.value = utilizadoresLocalStorage.length;
        }
      }
    },
    getNumberOfRaquetesPorUtilizador() {
    const utilizadoresLocalStorage = localStorage.getItem("utilizadores");
    if (utilizadoresLocalStorage) {
      const utilizadores = JSON.parse(utilizadoresLocalStorage);
      const numberOfRaquetesPorUtilizador = {};
      for (const utilizador of utilizadores) {
        const userId = utilizador.id;
        const reservasLocalStorage = localStorage.getItem("reservas");
        if (reservasLocalStorage) {
          const reservas = JSON.parse(reservasLocalStorage);
          const numberOfRaquetes = reservas.filter(
            (reserva) => reserva.userId === userId
          ).length;
          numberOfRaquetesPorUtilizador[userId] = numberOfRaquetes;
        }
      }
      return numberOfRaquetesPorUtilizador;
    }
    return {};
  },
    carregarReservas() {
      // Load reservas data from localStorage or API
      const reservasLocalStorage = localStorage.getItem('reservas');
      if (reservasLocalStorage) {
        this.reservas = JSON.parse(reservasLocalStorage);
      }
    },
    updateUsersChart() {
      const utilizadorStats = {};

      for (const reserva of this.reservas) {
        if (utilizadorStats.hasOwnProperty(reserva.username)) {
          utilizadorStats[reserva.username] += reserva.numRaquetes;
        } else {
          utilizadorStats[reserva.username] = reserva.numRaquetes;
        }
      }

      // Update the usersChart data
      this.usersChart.data.labels = Object.keys(utilizadorStats);
      this.usersChart.data.series = [Object.values(utilizadorStats)];
    },
  },

   updateReservasAceite() {
    const reservasAceiteLocalStorage = localStorage.getItem("reservaAceite");
    if (reservasAceiteLocalStorage) {
      const reservasAceite = JSON.parse(reservasAceiteLocalStorage);
      const reservasCampoCard = this.statsCards.find(
        (card) => card.title === "Reservas Campo"
      );
      if (reservasCampoCard) {
        reservasCampoCard.value = reservasAceite.length;
      }
    }
  },
  updateActivityChart() {
    const reservasAceiteLocalStorage = localStorage.getItem("reservaAceite");
    if (reservasAceiteLocalStorage) {
      const reservasAceite = JSON.parse(reservasAceiteLocalStorage);
      const horasMaisEscolhidas = this.getHorasMaisEscolhidas(reservasAceite);
      this.activityChart.data.labels = horasMaisEscolhidas.labels;
      this.activityChart.data.series = [horasMaisEscolhidas.series];
    }
  },
  getHorasMaisEscolhidas(reservasAceite) {
    const horas = {};
    for (const reserva of reservasAceite) {
      const hora = reserva.horario.split(" ")[1];
      if (horas.hasOwnProperty(hora)) {
        horas[hora] += 1;
      } else {
        horas[hora] = 1;
      }
    }
    const sortedHoras = Object.keys(horas).sort();
    const labels = sortedHoras.map((hora) => `${hora}AM`);
    const series = sortedHoras.map((hora) => horas[hora]);
    return { labels, series };
  },
};
</script>

<style scoped>
/* Chart Card Styles */
.chart-card {
  background-color: #ffffff;
  border-radius: 4px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
}

.chart-card-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 10px;
}

.chart-card-subtitle {
  font-size: 14px;
  color: #999999;
}

.chart-card-footer {
  font-size: 12px;
  color: #999999;
  margin-top: 15px;
}

/* Chart Styles */
.ct-chart {
  width: 100%;
  height: 300px;
}

/* Chart Legend Styles */
.chart-legend {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 15px;
}

.chart-legend-item {
  display: flex;
  align-items: center;
  margin-right: 15px;
}

.chart-legend-icon {
  width: 10px;
  height: 10px;
  margin-right: 5px;
}

.chart-legend-label {
  font-size: 12px;
  color: #999999;
}

/* Utility Classes */
.text-info {
  color: #17a2b8;
}

.text-danger {
  color: #dc3545;
}

.text-warning {
  color: #ffc107;
}
</style>

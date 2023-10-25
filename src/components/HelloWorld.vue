<template>
  <div class="container">
    <div class="chart">
      <div class="chart-title">Work Monitoring</div>
      <canvas id="myChart" width="800" height="400"></canvas>
    </div>
    <div class="form">
      <label for="pushvalue">Nilai Sigmoid:</label>
      <div class="input-group">
        <input type="number" id="pushvalue" required v-model="inputValue">
        <button id="addValueButton" @click="addValue">Tambahkan</button>
      </div>
      <div style="color: red;" class="error-notification" v-if="errorNotification">
        Nilai sigmoid tidak boleh lebih dari 100.
      </div>
    </div>
  </div>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Bulan</th>
          <th>Plan</th>
          <th>Actual</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(record, index) in records" :key="index">
          <td>{{ record.month }}</td>
          <td>{{ record.plan }}</td>
          <td>{{ record.actual }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import Chart from 'chart.js/auto';

export default {
  setup() {
    const inputValue = ref(null);
    const errorNotification = ref(false);
    const records = ref([]);
    let monthIndex = 0;

    onMounted(() => {
      const chartData = {
        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Agu', 'Sep', 'Okt', 'Nov', 'Des'],
        datasets: [
          {
            label: 'Plan',
            data: Array(12).fill(null),
            borderColor: 'rgba(0, 255, 68)',
            borderWidth: 2,
            backgroundColor: 'rgba(0, 123, 255, 0.2)',
          },
          {
            label: 'Actual',
            data: Array(12).fill(null),
            borderColor: 'rgba(255, 0, 0, 1)',
            borderWidth: 2,
            backgroundColor: 'rgba(255, 0, 0, 0.2)',
          },
        ],
      };

      const ctx = document.getElementById('myChart').getContext('2d');
      const chart = new Chart(ctx, {
        type: 'line',
        data: chartData,
        options: {
          plugins: {
            legend: {
              display: true,
            },
            tooltips: {
              callbacks: {
                label: (tooltipItem) => {
                  return tooltipItem.value + '%';
                },
              },
            },
          },
          scales: {
            x: {
              beginAtZero: true,
            },
            y: {
              title: {
                display: true,
                text: 'Progress'
              },
              suggestedMin: 0,
              suggestedMax: 100,
              ticks: {
                stepSize: 5,
                beginAtZero: true,
                callback: function(value) {
                  return value + '%';
                }
              }
            },
          },
          elements: {
            line: {
              tension: 0.4,
            },
          },
        },
      });

      const addValue = () => {
        const value = parseFloat(inputValue.value);
        if (!isNaN(value) && monthIndex < 12) {
          if (value > 100) {
            errorNotification.value = true;
          } else {
            errorNotification.value = false;
            if (chart.data.datasets[0].data[monthIndex] === null) {
              chart.data.datasets[0].data[monthIndex] = value;
            } else {
              chart.data.datasets[1].data[monthIndex] = value;
              records.value.push({
                month: getMonthName(monthIndex),
                plan: chart.data.datasets[0].data[monthIndex],
                actual: chart.data.datasets[1].data[monthIndex],
              });
              monthIndex++;
            }
            chart.update();
            inputValue.value = '';
          }
        }
      };

      const getMonthName = (index) => {
        const monthNames = [
          'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Agu', 'Sep', 'Okt', 'Nov', 'Des'
        ];
        return monthNames[index];
      };

      const pushValueButton = document.getElementById('addValueButton');
      pushValueButton.addEventListener('click', addValue);
    });

    return { inputValue, errorNotification, records };
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;
  flex-direction: column; /* Menambahkan flex-direction agar konten tampil di tengah vertikal */
}

.chart {
  background: linear-gradient(to bottom, #FFFFFF, #E6ECF0);
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  text-align: center;
  margin-bottom: 20px; /* Menambahkan margin bawah untuk mengatur jarak dengan form */
}

.chart-title {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #007BFF;
}

.form {
  background: linear-gradient(to bottom, #E6ECF0, #FFFFFF);
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  text-align: center;
}

.label {
  font-weight: bold;
  margin-bottom: 5px;
}

.input-group {
  display: flex;
  align-items: center;
  justify-content: center;
}

input {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  margin-right: 10px;
}

button {
  background: linear-gradient(to bottom, #007BFF, #0056b3);
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background: linear-gradient(to bottom, #0056b3, #007BFF);
}

/* Tambahkan gaya CSS untuk tabel */
.table-container {
  margin-top: 20px;
  width: 100%; /* Menambahkan lebar penuh ke tabel */
  overflow-x: auto; /* Membuat scroll horizontal jika diperlukan */
}

table {
  border-collapse: collapse;
  width: 100%; /* Menambahkan lebar penuh ke tabel */
}

th, td {
  padding: 8px;
  text-align: center;
  border: 1px solid #ccc;
}

th {
  background-color: #007BFF;
  color: #fff;
  font-weight: bold;
}

tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}
</style>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<canvas id="myChart"></canvas>


import Chart from 'chart.js/auto';

async function renderChart() {
  const data = await fetchPengunjung();

  const labels = data.map(item => item.bulan);
  const pengunjung = data.map(item => item.jumlah_pengunjung);

  const ctx = document.getElementById('myChart').getContext('2d');
  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: labels,
      datasets: [{
        label: 'Jumlah Pengunjung',
        data: pengunjung,
        backgroundColor: 'rgba(75, 192, 192, 0.2)',
        borderColor: 'rgba(75, 192, 192, 1)',
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true
        }
      }
    }
  });
}

renderChart();

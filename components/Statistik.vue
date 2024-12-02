<template>
    <div>
        <canvas id="statistik"></canvas>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const labels = ref([]);
const pengunjung = ref([]);

async function fetchData() {
  const { data, error } = await supabase
    .from('pengunjung') // Ganti dengan nama tabel Anda
    .select('*');

  if (error) {
    console.error('Error fetching data:', error);
    return;
  }

  // Map data ke format untuk Chart.js
  labels.value = data.map(item => item.bulan);
  pengunjung.value = data.map(item => item.jumlah_pengunjung);
}
onMounted(async () => {
  await fetchData();

  const data = {
    labels: labels.value,
    datasets: [{
      label: 'Pengunjung',
      data: pengunjung.value,
      backgroundColor: 'rgb(0, 120, 255)',
    }]
  };

  const config = {
    type: 'bar',
    data: data,
    options: {}
  };

  const myChart = new Chart(
    document.getElementById('statistik'),
    config
  );
});

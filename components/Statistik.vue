<!DOCTYPE html>
<html lang="en">
<head>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>
</head>
<body>
  <canvas id="myChart"></canvas>

  <script>
    // Sambungkan ke Supabase
    const supabaseUrl = 'https://mphyrckwcmayulgshugc.supabase.co'; // Ganti dengan URL Supabase Anda
    const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1waHlyY2t3Y21heXVsZ3NodWdjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTYyNTk1ODEsImV4cCI6MjAzMTgzNTU4MX0.R5FgpY0e7OTlafsZBQbGC3qqvxaZsYM8lmxxIT7MCG0'; // Ganti dengan API Key Anda
    const supabase = supabase.createClient(supabaseUrl, supabaseKey);

    // Ambil data dari Supabase dan buat chart
    async function renderChart() {
      const { data, error } = await supabase.from('pengunjung').select('*');
      if (error) return console.error(error);

      const labels = data.map(item => item.bulan);
      const values = data.map(item => item.jumlah_pengunjung);

      const ctx = document.getElementById('myChart').getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: 'Jumlah Pengunjung',
            data: values,
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: { beginAtZero: true }
          }
        }
      });
    }

    renderChart();
  </script>
</body>
</html>

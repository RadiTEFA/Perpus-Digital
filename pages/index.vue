<template>
  <div class="container-fluid">
    <div class="row my-5 d-flex justify-content-evenly ">
      <div class="col-lg-6 box">
        <NuxtLink to="/pengunjung/tambah">
          <div class="card bg-pengunjung rounded-5">
            <div class="card-body">
              <div class="Radi">
                <h2>Pengunjung</h2>
              </div>
            </div>
          </div>
        </NuxtLink>
      </div>
      <div class="col-lg-6 box">
        <NuxtLink to="/buku">
          <div class="card bg-buku rounded-5">
            <div class="card-body">
              <div class="Radi">
                <h2>Buku</h2>
              </div>
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
    <div class="statistik">
    <h1>STATISTIK</h1>
    </div>
      <div class="row my-5 d-flex justify-content-evenly ">
        <div class="col-lg-6 box">
          <NuxtLink to="http://localhost:3000/Pengunjung">
            <div class="card b2 rounded-5">
              <div class="card-body text">
                <h1><span class="no">{{ jml_pengunjung}}</span></h1>
                <p class="yes">Pengunjung</p>
              </div>
            </div>
          </NuxtLink>
        </div>
        <div class="col-lg-6 box">
          <NuxtLink to="https://perpus-digital-rho.vercel.app/Buku">
            <div class="card b3 rounded-5">
              <div class="card-body text">
                <h1 class="no">{{ jml_buku}}</h1>
                <p class="yes">Buku</p>
            </div>
          </div>
          </NuxtLink>
        </div>
      </div>
      <div class="container">
        <div class="row">
          <Statistik />
        </div>
      </div>
    </div>
  </template>


<script setup>
useHead({ title: "Home/Perpustakaan Digital" })
const supabase = useSupabaseClient()
const jml_pengunjung = ref(0)
const jml_buku = ref(0)


async function getjml_pengunjung() {
  const{ error , data, count } = await supabase
  .from("pengunjung")
  .select('*', { count: 'exact' })
  if (count) jml_pengunjung.value = count
}
async function getjml_buku() {
  const{ error , data, count } = await supabase
  .from("buku")
  .select('*', { count: 'exact' })
  if (count) jml_buku.value = count
}

onMounted(() => {
  getjml_pengunjung()
  getjml_buku()
})
</script>

<style scoped>
.card {
  height: 250px;
}
.card.bg-pengunjung{
  background-image: url('../assets/img/bg-home-kunjungan.jpeg');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  color: black;
}
.card.bg-buku {
  background: url('../assets/img/bg-home-cari-buku.jpg') no-repeat center center;
  background-size: cover;
  color: black;
}
.statistik {
  color: rgba(0, 0, 0, 0.761);
  margin-left: 30px;
}
.box{
  width: 45%;
}
.box a{
  text-decoration: none;
}
.b2{
  background-color: #ff8724f2;
}
.b3{
  background-color: rgba(72 209 204);
}
.text{
  display: flex;
  justify-content: center;
  align-items: center;
}
.no{
  font-size: 70px;
  color:black;
}
.yes{
  font-family: 'Times New Roman', Times, serif;
  font-size: 25px;
  padding-top: 40px;
  color:black;
}
.Radi{
  color: rgb(255, 255, 255);
  font-family: 'Times New Roman', Times, serif;
  text-shadow: 2px 2px 0px rgb(0, 0, 0);
}
</style>

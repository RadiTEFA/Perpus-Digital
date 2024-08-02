<template>
  <div class="container-fluid">
      <div class="row">
          <div class="col-lg-12">
              <h2 class="text-center my-4 tulisan">Riwayat kunjungan</h2>
              <div class="my-3">
                <form @submit.prevent="getCari">
                    <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Fiter...">
                </form>
              </div>
               <div class="my-3 text-muted">Menampilkan {{ saya?.length }} dari {{ TotalPengunjung }}</div>
               <table class="table table-striped tulisan">
                  <thead>
                      <tr>
                          <td>NO</td>
                          <td>NAMA</td>
                          <td>KEANGGOTAAN</td>
                          <td>WAKTU</td>
                          <td>TINGKAT</td>
                          <td>JURUSAN</td>
                          <td>KELAS</td>
                          <td>KEPERLUAN</td>
                      </tr>
                  </thead>
                  <tbody>
                      <tr v-for="(visitor,i) in visitors" :key="i">
                          <td>{{ 1+i }}.</td>
                          <td>{{ visitor.nama }}</td>
                          <td>{{ visitor.keanggotaan.nama }}</td>
                          <td>{{ visitor.tanggal }} {{ visitor.waktu }}</td>
                          <td>{{ visitor.tingkat }}</td>
                          <td>{{ visitor.jurusan }}</td>
                          <td>{{ visitor.kelas }}</td>
                          <td>{{ visitor.keperluan.nama }}</td>
                      </tr>
                  </tbody>
               </table>
          </div>
      </div>
      <NuxtLink to="/Pengunjung/tambah">
          <button type="submit" class="btn btn-dark btn-lg rounded-5 px-5 tulisan">Kembali</button>
      </NuxtLink>
         
      
  </div>
</template>
<script setup>

const supabase = useSupabaseClient()
const keyword = ref('')
const visitors = ref([])
const TotalPengunjung = ref(0);
const saya = ref(visitors)

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
  if(data) visitors.value = data
}
const getCari = async () => {
    const { data, error } = await supabase.from('pengunjung').select(`*, keanggotaan(*), keperluan(*)`)
    .ilike('nama', `%${keyword.value}%`)
    if(data) visitors.value = data
}
const getTotalPengunjung = async () => {
  const { count, error } = await supabase.from("pengunjung").select("*, keanggotaan(*)", { count: 'exact', head: true});
  if (count) TotalPengunjung.value = count;
};

onMounted(() => {
  getPengunjung()
  getCari()
  getTotalPengunjung()
})

</script>

<style scoped>
.btn{
  background-color: #D9D9D9;
  color: black;
  font-family: 'Times New Roman';
}
.tulisan{
  font-family: 'Times New Roman', Times, serif;
}
</style>

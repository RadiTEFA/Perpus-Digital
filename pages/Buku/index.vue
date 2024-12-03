<template>
  <div class="container-fluid">
    <div class="row"> 
      <div class="col-lg-12">
        <div class="my-3">
          <form @submit.prevent="getBuku">
          <input
            v-model="keyword"
            type="search"
            class="form-control rounded-5"
            placeholder="Mau baca apa hari ini?"
            />
          </form>
        </div>
        <div class="my-3 text-muted">Menampilkan {{ books?.length }} dari {{ TotalBuku }}</div>
        <div class="row justify-content-evenly">
          <div v-for="(buku, i) in books" :key="i" class="col-lg-2">
          <nuxt-link :to="`/buku/${buku.id}`">
              <div class="card mb-3 shadow-lg">
                <div class="card-body">
                  <img :src="buku.cover" class="cover" :alt="buku.judul" />
                </div>
              </div>
            </nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
    <NuxtLink to="/">
      <button type="submit" class="btn btn-dark btn-lg rounded-5 px-5 tulisan">Kembali</button>
    </NuxtLink>
</template>

<script setup>
const supabase = useSupabaseClient();
const TotalBuku = ref(0);
const books = ref([])

const getBuku = async () => {
  const { data, error} = await supabase
  .from('buku')
  .select(`*,kategori(*)`)
  .ilike("judul", `%${keyword.value}%`);
  if(data) books.value= data;
};
const getTotalBuku = async () => {
  const { count, error } = await supabase.from("buku").select("*, kategori(*)", { count: 'exact', head: true});
  if (count) TotalBuku.value = count;
};

onMounted(() => {
  getBuku();
  getTotalBuku();
});

const keyword = ref("");
</script>

<style scoped>
.shadow-lg {
  box-shadow: 6px 4px 0 #2e2e2eae !important;
}
.card:hover {
  transform: scale(1.05);
  box-shadow: 4px 4px 20px #2e2e2eae !important;
}
.card {
  transition: all .2s ease-in-out;
  width: 100%;
}
.btn{
  background-color: rgb(68, 80, 252);
  color: black;
  font-family: 'Times New Roman';
}
.card-body {
  width: 100%;
  height: 100%;
  height: 28rem;
  padding : 0;
}
.cover {
  width: 100%;
  object-fit: cover;
  object-position: 0 30;
}
.form-control {
  background-color: #D9D9D9;
}
.btn {
  background-color: #D9D9D9;
}
.tulisan{
  margin-left: 20px;
  font-family: 'Times New Roman', Times, serif;
  background-color: #D9D9D9;
}
</style>

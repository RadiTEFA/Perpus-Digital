<template>
  <div class="rincian">
      <h2 class="text-start my-4 tulisan">{{ buku.judul }}</h2>
      <div class="row">
          <div class="col-md-3">
              <div class="card">
                  <div class="card-body">
                      <span v-if="buku.cover"><img class="cover" :src="buku.cover" :alt="buku.judul"></span>
                      <span v-else><img src="https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.svgrepo.com%2Fsvg%2F508699%2Flandscape-placeholder&psig=AOvVaw2-SWmfk33NzXubPfqn0P16&ust=1714794757874000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCNjln7nK8IUDFQAAAAAdAAAAABAE://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.svgrepo.com%2Fsvg%2F508699%2Flandscape-placeholder&psig=AOvVaw2-SWmfk33NzXubPfqn0P16&ust=1714794757874000&source=images&cd=vfe&opi=89978449&ved=0CBAQjRxqFwoTCNjln7nK8IUDFQAAAAAdAAAAABAE" class="cover"></span>
                  </div>
              </div>
          </div>
          <div class="col-md-6">
              <ul class="list-group list-group-flush tulisan">
                  <li class="list-group-item"><b>Penulis : </b>{{  buku.penulis }}</li>
                  <li class="list-group-item"><b>Tahun Terbit : </b>{{  buku.tahun_terbit }}</li>
                  <li class="list-group-item"><b>Rak : </b>{{ buku.rak }}</li>
                  <li class=" list-group-item"><b>Deskripsi : </b>{{ buku.deskripsi }}</li>
              </ul>
          </div>
      </div>
      <br>
      <div>
        <NuxtLink to="/buku">
          <button type="submit" class="btn-dark btn-lg rounded-5 px-5">Kembali</button>
        </NuxtLink>
      </div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';

const supabase = useSupabaseClient()
const route = useRoute()
const buku = ref([])

const getBukuByID = async () => {
  const { data, error } = await supabase
  .from('buku')
  .select(`*, kategori_buku(*)`)
  .eq('id', route.params.id)
  .single()
  if(data) buku.value = data
}

onMounted(() => {
  getBukuByID()
})
</script>
<style scoped>
.cover{
  width: 100%;
}
.rincian{
  margin-left: 40px;
}
.tulisan{
  font-family: 'Times New Roman', Times, serif;
}
</style>

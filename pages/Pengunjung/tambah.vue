<template>
  <div class="container-fluid">
      <div class="row">
          <div class="col-lg-12">
              <h2 class="text-center my-4 judul">Form Kunjungan</h2>
              <form @submit.prevent="kirimData">
                  <div class="mb-3">
                      <input v-model="form.nama" class="form-control form-control-lg rounded-5 nama" type="text" placeholder="Nama"/>
                  </div>
                  <div class="mb-3">
                      <select v-model="form.keanggotaan"  class="form-control form-control-lg form-select rounded-5 keanggotaan">
                          <option value="">Keanggotaan</option>
                          <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
                      </select>
                  </div>
                  <div v-if="form.keanggotaan == 2" class="mb-3">
                      <div class="row">
                          <div class="col-md-4">
                              <select v-model="form.tingkat" class="form-control form-control-lg form-select rounded-5 mb-2 tingkat">
                                  <option value="">Tingkat</option>
                                  <option value="X">X</option>
                                  <option value="XI">XI</option>
                                  <option value="XII">XII</option>
                              </select>
                          </div>
                          <div class="col-md-4">
                              <select v-model="form.jurusan" class="form-control form-control-lg form-select rounded-5 mb-2 jurusan">
                                  <option value="">Jurusan</option>
                                  <option value="PPLG">PPLG</option>
                                  <option value="TJKT">TJKT</option>
                                  <option value="TSM">TSM</option>
                                  <option value="DKV">DKV</option>
                                  <option value="TOI">TOI</option>
                              </select>
                          </div>
                          <div class="col-md-4">
                              <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2 kelas">
                                  <option value="">Kelas</option>
                                  <option value="1">1</option>
                                  <option value="2">2</option>
                                  <option value="3">3</option>
                                  <option value="4">4</option>
                              </select>
                          </div>
                      </div>
                  </div>
                  <div class="mb-3">
                      <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5 mb-2 keperluan">
                          <option value="">Keperluan</option>
                          <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
                      </select>
                  </div>
                     
                          <button type="submit" class="btn btn-dark btn-lg rounded-5 px-5">Kirim</button>
                     
              </form>
          </div>
      </div>
  </div>
</template>
<script setup>
const supabase = useSupabaseClient()

const members = ref([]);
const objectives = ref([]);

const form = ref({
  nama: "",
  keanggotaan:"",
  tingkat:"",
  jurusan:"",
  kelas:"",
  keperluan:"",
});

const kirimData = async () => {
  console.log(form.value)
  const { error } = await supabase.from("pengunjung").insert([form.value])
  if(!error) navigateTo("/Pengunjung")
  else throw error
}

const getkeanggotaan = async () => {
  const { data, error } = await supabase.from("keanggotaan").select("*")
  if(data) members.value = data
};

const getkeperluan = async () => {
  const { data, error } = await supabase.from("keperluan").select("*")
  if(data) objectives.value = data
};

onMounted(() => {
  getkeanggotaan();
  getkeperluan();
});
</script>
<style scoped>
.btn{
  background-color: rgb(68, 80, 252);
  color: black;
  font-family: 'Times New Roman';
}
.nama::placeholder{
  font-family: 'Times New Roman', Times, serif;
  color: black;
  font-size: 18px;
}
.keanggotaan{
  font-family: 'Times New Roman', Times, serif;
  padding-left: 18px;
  font-size: 18px;
}
.tingkat{
  font-family: 'Times New Roman', Times, serif;
  padding-left: 18px;
}
.jurusan{
  font-family: 'Times New Roman', Times, serif;
  padding-left: 18px;
}
.kelas{
  font-family: 'Times New Roman', Times, serif;
  padding-left: 18px;
}
.keperluan{
  font-family: 'Times New Roman', Times, serif;
  padding-left: 18px;
  font-size: 18px;
}
.judul{
  font-family: 'Times New Roman', Times, serif;
}
</style>
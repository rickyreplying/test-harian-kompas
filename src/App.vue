<template>
  <div class="container mx-auto px-5 font-mono">
    <div class="mt-5 font-bold text-center">
      <h1 class="text-md lg:text-3xl sm:text-md">Diari Jajan Februari 2021</h1>
      <h2 class="text-md lg:text-2xl sm:text-md">Pengeluaran Bulan Ini Rp {{total}}</h2>
    </div>

    <div class="flex justify-center mt-5">
      <button @click="toggleModal()" class="rounded-lg bg-indigo-900 py-2 px-4 text-white font-mono ">Tambah
        Item</button>
    </div>
    <div v-if="showModal"
      class="overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex">
      <div class="relative w-auto my-6 mx-auto max-w-xl">
        <!--content-->
        <div
          class="border-0 rounded-lg shadow-lg relative flex flex-col w-full bg-white outline-none focus:outline-none">
          <!--header-->
          <div class="flex items-start justify-between p-5 border-b border-solid border-slate-200 rounded-t">
            <h1 class="text-xl font-semibold">
              Tambah Entri
            </h1>
          </div>
          <!--body-->
          <div class="relative p-6 flex-auto">
            <form @submit.prevent="addData">
              <input type="hidden" v-model="form.id">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="nama">
                Nama
              </label>
              <input v-model="form.nama"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight" id="nama"
                type="text">

              <label class="block text-gray-700 text-sm font-bold mb-2" for="harga">
                Harga
              </label>
              <input v-model="form.pengeluaraan"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight" id="pengeluaraan"
                type="number">

              <label class="block text-gray-700 text-sm font-bold mb-2" for="jam">
                Jam
              </label>
              <input v-model="form.jam"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight" id="jam"
                type="time">

              <label class="block text-gray-700 text-sm font-bold mb-2" for="jam">
                Tanggal
              </label>
              <input v-model="form.tanggal"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight" id="tanggal"
                type="text">


              <div class="flex items-center justify-end p-6 border-t border-solid border-slate-200 rounded-b">
                <button class="text-white bg-gray-500 text-sm px-6 mx-2 py-3 rounded" type="button"
                  v-on:click="toggleModal()">
                  Batal
                </button>
                <button class="text-white bg-indigo-900 text-sm px-6 mx-2 py-3 rounded" type="submit">
                  Kirim
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="opacity-25 fixed inset-0 z-40 bg-black">
    </div>

      <div class="mt-10 grid lg:grid-cols-4 md:grid-cols-2 gap-4">
        <div v-for="detail in detailList" :key="detail.id">
          <Card :detail="detail" />
        </div>
      </div>
    </div>






</template>



<script>
  import Card from './components/Card.vue'

  import axios from 'axios';
  export default {
    components: {
      Card
    },
    data() {
      return {
        showModal: false,
        form: {
          id: '',
          jam: '',
          tanggal: '',
          nama: '',
          pengeluaraan: '',
        },
        total: "",
        detailList: []
      }
    },
    mounted() {
      this.loadData()
    },
    methods: {
      toggleModal: function () {
        this.showModal = !this.showModal;
      },
      loadData() {
        axios.get('http://localhost:3000/detail').then(respond => {
          this.detailList = respond.data;

          //group array berdasarkan tanggal
           const result = Object.entries(respond.data.reduce((acc, { tanggal, jam, nama, pengeluaraan }) => {
              acc[tanggal] = [...(acc[tanggal] || []), { jam, nama, pengeluaraan }];
              return acc;
            }, {})).map(([key, value]) => ({ tanggal: key, keterangan: value }));
            this.detailList = result;
            console.log(result);


             //menghitung total pengeluaran
            this.total = result.reduce((acc, curr)=> {
              return acc+curr.keterangan.reduce((a,c) => a+c.pengeluaraan,0)
            },0);
            this.total = this.total.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }).catch((error) => {
          console.log(error);
        })
      },
      addData() {
        axios.post('http://localhost:3000/detail/', this.form).then(res => {
          this.loadData()
          this.form.nama = '';
          this.form.pengeluaraan = '';
          this.form.jam = '';
          this.form.tanggal = '';
          this.toggleModal();
        })
      },
    }
  }
</script>
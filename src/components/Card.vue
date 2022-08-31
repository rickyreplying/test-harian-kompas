<template>
    <div class="p-2 w-64 lg:w-80 sm:w-48 max-w-sm bg-white rounded-lg border shadow-md lg:p-4">
        <div class="flex items-center mb-2 divide-y divide-solid">
            <h1 class="text-md lg:text-xl sm:text-md font-bold text-gray-800 ">{{detail.tanggal}}</h1>
        </div>
        <div>
            <ul class="divide-y divide-solid divide-gray-200 text-sm">
                <li v-for="detail in detail.keterangan">
                    <div class="flex">
                        <div class="flex-auto w-28">
                            {{detail.jam}}
                        </div>
                        <div class="flex-auto w-80">
                            {{detail.nama}}
                        </div>
                        <div class="flex-auto w-48">
                            Rp {{detail.pengeluaraan.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")}}
                        </div>
                    </div>
                </li>

            </ul>
        </div>
    </div>


</template>

<script>
    import axios from "axios";
    export default {
        name: "Card",
        props: ["detail"],
        data() {
            return {
                total: ""
                
            }
        },
        
        methods: {
            // hitungTotal() {
            //     return console.log(detail);
            // },
        },
    }

    
</script>
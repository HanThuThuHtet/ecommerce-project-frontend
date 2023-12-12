<template>
    <div class="container">
        <div class="row my-5">
            <div class="col-12">
                <h1>Product List</h1>
                <hr>
                <div class="d-flex justify-content-between">
                    <Pagination 
                    v-if="rows.meta" 
                    @fetchLink="fetchProducts"
                    :links="rows.meta.links" />
                    <div class="w-25">
                        <Search />
                        <div class="input-group">
                            <input class="form-control" type="text">
                            <button class="btn btn-primary">
                                <i class="bi bi-search"></i>
                            </button>
                        </div>
                    </div>
                </div>
                <table class="table text-end align-middle">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Name</th>
                            <th>Price</th>
                            <th>Stock</th>
                            <th>Date</th>
                            <th>Time</th>
                            <th>Controls</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="row in rows.data">
                            <td>{{ row.id }}</td>
                            <td>{{ row.name }}</td>
                            <td>{{ row.price }}</td>
                            <td>{{ row.stock }}</td>
                            <td>{{ row.date }}</td>
                            <td>{{ row.time }}</td>
                            <td>
                                <div class=" btn-group">
                                    <button @click="fetchProduct(row.id)" class="btn btn-sm btn-outline-primary">
                                        <i class="bi bi-info-circle"></i>
                                    </button>
                                    <router-link :to="'/product/edit/'+row.id"  class="btn btn-sm btn-outline-primary">
                                        <i class="bi bi-pencil"></i>
                                    </router-link>
                                    <button @click="deleteProduct(row.id)" class="btn btn-sm btn-outline-primary">
                                        <i class="bi bi-trash3"></i>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
                
            </div>
        </div>
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
import axios from 'axios';
import Pagination from '@/components/Pagination.vue';
import Search from '@/components/Search.vue';
import Swal from "sweetalert2";
export default{
    data() {
        return {
            rows: {}
        };
    },
    computed: {
        ...mapGetters([
            'getUrl'
        ])
    },
    methods: {
        showToast(icon,message){
            const Toast = Swal.mixin({
                        toast: true,
                        position: 'top-end',
                        showConfirmButton: false,
                        timer: 3000,
                        timerProgressBar: true,
                        didOpen: (toast) => {
                            toast.addEventListener('mouseenter', Swal.stopTimer)
                            toast.addEventListener('mouseleave', Swal.resumeTimer)
                        }
                        })

                        Toast.fire({
                        icon: icon,
                        title: message
                        })
        },
        fetchProducts(url) {
            axios.get(url)
                .then(res => this.rows = res.data);
            //.then(res => console.log(res))
        },
        fetchProduct(id){
            axios.get(this.getUrl('/products/')+id)
                .then(res => console.log(res))
        },
        deleteProduct(id){
            axios.delete(this.getUrl('/products/')+id)
                .then(res => {
                    this.fetchProducts(this.rows.meta.links.find(link => link.active === true).url),
                    this.showToast('success',res.data.message)
                })
        }
    },
    mounted() {
        this.fetchProducts(this.getUrl('/products'));
    },
    components: { Pagination }
}
</script>

<style scoped>
</style>
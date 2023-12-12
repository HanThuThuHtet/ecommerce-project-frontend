<template>
    <div class="container">
        <div class="row my-4">
            <div class="col-12">
                <h1>Product Edit</h1>
                <hr>
                <form action="" @submit.prevent="updateProduct" ref="productEdit">
                    <div class="row">
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Product Name" name="name" :errors="errors" :value="product.name"  />
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Price" type="number" name="price"  :errors="errors" :value="product.price" />
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Stock" type="number" name="stock"  :errors="errors" :value="product.stock"/>
                            </div>
                        </div>
                        <div class="col-12 text-center">
                            <button :disabled="isLoading" class="btn btn-lg btn-primary">
                                <span v-if="isLoading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                                Update Product
                            </button>
                        </div>
                    </div>
                </form>
            </div>
        </div>

    </div>
</template>

<script>
import Input from '@/components/Input.vue'
import { mapGetters } from 'vuex';
import axios from 'axios';
import Swal from "sweetalert2";
export default{
    data(){
        return {
            errors: {},
            isLoading : false,
            product: {}
        }
    },
    components: {Input},
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
        updateProduct(){
            this.isLoading = true;
            let formData = new FormData(this.$refs.productEdit);
            axios.put(this.getUrl('/products/'+this.$route.params.id),new URLSearchParams(formData).toString())
            .then(res => 
            {
                this.errors = {};
                if(res.data.success === true){
                    this.showToast('success',res.data.message);
                    this.product = res.data.product;
                    this.$router.push('/product');
                }
            })
            .catch(e => {
                console.log(e);
                if(e.response.status === 422){
                    this.errors = e.response.data.errors;
                    this.showToast('error',e.response.data.message);
                }
            })
            .finally( onFinally => this.isLoading = false)
        },
        fetchFormValue(id){
            axios.get(this.getUrl('/products/'+id))
            .then(res => this.product = res.data.data);
        }
    },
    mounted(){
        this.fetchFormValue(this.$route.params.id);
    }
}
</script>

<style scoped>
</style>
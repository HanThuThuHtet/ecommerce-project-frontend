<template>
    <div class="container">
        <div class="row my-4">
            <div class="col-12">
                <h1>Product Create</h1>
                <hr>
                <form action="" @submit.prevent="addProduct" ref="productCreate">
                    <div class="row">
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Product Name" name="name" :errors="errors" />
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Price" type="number" name="price"  :errors="errors" />
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                                <Input label="Stock" type="number" name="stock"  :errors="errors"/>
                            </div>
                        </div>
                        <div class="col-12">
                            <div class="mb-4">
                                <label for="photos" class="form-label">Product Photo</label>
                                <input type="file" name="photos[]" id="photos" class="form-control " :errors="errors"  :class="{'is-invalid':errors.photos}"  multiple>
                                <div class="text-danger" v-if="errors.photos">{{ errors.photos[0] }}</div>
                            </div>
                        </div>
                        <div class="col-12 text-center">
                            <button :disabled="isLoading" class="btn btn-lg btn-primary">
                                <span v-if="isLoading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                                Add Product
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
            isLoading : false
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
        addProduct(){
            this.isLoading = true;
            let formData = new FormData(this.$refs.productCreate);
            axios.post(this.getUrl('/products'),formData)
            .then(res => 
            {
                this.errors = {};
                if(res.data.success === true){
                    this.showToast('success',res.data.message);
                    this.$refs.productCreate.reset();
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
        }
    },
}
</script>

<style scoped>
</style>
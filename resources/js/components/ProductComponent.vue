<template>
    <div>
        <div class="row no-gutters bg-light position-relative mb-2" v-for="(item, index) in products" :key="index">
            <div class="col-md-3 mb-md-0 p-md-3">
                <img :src="'data:image/jpeg;base64,'+`${item.photo}`" class="w-100" alt="Imagen">
            </div>
            <div class="col-md-9 position-static p-4 pl-md-0">
                <h5 class="mt-0">{{item.name}} <span class="badge badge-success font-weight-normal">{{item.category}}</span></h5>
                <p>{{item.description}}</p>
                <p>Precio: <b>$ {{formatPrice(item.price)}}</b></p>
                <p>{{item.quantity}} Unidades Disponibles</p>
                <a :href="'product/'+item.slug" class="btn btn-primary">Ficha Técnica</a>
                <span v-if="item.userId">
                    <a :href="'edit/'+item.slug" class="btn btn-success">Editar</a>
                    <button @click="deleteProduct(item, index)" class="btn btn-danger">Eliminar</button>
                </span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                products: [],
            }
        },
        created(){
            axios.get('/getProducts').then(res=>{
            this.products = res.data;
            })
        },
        methods: {
            formatPrice(value) {
                let val = (value/1).toFixed(2).replace('.', ',')
                return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
            },
            deleteProduct(product, index){
                const confirmacion = confirm(`Eliminar producto ${product.name}`);
                if(confirmacion){
                    axios.delete(`/delete/${product.id}`)
                    .then(()=>{
                        this.products.splice(index, 1);
                    })
                }
            },
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>

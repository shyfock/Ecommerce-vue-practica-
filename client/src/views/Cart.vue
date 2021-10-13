<template>
    <BasicLayouts>
        <h1>Carrito</h1>
        <table class="ui celled table" v-if="products">
            <thead>
                <tr>
                    <th>Producto</th>
                    <th>Cantidad</th>
                    <th>Precio</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="product in products" :key="product.id">
                    <td>{{ product.name }}</td>
                    <td>{{ product.quantity }}</td>
                    <td>{{ product.price }}</td>
                    <td style="text-align: center">
                        <i class="close icon" @click="deleteAllProductsCart(product.id)"></i>
                    </td>
                </tr>
                <tr>
                    <td></td>
                    <td>Total:</td>
                    <td colspan="2">{{ getTotal() }}€</td>
                </tr>
            </tbody>
        </table>
        <button class="ui button primary fluid">
            Generar Pedido
        </button>
        <h3 v-if="!products">No tiene productos en el carrito</h3>
    </BasicLayouts>
</template>

<script>
import { ref, onMounted, watchEffect } from 'vue'
import BasicLayouts from '../layouts/BasicLayouts.vue'
import { getProductsCartApi, deleteAllProductsCartApi } from '../api/cart' 
export default {
    name: 'Cart',
    components: {
        BasicLayouts,
    },
    setup() {
        let products = ref(null);
        let reloadCart = ref(false);
        watchEffect(async () => {
            reloadCart.value;
            const response = await getProductsCartApi();
            products.value = response;
        });
        // onMounted (async () => {
        //     const response = await getProductsCartApi();
        //     products.value = response;
        // });
        const getTotal = () => {
            let totalTemp = 0;
            products.value.forEach((product) => {
                totalTemp += product.price * product.quantity;
            });
            return totalTemp.toFixed(2);
        };
        const deleteAllProductsCart = (idProduct) => {
            deleteAllProductsCartApi(idProduct);
            reloadCart.value = !reloadCart.value;
        };
        return {
            products,
            getTotal,
            deleteAllProductsCart,
        };
    },
};
</script>

<style>

</style>
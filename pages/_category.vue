<template>
    <div>
        <CategoryMenu :level="0" :categories="categories"></CategoryMenu>

        <hr class="my-2">


        <div class="d-flex flex-wrap justify-space-between">
            <v-card class="mb-5 mx-2 w-25" v-for="product in products.items" :key="product.uid">
                <v-img height="150" :src="product.image.url" />
                <v-card-title>{{ product.name }}</v-card-title>
            </v-card>
        </div>


        <v-pagination
            @input="onPageChange"
            v-model.number="page"
            :length="products.page_info.total_pages"
        ></v-pagination>

    </div>
</template>


<script lang="ts">

import { Component, Vue, } from 'nuxt-property-decorator'
import CategoryMenu from '~/components/CategoryMenu.vue';

import productByCategory from '~/apollo/queries/products/productByCategory.gql'
import allCategories from '~/apollo/queries/category/allCategories.gql'
import { CategoryResult, Products } from '~/types/types';

@Component({
    apollo: {
        products: {
            prefetch: true,
            query: productByCategory,
            variables() {
                return {
                    categoryUID: this.$route.params.category,
                    currentPage: this.page
                }
            }
        },
        categories: {
            prefetch: true,
            query: allCategories
        }
    },
    components: {
        CategoryMenu
    }
})
export default class Index extends Vue {

    categories: CategoryResult | null = null
    products: Products | null = null

    page: number = parseInt(this.$route.query.page as string) || 1;

    mounted() {
        console.log(this.products)
    }

    onPageChange(page: number) {
        this.$router.push({query: {page: page as unknown as string}})
    }

}
</script>

<style>

.w-25 {
    width: fit-content;
}

</style>

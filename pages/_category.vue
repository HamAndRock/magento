<template>
    <div>

        <v-container>
            <v-row>
                <v-col cols="12" md="3">
                    <CategoryMenu :level="0" :categories="categories"></CategoryMenu>
                </v-col>
                <v-col cols="12" md="9">

                    <v-row>
                        <v-col sm="6" md="4" v-for="product in products.items" :key="product.uid">
                            <v-card class="mb-5 mx-2 w-25"  elevation="5">
                                <v-img :src="product.image.url"/>
                                <v-card-title>{{ product.name }}</v-card-title>
                                <v-card-actions class="mx-2">
                                    <div>
                                        {{product.price_range.minimum_price.final_price.value}}
                                        {{product.price_range.minimum_price.final_price.currency}}
                                    </div>
                                    <v-btn class="ml-auto" color="success">BUY</v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-col>
                    </v-row>


                    <v-pagination
                        @input="onPageChange"
                        v-model.number="page"
                        :length="products.page_info.total_pages"
                    ></v-pagination>

                </v-col>
            </v-row>
        </v-container>


    </div>
</template>


<script lang="ts">

import {Component, Vue,} from 'nuxt-property-decorator'
import CategoryMenu from '~/components/CategoryMenu.vue';

import productByCategory from '~/apollo/queries/products/productByCategory.gql'
import allCategories from '~/apollo/queries/category/allCategories.gql'
import {CategoryResult, Products} from '~/types/types';

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
        window.location.search = `page=${page}`
        // this.$router.push({query: {page: page as unknown as string}}) CORS not working
    }

}
</script>

<style>

.w-25 {
    width: fit-content;
}

</style>

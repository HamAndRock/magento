<template>
    <v-container class="d-flex justify-space-between px-0">
        <a v-for="category in categories.items" :href="category.uid">
            <v-card hover  :class="['mx-2', $route.params.category === category.uid && 'active']">
                <v-card-title>
                    {{ category.name }}
                    <v-icon v-if="category.children">mdi-chevron-down</v-icon>
                </v-card-title>
            </v-card>
        </a>

    </v-container>
</template>


<script lang="ts">

import { Component, Vue, Prop } from 'nuxt-property-decorator';
import { CategoryResult } from '~/types/types';

@Component
export default class CategoryMenu extends Vue {

    @Prop()
    categories!: CategoryResult

    @Prop()
    level!: number;

    mounted() {
        console.log(this.categories)
    }

}

</script>


<style>

a {
    text-decoration: none;
}

.active {
    background-color: white !important;
    color: black !important;
}

</style>

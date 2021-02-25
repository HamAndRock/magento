<template>
    <div>

        <h3>Categories</h3>
        <v-treeview
            rounded
            :items="categories.items"
        >
            <template v-slot:label="{item}">
                <!-- Would use NuxtLink, but CORS not working-->
                <a :class="[$route.params.category === item.id && 'active']" :href="item.id">{{ item.name }}</a>
            </template>
        </v-treeview>

    </div>
</template>


<script lang="ts">

import {Component, Vue, Prop} from 'nuxt-property-decorator';
import {CategoryResult} from '~/types/types';

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
    border-radius: 4px;
    margin-left: 5px;
    padding: 0 15px;
    color: white !important;
}

.active {
    background-color: rgba(255, 255, 255, 0.8) !important;
    color: black !important;
}

.v-treeview-node__label {
    display: flex;
}

h3 {
    padding-left: 15px;
}

</style>

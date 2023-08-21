<template>
    <h1>Rabbits</h1>
    <ul>
        <li v-for="rabbit in rabbits" :key="rabbit.id">
            [{{ rabbit.id }}] {{ rabbit.name }}
        </li>
    </ul>

    <div class="pages row justify-content-center cursor-pointer">
        <div
            v-for="(page, index) in pages"
            :key="index"
            class="page col "
            :class="(page.active ? 'bg-white text-dark' : 'bg-secondary')
                    + ' '
                    + (page.url == null ? 'd-none' : '')"
            v-html="page.label"

            role="button"

            @click="loadPage(page.url)"
        />

    </div>
</template>

<script>

import axios from 'axios';

export default {
    name: 'HelloWorld',
    data: function() {
        return {
            rabbits: [],
            pages: []
        }
    },
    methods: {

        loadPage(target) {

            if (target == null) return;

            this.loadRabbits(target);
        },
        loadRabbits(target) {
            axios.get(target)
                 .then(response => {

                    const data = response.data;
                    console.log(data);

                    this.rabbits = data.rabbits.data;
                    this.pages = data.rabbits.links;
                 })
                 .catch(error => {
                    console.log(error);
                 });
        }
    },
    mounted() {

        this.loadRabbits('http://localhost:8000/api/v1/rabbit-index');
    }
}

</script>

<style>

.page {

    width: 50px;
}
</style>

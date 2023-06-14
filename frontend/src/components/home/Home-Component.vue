<template>
    <div class="home">
        <PageTitleTemplateVue icon="fa fa-home" main="Dashboard"
            sub="Base de Conhecimento" />
        <div class="stats">
            <StatComponentVue title="Categorias" :value="stat.categories"
                icon="fa fa-folder" color="#d54d50" />
            <StatComponentVue title="Artigos" :value="stat.articles"
                icon="fa fa-file" color="#3bc480" />
            <StatComponentVue title="Usuários" :value="stat.users"
                icon="fa fa-user" color="#3282cd" />
        </div>
    </div>
</template>

<script>
import PageTitleTemplateVue from '../template/PageTitle-Template.vue'
import StatComponentVue from './Stat-Component.vue'
import axios from 'axios'
import { baseApiUrl } from '../../global'

export default {
    name: 'Home',
    components: { PageTitleTemplateVue, StatComponentVue },
    data: function() {
        return {
            stat: {}
        }
    },
    methods: {
        getStats() {
            axios.get(`${baseApiUrl}/stats`)
                .then(res => this.stat = res.data)
        }
    }, 
    mounted() {
        this.getStats()
    }


}
</script>

<style>
    .stats {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }

</style>
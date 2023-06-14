<template>
    <div class="admin-pages">
        <PageTitleTemplateVue icon="fa fa-cogs" main="Administração do Sistema"
            sub="Cadastro & Cia"/>
        <div class="admin-pages-tabs">
            <b-card no-body>
                <b-tabs card>
                    <b-tab title="Artigos">
                        <ArticleAdminVue />
                    </b-tab>
                    <b-tab title="Categorias">
                        <CategoryAdminVue />
                    </b-tab>
                    <b-tab title="Usuáriios">
                        <UserAdminVue />
                    </b-tab>
                </b-tabs>
            </b-card>
        </div>
    </div>
</template>

<script>
import PageTitleTemplateVue from '../template/PageTitle-Template.vue'
import ArticleAdminVue from './Article-Admin.vue'
import CategoryAdminVue from './Category-Admin.vue'
import UserAdminVue from './User-Admin.vue'

export default {
    name: 'AdminPages',
    components: { PageTitleTemplateVue, ArticleAdminVue, CategoryAdminVue, UserAdminVue }
}
</script>

<style>

</style>
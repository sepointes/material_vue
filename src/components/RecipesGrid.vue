<template slot="tab-pane-1">
    <div class="md-layout">
        <div v-if="isLoading" class="article-preview">Loading recipes...</div>
        <div v-else>
            <div v-if="articles.length === 0" class="article-preview">
                No recipes are here... yet.
            </div>
            <RwvRecipePreview
                    v-for="(recipe, index) in articles"
                    :recipe="recipe"
                    :key="recipe.title + index"
            />
            <VPagination :pages="pages" :currentPage.sync="currentPage" />
        </div>
    </div>
</template>

<script>
    import { mapGetters } from 'vuex';
    import RwvRecipePreview from "@/components/VRecipePreview";
    import VPagination from "@/components/VPagination";
    import { FETCH_ARTICLES } from "@/store/actions.type";
    export default {
        name: 'RecipesGrid',
        components: {
            RwvRecipePreview,
            VPagination
        },
        props: {
            type: {
                type: String,
                required: false,
                default: "all"
            },
            author: {
                type: String,
                required: false
            },
            tag: {
                type: String,
                required: false
            },
            favorited: {
                type: String,
                required: false
            },
            itemsPerPage: {
                type: Number,
                required: false,
                default: 10
            }
        },
        data() {
            return {
                currentPage: 1
            };
        },
        mounted(){
                this.$store.dispatch('getRecipes')

        },
        computed: {
            listConfig() {
                const { type } = this;
                const filters = {
                    offset: (this.currentPage - 1) * this.itemsPerPage,
                    limit: this.itemsPerPage
                };
                if (this.author) {
                    filters.author = this.author;
                }
                if (this.tag) {
                    filters.tag = this.tag;
                }
                if (this.favorited) {
                    filters.favorited = this.favorited;
                }
                return {
                    type,
                    filters
                };
            },
            pages() {
                if (this.isLoading || this.articlesCount <= this.itemsPerPage) {
                    return [];
                }
                return [
                    ...Array(Math.ceil(this.articlesCount / this.itemsPerPage)).keys()
                ].map(e => e + 1);
            },
            ...mapGetters(["articlesCount", "isLoading", "articles"])
        },
        watch: {
            currentPage(newValue) {
                this.listConfig.filters.offset = (newValue - 1) * this.itemsPerPage;
                this.fetchArticles();
            },
            type() {
                this.resetPagination();
                this.fetchArticles();
            },
            author() {
                this.resetPagination();
                this.fetchArticles();
            },
            tag() {
                this.resetPagination();
                this.fetchArticles();
            },
            favorited() {
                this.resetPagination();
                this.fetchArticles();
            }
        },
        mounted() {
            this.fetchArticles();
        },
        methods: {
            fetchArticles() {
                this.$store.dispatch(FETCH_ARTICLES, this.listConfig);
            },
            resetPagination() {
                this.listConfig.offset = 0;
                this.currentPage = 1;
            }
        }
    };
</script>

<style scoped>
</style>
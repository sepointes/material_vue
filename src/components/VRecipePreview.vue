<template>
    <md-card class="md-card-article" :class="{ 'md-card-hidden': cardHidden }">
        <md-card-header :class="getClass(headerColor)">
            <slot name="title"></slot>
            <div class="social-line">
                <slot name="buttons"></slot>
            </div>
        </md-card-header>

        <md-card-content>
            <slot name="description"></slot>
            <slot name="inputs"></slot>
        </md-card-content>

        <md-card-actions>
            <slot name="footer"></slot>
        </md-card-actions>
    </md-card>
    <!--<div class="article-preview">-->
        <!--<RwvArticleMeta isPreview :recipe="recipe" />-->
        <!--<router-link :to="articleLink" class="preview-link">-->
            <!--<h1 v-text="recipe.title" />-->
            <!--<p v-text="recipe.description" />-->
            <!--<span>Read more...</span>-->
            <!--<TagList :tags="recipe.tagList" />-->
        <!--</router-link>-->
    <!--</div>-->
    <!--<div class="md-layout-item md-size-25 ml-auto">-->
        <!--<img :src="item.image" class="rounded" />-->
        <!--<img :src="item.image" class="rounded" />-->
    <!--</div>-->
</template>

<script>
   // import RwvArticleMeta from "./ArticleMeta";
//    import TagList from "./TagList";

    export default {
        name: "RwvRecipePreview",
        components: {
           // RwvArticleMeta,
            //TagList
        },
        props: {
            recipe: { type: Object, required: true }
        },
        computed: {
            articleLink() {
                return {
                    name: "article",
                    params: {
                        slug: this.recipe.slug
                    }
                };
            }
        }
    };
</script>

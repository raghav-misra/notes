<template>
    <section>
        <nuxt-link to="/" class="back-button">⟵ Back Home</nuxt-link>

        <h1 class="title">{{ title }}</h1>

        <details v-if="document && document.toc" class="toc">
            <summary><h2>Contents:</h2></summary>

            <div class="headings">
                <a
                    v-for="(section, i) in document.toc"
                    :key="i"
                    :href="`#${section.id}`"
                >
                    {{ section.text }}
                </a>
            </div>
        </details>

        <article ref="document" class="page">
            <small>
                <i>YOU GUYS BETTER GIVE ME CREDIT ON THE TEST</i> ❤️
                <br />
                <b>GNU LGPL v3</b>
            </small>

            <nuxt-content :document="document" />
        </article>
    </section>
</template>

<script lang="ts">
import { IContentDocument } from "@nuxt/content/types/content";
import Vue from "vue";

export default Vue.extend({
    data() {
        return {
            document: null as IContentDocument | null,
        };
    },

    async fetch() {
        const content = await this.$content(
            `notes/${this.$route.params.slug}`
        ).fetch();

        this.document = Array.isArray(content) ? content[0] : content;
    },

    head() {
        return {
            title: `notes/${this.$route.params.slug}`,
        };
    },

    computed: {
        title(): string {
            return this.document ? this.document.title : "";
        },
    },

    watch: {
        title() {
            document.title = this.title === "" ? "Notes" : this.title;
        },
    },
});
</script>

<style scoped>
/* table of contents */
.toc {
    background: white;
    padding: 1rem 2rem;
}

.toc * {
    color: var(--back) !important;
}

.toc summary {
    display: flex;
    cursor: pointer;
}

.toc summary h2::before {
    content: "+";
    margin-right: 0.5rem;
}

.toc[open] summary h2::before {
    content: "-";
}

.toc .headings {
    flex-wrap: wrap;
    display: flex;
    margin: 0.5rem;
}

.toc a {
    width: 32%;
    text-decoration: none;
    color: cornflowerblue !important;
}

/* go back -button */
.back-button {
    margin-bottom: 1rem;
    display: inline-block;
    font-size: 1.5rem;
}
</style>

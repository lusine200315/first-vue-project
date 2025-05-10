<template>
    <nav
        class="navbar navbar-expand-lg"
        :class="[`navbar-${theme}`, `bg-${theme}`, 'navbar', 'navbar-expand-lg']"
    >
        <div class="container-fluid">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <a class="navbar-brand" href="#">My Vue</a>
                <navbar-link
                    v-for="(page, index) in publishedPages" class="nav-item" :key="index"
                    :page="page"
                    :index="index"
                ></navbar-link>

                <li>
                    <router-link
                    :to="'/pages'"
                    class="nav-link"
                    active-class="active"
                    aria-current="page"
                    >Pages</router-link>
                </li>
            </ul>
            <form class="d-flex">
                <button
                    class="btn btn-primary"
                    @click.prevent="changeTheme()"
                >Toggle Navbar</button>
            </form>
        </div>
    </nav>
</template>

<script>
import NavbarLink from './NavbarLink.vue';
    export default {
        components: {
            NavbarLink
        },
        inject: ['$pages', '$bus'],
        created() {
            this.getThemeSettings();

            this.pages = this.$pages.getAllPages();

            this.$bus.$on('page-updated', () => {
                this.pages = [...this.$pages.getAllPages()];
            });

            this.$bus.$on('page-created', () => {
                this.pages = [...this.$pages.getAllPages()];
            });

            this.$bus.$on('page-deleted', () => {
                this.pages = [...this.$pages.getAllPages()];
            });
        },
        computed: {
            publishedPages() {
                return this.pages.filter(page => page.published);
            }
        },
        data() {
            return {
                theme: 'dark',
                data: []
            };
        },
        methods: {
            changeTheme() {
                this.theme = this.theme === 'light' ? 'dark' : 'light';
                this.storeThemeSettings();
            },
            storeThemeSettings() {
                localStorage.setItem('theme', this.theme);
            },
            getThemeSettings() {
                let theme = localStorage.getItem('theme');
                if (theme) {
                    this.theme = theme;
                }

            },
        }
    }
</script>
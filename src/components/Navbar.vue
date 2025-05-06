<template>
    <nav
        class="navbar navbar-expand-lg"
        :class="[`navbar-${theme}`, `bg-${theme}`, 'navbar', 'navbar-expand-lg']"
    >
        <div class="container-fluid">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <a class="navbar-brand" href="#">My Vue</a>
                <li v-for="(page, index) in pages" class="nav-item" :key="index">
                    <navbar-link
                        :page="page"
                        :isActive="activePage == index"
                        @click.prevent="navLinkClick(index)"
                    ></navbar-link>
                </li>
            </ul>
            <form class="d-flex" action="">
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
        created() {
            this.getThemeSettings()
        },
        props: ['pages', 'activePage', 'navLinkClick'],
        data() {
            return {
                theme: 'dark',
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
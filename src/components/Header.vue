<template lang="pug">
    .header-wrapper
        .header
            .row
                .col-md-8
                    app-search
                .col-md-1.d-flex.justify-content-center.single-icon-block
                    i.fa.fa-2x.fa-ravelry.icon-color.align-middle
                .col-md-3.offset-md-0.right-side.d-flex.justify-content-center
                    .profile-bage(@click="goToProfile(profileLink)")
                        md-avatar.md-avatar-icon
                            img(:src="profile.avatar")
                        span.personal-name {{ profile ? `${profile.name} ${profile.surname}` : '' }}
                    .personal-menu
                        md-menu(md-direction="bottom left")
                            md-icon(md-menu-trigger) expand_more
                            md-menu-content
                                md-menu-item(v-for="item in menuItems", :key="item", @click.native="item.action")
                                    md-icon(v-if="item.icon") {{ item.icon }}
                                    span {{ item.label }}
</template>

<script>
    import AppSearch from './search-input/SearchInput.vue';

    export default {
        name: 'kmc-header',
        props: [
            'menuItems',
            'profile',
            'profileLink'
        ],
        components: {
            'app-search': AppSearch
        },
        methods: {
            goToProfile(link) {
                this.$router.push(link);
            }
        }
    }
</script>

<style lang="stylus" scoped>
@import '../styles/constants.styl'

.right-side
    border-left: solid 1px $color2
    height: $header_heigth

.header-wrapper
    position fixed
    width 100%
    z-index 2

.header
    box-shadow 0 1px 1px 0 rgba(0,0,0,0.2)
    transition 0.3s
    width: calc(100% - $sidebar__width)
    height: $header_heigth
    padding-left 60px//$sidebar__width
    overflow hidden
    background-color white

ul
    list-style-type none
    margin 0
    padding 0
    overflow hidden

li
    float left
    > a
        display block
        text-aligin center
        padding 14px 16px
        text-decoration none  
.profile-bage
    display block
    margin-right 20px
    margin-top 5px
    float left
    cursor pointer
    .personal-name
        margin-left 10px

.personal-menu
    margin-top 12px
    float right
    cursor pointer

.icon-color
    color #5c6971
    cursor pointer

.single-icon-block
    top 10px
    height 54px
</style>
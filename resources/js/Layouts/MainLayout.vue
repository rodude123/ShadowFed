<script setup>
import AddPost from '@/Components/AddPost.vue';
import SideNav from '@/Components/SideNav.vue';
import ProfileNav from '@/Layouts/ProfileNav.vue';
import { Link } from '@inertiajs/vue3';
import { provide, ref } from 'vue';
import CloseIcon from 'vue-material-design-icons/Close.vue';
import Magnify from 'vue-material-design-icons/Magnify.vue';
import MenuIcon from 'vue-material-design-icons/Menu.vue';
import PlusThick from 'vue-material-design-icons/PlusThick.vue';

let showCreatePost = ref(false);
const showSideNav = ref(false);
provide('showSideNav', showSideNav);

function toggleSideNav()
{
    showSideNav.value = !showSideNav.value;
}

</script>

<template>
    <div id="MainLayout" :class="['mainLayout', {shifted: showSideNav}]">
        <nav id="TopNavHome" v-if="$page.url === '/'">
            <div>
                <div class="logo">
                    <Link href="/">
                        <img src="/shadowFedIcon.svg" alt="">
                        ShadowFed
                    </Link>
                </div>
                <div class="search">
                    <div>
                        <Magnify class="icon" :size="25" fill-color="#FFFFFF"/>
                        <input type="text" name="search" id="searchBox" placeholder="Search" >
                    </div>
                </div>
                <div class="addPost altBtnHover">
                    <PlusThick class="icon" :size="40" fill-color="#737373" @click="$event => showCreatePost = true"/>
                </div>
                <div class="menu">
                    <div class="hamburger altBtnHover">
                        <MenuIcon v-if="!showSideNav" :size="35" fill-color="#000000" @click="toggleSideNav"/>
                        <CloseIcon v-else :size="35" fill-color="#000000" @click="toggleSideNav"/>
                    </div>
                    <SideNav :isShown="showSideNav" @close="toggleSideNav"/>
                    <ProfileNav/>
                </div>
            </div>
        </nav>
    </div>
    <AddPost v-if="showCreatePost" @close="showCreatePost = false"/>
</template>

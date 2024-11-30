<script setup>
import { defineProps } from 'vue';
import { Link, usePage } from '@inertiajs/vue3';
import CompassOutlineIcon from 'vue-material-design-icons/CompassOutline.vue';
import HeartOutline from 'vue-material-design-icons/HeartOutline.vue';
import HomeOutline from 'vue-material-design-icons/HomeOutline.vue';
import LogoutVariantIcon from 'vue-material-design-icons/LogoutVariant.vue';
import MessageTextOutline from 'vue-material-design-icons/MessageTextOutline.vue'

const props = defineProps({
    isShown: {
        type: Boolean,
        default: false,
    },
});
const emit = defineEmits();
// const user = usePage().props.auth.user;

function closeNav()
{
    emit('close');
    setTimeout(_ => {}, 500);

}

</script>

<template>
    <div class="sideNav" :class="{'hide': !isShown}" id="sideNav">
        <ul>
            <li>
                <Link href="#">Home <HomeOutline size="30" class="icon"/></Link>
            </li>
            <li>
                <Link href="#">Discover <CompassOutlineIcon size="30" class="icon"/></Link>
            </li>
            <li>
                <Link href="#">Messages <MessageTextOutline size="30" class="icon"/></Link>
            </li>
            <li>
                <Link href="#">Notifications <HeartOutline size="30" class="icon"/></Link>
            </li>
            <li>
                <Link href="#">Profile <img src="https://picsum.photos/id/64/300/320" alt="Profile Image" class="profile"></Link>
            </li>
            <li class="bottom">
                <Link href="#">Logout <LogoutVariantIcon size="30" class="icon"/></Link>
            </li>
        </ul>
    </div>
</template>

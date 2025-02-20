<template>
    <aside :class="{ 'sidebar__closed': sidebarStore.closed}">
        <template v-if="sidebarStore.closed">
            <IconRightArrow class="sidebar__icon" @click="sidebarStore.toggleSidebar()" />
        </template>
        <template v-else>
            <h2>Sidebar</h2>
            <IconLeftArrow class="sidebar__icon" @click="sidebarStore.toggleSidebar()" />
            <TheButton>Create post</TheButton>
            <div>
                Current time: {{currentTime}}
            </div>
            <TheButton @click="onUpdateTimeClick">Update Time</TheButton>
            <a @click="navigateToPrivacy">Programmatic to privacy</a>
            <router-link to="about">About</router-link>
        </template>
    </aside>
</template>
<script setup>
import { ref, onBeforeMount } from 'vue';
import TheButton  from '../atoms/TheButton.vue'
import IconLeftArrow from '../icons/IconLeftArrow.vue'
import IconRightArrow from '../icons/IconRightArrow.vue'
import { RouterLink, useRouter } from 'vue-router';
import { useSidebarStore } from '../../stores/sidebar';

const currentTime = ref(new Date().toLocaleTimeString());
//const closed = ref(false);
const sidebarStore = useSidebarStore()

// const toggleSidebar = () => {
//     closed.value = !closed.value;
//     window.localStorage.setItem("sidebar", closed.value);
// }
const onUpdateTimeClick = () => {
    currentTime.value = new Date().toLocaleTimeString();
};
const router = useRouter();

const navigateToPrivacy = (event) => {
event.preventDefault();
console.log("Run a side effect");
router.push("privacy");
}

onBeforeMount( async () => {
    // const sidebarState = window.localStorage.getItem("sidebar");
    // closed.value = sidebarState === "true";
    sidebarStore.loadSidebarFromLocalStorage()
});
</script>
<style scoped>
aside {
    display: flex;
    flex-direction: column;
    position: relative;
    &.sidebar__closed{
        width: 40px;
    }
    .sidebar__icon{
        position: absolute;
        right: 12px;
        top: 22px;
        cursor: pointer;
    }
}
</style>
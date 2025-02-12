<template>
    <aside :class="{ 'sidebar__closed': closed}">
        <template v-if="closed">
            <IconRightIcon @click="toggleSidebar"/>
        </template><template v-else>
            <h2>Sidebar</h2>
            <IconLeftIcon class="sidebar__icon" @click="toggleSidebar"/>
            <TheButton>Create post</TheButton>
            <div>
                Current time: {{ currentTime }}
            </div>
            <TheButton @click.once="onUpdateTimeClick" value="Update time" />
        </template>

    </aside>
</template>
<script setup>
import { onBeforeMount, ref } from 'vue';
import TheButton from '../atoms/TheButton.vue';
import IconLeftIcon from '../icons/IconLeftArrow.vue'
import IconRightIcon from '../icons/IconRightArrow.vue'

const currentTime = ref(new Date().toLocaleTimeString());
const closed= ref(false);

const onUpdateTimeClick = () => {
    currentTime.value = new Date().toLocaleTimeString();
};

const toggleSidebar = ()=>{
    closed.value = !closed.value;
    window.localStorage.setItem("sidebar", closed.value);
}

onBeforeMount(()=>{
    const sidebarState = window.localStorage.getItem("sidebar");
    closed.value = sidebarState == "true";
})

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
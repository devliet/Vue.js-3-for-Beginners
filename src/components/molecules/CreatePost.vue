<template>
    <form ref="createPostForm" @submit="createPost">
        <h2>Create a Post</h2>
        <textarea rows="4" cols="20" ref="textareaRef" required="true" minlength="10">Value to add</textarea>
        <TheButton @click="createPostHandler">Create a post</TheButton>
    </form>
</template>
<script setup>
import { onMounted, ref} from 'vue';
import TheButton from '../atoms/TheButton.vue';
import { usePostsStore} from '../../stores/posts';
import { useVisibilityStore } from '../../stores/visibility';

const textareaRef = ref(null);
const createPostForm = ref(null);

const postsStore = usePostsStore()
const visibilityStore = useVisibilityStore()

const { addPost} =  postsStore

const createPost = (event) =>{
    event.preventDefault();
    if(createPostForm.value.reportValidity()){

    }
}

const createPostHandler = (event) => {
    event.preventDefault();
    if(createPostForm.value.reportValidity()){
        addPost(textareaRef.value.value)
        visibilityStore.toggleForm();

    }
}

onMounted(()=> {
    textareaRef.value.focus();
})
</script>
<style scoped>
form {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 16px;
    padding-bottom: 16px;
    border-bottom: 1px solid var(--color-border);
}
</style>
<template>
    <section class="userView">
        <h2>User information</h2>
        <template v-for="key in valuesToDisplay">
            <label v-if="user[key]">
                {{ key }}
                <input type="text" disabled :value="user[key]" /><br/>
            </label>
        </template>
    </section>
</template>
<script setup>
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
const user = reactive({});
const valuesToDisplay = [
    "title",
    "first",
    "last",
    "email",
    "picture",
    "gender"

];
const fetchUser = (userId) => {
    const url = `https://randomuser.me/api/?seed=${userId}`;
    fetch(url, {
        // "headers": {
        //     "app-id": "657a3106698992f50c0a5885"
        // }
    })
        .then(response => response.json())
        .then(result => {
         //   Object.assign(user, result);
         
            user.title= result.results[0].name.title,
            user.first=result.results[0].name.first,
            user.last= result.results[0].name.last,
            user.email= result.results[0].email,
            user.picture= result.results[0].picture.thumbnail,
      //   comments="fields3[index].body,
            user.gender= result.results[0].gender



        });
} 
const route = useRoute();
fetchUser(route.params.userId);
</script>
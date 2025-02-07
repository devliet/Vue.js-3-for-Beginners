<template>
  <!-- substituting retweets -->
  <!-- vue 3 v-for across async fetch data -->
  <p>
    Social posts below
  </p>
  <div v-if="fields1.posts && fields2">
    <p>
      let's go
    </p>
    <!-- <SocialPost v-for="(post) in fields1.posts" :username="p" :userId="post.userId" :avatarSrc="p" :post="post.body"
      :comments="p" :likes="post.reactions.likes" :retweets="post.reactions.dislikes" :key="post.userId"
      @delete="onDelete(index)"></SocialPost> -->
    <SocialPost
    v-for="(post, index) in fields1.posts"
    :username="fields2[index].login.uuid"
    :userId="post.userId"
    :avatarSrc="fields2[index].picture.medium"
    :post="post.body"
    :comments="fields3[index].body"
    :likes="post.reactions.likes"
    :retweets="post.reactions.dislikes"  
    :key="post.id"
    @delete="onDelete(index)"
  ></SocialPost>

  </div>
</template>




<script setup>
import { ref, onMounted } from 'vue';
import SocialPost from '../molecules/SocialPost.vue'


const posts = ref([]);

const fields1 = ref([]);
const fields2 = ref([]);
const fields3 = ref([]);


async function fetchFields() {
  const [fields1Response, fields2Response, fields3Response] = await Promise.all([
    fetch('https://dummyjson.com/posts?limit=5&select=userId,body,reactions,views'),
    fetch('https://randomuser.me/api/?inc=login,picture&results=5'),
    fetch('https://dummyjson.com/comments?limit=5&select=body'),
  ])


  const fields11 = await fields1Response.json();
  //  fields1.value = fields11
  const fields22 = await fields2Response.json();
  //  fields2.value = fields22

  const fields33 = await fields3Response.json();
  //  fields3.value = fields33

  return [fields11, fields22, fields33];
}

onMounted(
  async () => {
  await fetchFields().then(([fields11, fields22, fields33]) => {
    fields1.value = fields11;
    fields2.value = fields22.results;
    fields3.value = fields33.comments;

  //  fields11, fields22, fields33
  }
  ).catch(error => {
  console.log("error promises")
})
  }
)
</script>
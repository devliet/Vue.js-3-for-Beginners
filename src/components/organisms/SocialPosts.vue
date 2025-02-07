<template>
  <!-- substituting retweets -->
  <!-- vue 3 v-for across async fetch data -->
  <p>
    Social posts below
  </p>
  <div v-if="posts">
    <p>
      let's go
    </p>
    <!-- <SocialPost v-for="(post) in fields1.posts" :username="p" :userId="post.userId" :avatarSrc="p" :post="post.body"
      :comments="p" :likes="post.reactions.likes" :retweets="post.reactions.dislikes" :key="post.userId"
      @delete="onDelete(index)"></SocialPost> -->

        <!-- :comments="fields3[index].body" -->

    <SocialPost
    v-for="(post, index) in posts"
    :post-id="post.id"
    :username="post.username"
    :userId="post.userId"
    :avatarSrc="post.avatarSrc"
    :post="post.post"
    :likes="post.likes"
    :retweets="post.retweets"  
    :key="post.id"
    @delete="onDelete(index)"
  ></SocialPost>

  </div>
</template>




<script setup>
import { ref, onMounted, watch } from 'vue';
import SocialPost from '../molecules/SocialPost.vue'

const onDelete = ( postIndex ) => {
    posts.value.splice(postIndex, 1);
  }

const posts = ref([]);

const fields1 = ref([]);
const fields2 = ref([]);
const fields3 = ref([]);

const skip = ref(0);
let limit = ref(5);
watch(
  posts.value,
  (newValue, old)=>{
    console.log(newValue.length + "xxxx")
    if(newValue.length <4){
      skip.value++;
      fetchFields(skip, limit);
    }
  }
)

async function fetchFields(skip, limit) {
  const [fields1Response, fields2Response, fields3Response] = await Promise.all([
    fetch(`https://dummyjson.com/posts?limit=5&select=userId,body,reactions,views&skip=${skip.value}&limit=${limit.value}`),
    fetch(`https://dummyjson.com/users?limit=5&select=username,image&skip=${skip.value}&limit=${limit.value}`),
    fetch(`https://dummyjson.com/comments?limit=5&select=body&skip=${skip.value}&limit=${limit.value}`),
  ])


  const fields11 = await fields1Response.json();
  //  fields1.value = fields11
  const fields22 = await fields2Response.json();
  //  fields2.value = fields22

  const fields33 = await fields3Response.json();
  //  fields3.value = fields33

  fields1.value = fields11.posts;
    fields2.value = fields22.results;
    fields3.value = fields33.comments;

   
   for(let index=0;index<5;index++){
     posts.value.push({
      id: fields3.value[index].id,
      username: fields2.value[index].username,
      userId: fields1.value[index].userId,
      avatarSrc: fields2.value[index].image,
      post: fields1.value[index].body,
   //   comments="fields3[index].body,
      likes: fields1.value.likes,
      retweets: fields1.value[index].reactions.dislikes,
      tags: fields1.value[index].tags

     })
     
    }

 // return [fields11, fields22, fields33];
}

onMounted(
  async () => {
  await fetchFields(skip, limit)
  .catch(error => {
  console.log("error promises")
  console.log(error)
})
  }
)
</script>
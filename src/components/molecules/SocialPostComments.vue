
  <template>
    <p>
     --- {{ comments.length }}***
    </p>
    <div class="SocialPostComments"  v-if="comments.length!=0">
        <p>Comments:</p>
        <div v-for="{user, body} in comments" class="comment">
          <p>{{user.username }}: <strong> {{ body }}</strong></p>
        </div>
    </div>
    <div v-else>
      No comments for this post
    </div>
  </template>
  
<script setup >
import { reactive } from 'vue';
const props = defineProps({
  comments: Array, postId: Number
})

const comments = reactive([]);

const fetchComments = (postId) =>{
  const baseUrl = 'https://dummyjson.com';

  fetch(`${baseUrl}/comments/post/${postId}`)
  .then(response => response.json())
  .then( result => {
    Object.assign(comments, result.comments)
  })
}

fetchComments(props.postId);
</script>
  
<style lang="scss">
.SocialPostComments{
  padding-left:24px;
  p {
    font-weight: bold;
  }
  .comment {
    display:flex;
    justify-content: space-between;
    background-color: var(--color-input-mute);
    border-radius: 10px;
    margin-bottom: 8px;
    padding:8px 16px;
    color:var(--color-background-soft);
    width: 75%;
  }
  svg {
    fill:var(--color-background-mute);
  }
}
</style>
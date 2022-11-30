<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo @follow='follow' @unfollow='unfollow' :user="user" />
                <UserProfileWrite v-if='is_me' @post_a_post="post_a_post" />
            </div>
            <div class="col-9">
                <UserProfilePosts :user="user" :posts="posts" @delete_a_post="delete_a_post" />
            </div>
        </div>
    </ContentBase>
</template>
  
<script>

import ContentBase from '@/components/ContentBase';
import UserProfileInfo from '@/components/UserProfileInfo.vue';
import UserProfilePosts from '@/components/UserProfilePosts.vue';
import UserProfileWrite from '@/components/UserProfileWrite.vue';
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
import $ from 'jquery'
import { useStore } from 'vuex'
import { computed } from 'vue'


export default {
    name: 'UserProfileView',
    components: {
        ContentBase,
        UserProfileInfo,
        UserProfilePosts,
        UserProfileWrite,
    },

    setup() {
        const store = useStore();
        const route = useRoute();
        const userId = parseInt(route.params.userId);
        // console.log(route.params.userId);

        const user = reactive({});

        const posts = reactive({})
        // console.log(userId)

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/getinfo/',
            type: 'get',
            data: {
                user_id: userId,
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(resp) {
                console.log(resp);
                user.id = resp.id;
                user.username = resp.username;
                user.photo = resp.photo;
                user.is_followed = resp.is_followed;
                user.followerCount = resp.followerCount;

            },
        });

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
            type: 'get',
            data: {
                user_id: userId,
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(resp) {
                posts.count = resp.length;
                posts.posts = resp;
            }
        })

        const follow = () => {
            if (user.is_followed) return;
            user.is_followed = true;
            user.followersCount++;

        };

        const unfollow = () => {
            if (!user.is_followed) return;
            user.is_followed = false;
            user.followersCount--;

        };

        const post_a_post = (content) => {
            posts.count++;
            posts.posts.unshift({
                id: posts.count,
                userId: 1,
                content: content,
            })

        };
        const delete_a_post = post_id => {
            posts.posts = posts.posts.filter(post => post.id !== post_id);
            posts.count = posts.posts.length;
        }

        const is_me = computed(() => userId === store.state.user.id);

        return {
            user,
            follow,
            unfollow,
            posts,
            post_a_post,
            is_me,
            delete_a_post,
        }
    }
}
</script>
  
<style scoped>

</style>
  
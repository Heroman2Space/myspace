<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo @follow='follow' @unfollow='unfollow' :user="user" />
                <UserProfileWrite @post_a_post="post_a_post" />
            </div>
            <div class="col-9">
                <UserProfilePosts :posts="posts" />
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

export default {
    name: 'UserProfileView',
    components: {
        ContentBase,
        UserProfileInfo,
        UserProfilePosts,
        UserProfileWrite,
    },

    setup() {
        const user = reactive({
            id: 1,
            username: 'chrischen',
            firstname: 'chris',
            lastname: 'chen',
            followersCount: 0,
            is_followed: false,

        });

        const posts = reactive({
            count: 3,
            posts: [
                {
                    id: 1,
                    userId: 1,
                    content: "算法开心",
                },
                {
                    id: 2,
                    userId: 1,
                    content: "web开心",
                },
                {
                    id: 3,
                    userId: 1,
                    content: "Acwing开心",
                }

            ]
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

        }

        return {
            user,
            follow,
            unfollow,
            posts,
            post_a_post,
        }
    }
}
</script>
  
<style scoped>

</style>
  
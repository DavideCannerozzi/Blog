<template>
  <div>
    <h1>Contentful</h1>
    <p v-for="post in posts" :key="post.title">
      {{ post.fields.title }}
      {{ post.fields.slug }}
    </p>
  </div>
</template>

<script>
import {createClient} from '~/plugins/contentful.js'

const client = createClient()

export default {
 asyncData ({env}) {
      return Promise.all([
        // fetch the owner of the blog
        client.getEntries({
          'sys.id': env.CTF_PERSON_ID
        }),
        // fetch all blog posts sorted by creation date
        client.getEntries({
          'content_type': env.CTF_BLOG_POST_TYPE_ID,
          order: '-sys.createdAt'
        })
      ]).then(([entries, posts]) => {
        // return data that should be available
        // in the template
        return {
          person: entries.items[0],
          posts: posts.items
        }
      }).catch(console.error)
    }
}
</script>

<style>

</style>


<template>
    <div v-if="content">
      <render-content :content="content" />
    </div>
  </template>
  
  <script>
  import Vue from 'vue'
  import { onSSR } from '@vue-storefront/core'
  import { useContent, storyblokBridge } from '@vue-storefront/storyblok'
  import RenderContent from '~/components/cms/RenderContent.vue'
  import { computed, onMounted } from '@vue/composition-api';
  
  export default Vue.extend({
    name: 'DynamicPage',
    components: {
      RenderContent
    },
    setup() {
      const { search, content } = useContent('unique-id')
      const story = computed(()=> content.value);
      // get data
      onSSR(async () => {
        await search({ url: 'home-page' })
      })
      onMounted(()=>{
        storyblokBridge(story.value)
      })
      // return data
      return {
        content: story, 
      }
    }
  })
  </script>
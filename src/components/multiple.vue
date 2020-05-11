<template>
<div class="full">
    <div v-for="i of l.length" :key="i" >
    <vueViewer :address="l[i-1]" />
    </div>
</div>

</template>

<script>
import vueViewer from '@/components/vueViewer'
export default {
    name: 'multiple',
    data(){
        return {
            l: ['http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf','https://cdn.rawgit.com/mozilla/pdf.js/c6e8ca86/test/pdfs/calrgb.pdf'],
            index: 0
        }

    },
    components:{
        vueViewer
    }
}
</script>

<style scopped>
.full{
  
    overflow-y: scroll;
    height: 100%;

}
</style>>
<template>
  <Header :parent_id="parent_id"/>
  <Memberlist />
  <Paging />
</template>

<script>
import Header from '@/components/main/header_nav.vue';
import Memberlist from '@/components/administer/member_listupdate.vue';
import Paging from '@/components/mypage/pageDo.vue';

export default {
components: {
    Header , Memberlist,Paging
  },
    props:{
        parent_id:{
            type:String,
            required:false
        }
    },
    emits:['parent_getSession'],
    setup(props,context){
        context.emit("parent_getSession");
    }
}
</script>

<style>

</style>
<template>
<div>
  <Header :parent_id="parent_id"/>
  <Myinform  :parent_id="parent_id"/>
</div>
</template>

<script>
import Header from '@/components/main/header_nav.vue';
import Myinform from '@/components/mypage/mypage_inform.vue';
export default {
  components: {
    Header , Myinform 
  },
  props:{
        parent_id:{
            type:String,
            required:true
        }
    },
    emits: ['parent_getSession'],
    setup(props,context){
        context.emit("parent_getSession");
    }
}
</script>

<style scoped>
.item{  display: flex;
  margin-top: 150px;
  text-align:left;
}
.btn_group{
    justify-content: left;
}
.col{text-align: center;}

footer{
  margin-top :600px;
}
</style>
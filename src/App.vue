<template>


    <navbar
 :pages="pages"
 :active-page="activePage"

 >
 </navbar>


 
 <router-view>
   
</router-view>

</template>

<script>
import Navbar from './components/Navbar.vue';


export default{
   components:{
       Navbar,
     
   },
   inject:['$bus'],
   created(){
       this.getPages();

       this.$bus.$on('navbarLinkActived',(index)=>{
           this.activePage = index;
       })
   }, 
   data(){
           return{
               activePage:0,
             
             pages:[]
           

             
       }},
       methods:{
           async getPages(){
               let res = await fetch('pages.json');
               let data = await res.json();

               this.pages= data;
           },
           pageCreated(pageObj){
              this.pages.push(pageObj);
           }
       }
}
</script>
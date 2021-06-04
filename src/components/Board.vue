<template>
  <div class="container">
    <!-- rowclicked 함수는 item(클릭한대상),index(클릭댄대상의 인덱스),event(클릭대상에 이벤트정보를 볼수있음)  -->
    <b-table striped hover 
      :items="items" 
      :per-page="perPage" 
      :current-page="currentPage" 
      :fields="fields" 
      @row-clicked="rowClick"
      >
    </b-table>
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      align="center"
    >
    </b-pagination>
    <b-button @click="writeContent" >글쓰기</b-button>
  </div>
</template>

<script>
import data from '@/data'

  export default {
    data() {
      let items = data.Content.sort((a,b)=>{return b.content_id - a.content_id}) //역순으로 정렬할때
//User name속성을 추가 
//console.log(items)
      items = items.map(contentItem=>{return{...contentItem, user_name: data.User.filter(userItem=>userItem.user_id===contentItem.user_id)[0].name}})
//console.log(data.User)
      return {
        fields:[
            {
                key:'content_id',
                label:'글번호',
            },
            {
                key:'title',
                label:'제목',
            },
            {
                key:'created_at',
                label:'작성일',
            },
            {
                key:'user_name',
                label:'글쓴이 ',
            },
        ],
        items: items,
        currentPage:1, //시작페이지
        perPage:10, //한페이지당 몇개 보여줄지 
      }
    },
    methods:{
      rowClick(item, index, e){
        //console.log(1)
       // console.log(item.content_id) => 글순서
        this.$router.push({
          path:`/board/free/detail/${item.content_id}`
        })
      },
      writeContent(){
        this.$router.push({
          path:'/board/free/create'
        })
      }
    },
    computed:{
      rows(){
        return this.items.length
      }
    }
  }
</script> 
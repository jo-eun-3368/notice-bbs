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
      


      //forEach = 기본적인 형태의 반복문 return 값이 음슴
      //map = return값을 가지며 새로운 배열을 만듬
      //filter = 원하는 값들로만 생로운 배열로 만듬(조건이 true)
      //find = 단 하나의 조건문에 화합하는 결과값만 출력 / 반복문 종료


      let items = data.Content.sort((a,b)=>{return b.content_id - a.content_id}) //역순으로 정렬 => sort

      console.log(items)

          items = items.map(contentItem => { return{...contentItem, 
                  user_name: data.User.filter(userItem => userItem.user_id === contentItem.user_id)[0].name}})
      
      //User name을 User 데이터와 비교해서 맞는것만 배열로 출력
      //console.log(items)
      console.log(data.User,"data.User")
      console.log(items,"data.User")


      return {
        items: items,
        currentPage:1, //시작페이지
        perPage:10, //한페이지당 몇개 보여줄지 
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
      }
    },


    //메소드 정의
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


    mounted(){
      console.log(this.rows)
    },


    computed:{
      rows(){
        return this.items.length  //글 목록 갯수
      }
    }
  }
</script> 
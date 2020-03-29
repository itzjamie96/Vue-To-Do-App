<template>
<v-app>
    <v-container>
        <v-row>
            <v-col cols="4">
                <h1>To Do List</h1>
            </v-col>
            <v-col cols="8">
                <p class="mt-3">전체 할일: {{todoList.lenght}} / 완료: {{countDone}} / 남은 할일: {{todoList.length - countDone}}</p>
            </v-col>
        </v-row>
        <v-row>
            <v-col pa-2>
                <List
                    :todoList="todoList"
                    @statusControl="statusControl"
                    @listDelete="listDelete"
                ></List>
            </v-col>
            <!--
                listAdd.vue에서 emit된 listEdit을 여기서 듣는 것
            -->
            <v-col pa-2>
                <ListAdd 
                @listAdd="listAdd"
                @listEdit="listEdit"
                ></ListAdd>
            </v-col>
        </v-row>
    </v-container>
</v-app>  
</template>

<script>
//components 내부의 List, ListAdd를 import
import List from './List'
import ListAdd from './ListAdd'

export default {
    components: {
        List,
        ListAdd
    },
    data() {
        return{
            todoList: []
        }
    },
    computed: {
        countDone() {
            //status가 done인 것들 숫자로 세기
            let count = 0
            this.todoList.forEach(list => {
                if(list.status === "done") count++
            })
            return count
        }
    },
    methods : {
        listAdd(memo) {     //memo를 인자로 받아서 위의 todoList[]에 push 할 것
            console.log("받았어")
            this.todoList.push({memo: memo, status:'created'})      //memo:memo = 객체
        },

        statusControl(index, status) {
            this.todoList[index].status = status  //todoList의 인덱스를 받아와서 속성 status를 찾아 인자로 받은 status 입력
        },
        
        listDelete(index) { //todoList에서 n번재 요소를 찾은 후 거기서부터 1개지우기: 1개만 지워야하니까 index에서 한개
            this.todoList.splice(index,1)
        },

        listEdit(memo, index) { //todoList의 특정 인덱스의 메모를 emit에서 받아온 memo로 바꾼다 (대입)
            this.todoList[index].memo = memo
        }
    }
}
</script>
<template>
    <div>
        <v-textarea
          outlined
          v-model="memo"
          label="할일을 입력해주세요"
          value=""
        ></v-textarea>
        <v-btn v-if="mode==='add'" @click="listAdd">리스트 추가</v-btn>
        <v-btn v-else @click="listEdit">리스트 수정</v-btn>

    </div>
</template>

<script>
import {eventBus} from "../main"
export default {
    data() {
        return {
            memo: null,  //memo를 인자로 받아서 Home에서 신호 받을거임
            index: null,
            mode: "add"     //평소 모드 = 추가
        }
    },
    /*
    이 컴포넌트가 생성될 때 이미 eventBus에 내려줄 곳을 알아야함
    -> listEdit을 듣고 memo, index를 알게 된다
    -> callBack함수에서는 =>(arrow function)을 써야함
    
    */
    created() {
        eventBus.$on('listEdit', (memo, index) => {
            this.memo = memo        //버스타고 넘어온 메모를 여깄는 메모에 넣어줌
            this.index = index      //인덱스는 킵해야함
            this.mode = "edit"      //이미 만들어졌을 때 모드 = 수정 가능
            //console.log(memo, index)
        }) 
        
    },
    methods: {
        listAdd() {
            console.log("리스트 추가 완료")
            if(this.memo===null) {
                alert("할 일을 입력해주세요")
            } else {
                this.$emit("listAdd", this.memo)
                this.memo = null        //list추가 후 textArea에 null
            }
            
        },
        listEdit() {            //EventBus와는 상관없는 listEdit
            if(this.memo===null) {
                alert("할 일을 입력해주세요")
            } else {
                this.$emit("listEdit", this.memo, this.index)
                this.memo = null        //list추가 후 textArea에 null
                this.mode = "add"       //수정 후의 모두 = 추가
            }
        }
    }
}
</script>
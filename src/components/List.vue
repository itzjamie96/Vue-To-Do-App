<template>
    <v-app>
    <div>
        <v-card
            class="pa-3 ma-5"
            :class="{'done': list.status==='done'}"
            v-for="(list, index) in todoList"
            :key="index"
            >    
            <p>{{ list.memo }}</p>
            

            <v-btn 
            fab text small color="green"
            v-if="list.status==='created'"
            @click="$emit('statusControl', index, 'done')"
            ><i class="fas fa-check"></i></v-btn><!--완료-->
            
            <v-btn 
            fab text small color="primary"
            v-else
            @click="$emit('statusControl', index, 'created')"
            ><i class="fas fa-redo-alt"></i></v-btn><!--부활-->
            
            <v-btn 
            fab text small color="red"
            @click="$emit('listDelete', index)"
            ><i class="fas fa-trash-alt"></i></v-btn><!--제거-->
            
            <!--
            -->
            <v-btn 
            fab text small color="yellow"
            v-if="list.status==='created'"
            @click="listEdit(list.memo,index)"
            ><i class="far fa-edit"></i></v-btn><!--수정-->

        </v-card>
    </div>
    </v-app>
</template>

<script>
import {eventBus} from "../main"

export default {
    props:["todoList"],
    methods: {
        /*
        버스에 memo, index를 태운 후 listAdd Component에 내려줘야 수정이 되게찌!
        */
        listEdit(memo, index) { //이건 위에있는 listEdit
            eventBus.listEdit(memo, index)  //이건 eventBus에서 받아온 listEdit
        }
    }
}
</script>

<style scoped>
.done p{
    text-decoration: line-through;
    color: green;
}
.done.v-card{
    background-color: gray;
}


</style>
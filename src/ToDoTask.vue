<template>
        <li class="list-group-item list-group-item-success">
            <p :id = "id" :class="{ complete: completed }" v-if="!editNow" @dblclick="edit()">{{id}}. {{ title }}</p>      <!--if task not edited now-->
            <label :id = "id" v-else-if="editNow">      <!--if task edited now-->
                <input class="edited" v-focus type="text" size="40" v-model="inputNewValue" @keypress.13="$emit('save', id, inputNewValue)" @keyup.13="edit">
            </label>
            <button class="btn btn-outline-success" v-if="!editNow" @click="edit()">edit</button>
            <button class="btn btn-outline-warning" v-else-if="editNow" @click="$emit('save', id, inputNewValue)" @mouseup="edit">save</button>
            <button class="btn btn-outline-primary" @click="$emit('completedTask', id)">complete</button>       
            <button class="btn btn-outline-danger" @click="$emit('remove', id)">remove</button> 
        </li>
</template>

<script>
export default ({
    name: "todo",
    props: ['title', 'id', 'completed'],
    data () {
       return {
           editNow: false,
           inputNewValue: this.title
       }
    },
    methods: {
        edit: function () {
            this.editNow = !this.editNow;
        }
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus();
            }
        }
    }
})
</script>

<style scoped>
    .edited {
        font-size: 28px;
        height: 40px;
        border-radius: 5px; 
        outline: none;
        border: none;
        background: none;
        box-shadow: 0 10px 20px rgba(0,0,0,0.5);
    }
    .list-group-item {
        overflow: scroll;
        font-size: 22px;
        color: rgb(89, 95, 104);
    }
    .complete {
        transition: 2s;
        color: #aabeaf;;
        text-decoration: line-through;
    }
</style>
<template>
    <div class="todo-warp">
        <h3>todoList Demo</h3>
        <ul>
            <li v-for="(item, index) in list" :key="index">
                {{ index + 1 }}、{{ item }}
                <!-- 删除选中 tudo 项 -->
                <span @click="list.splice(index, 1)">-</span>
            </li>
        </ul>

        <input type="text" v-model="todoItem" />
        <button @click="addTodo">add todo item</button>
    </div>
</template>

<script>
    export default {
        name: 'todoList',
        data(){
            return{
                list: ['Vue', 'React', 'Angular'],
                todoItem: ''
            }
        },
        methods: {
            addTodo(){
                let item = this.todoItem.trim()
                // todo 输入为空 或者 已存在 则不能添加
                if(item && !this.list.includes(item)){
                    this.list.push(this.todoItem)
                    this.todoItem = ''
                }else{
                    alert('todo项已存在或为空！')
                }
            }
        }
    }
</script>

<style lang="stylus" scoped>
    .todo-warp
        width 400px
        height auto
        margin 50px auto
        color #333
        h3
            text-align center
            font-size 20px
        ul
            margin 20px auto
        li
            height 24px
            line-height 24px
            overflow hidden
            span
                display block
                width 20px
                height 20px
                border-radius 50%
                background-color red
                float right
                margin-top 2px
                cursor pointer
                text-align center
                line-height 20px
                color #fff
</style>
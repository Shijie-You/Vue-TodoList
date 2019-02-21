<template>
    <div id="contain-all">
        <header id="header-TodoList" >
            <div id="toDoList-content">Todo List</div>
        </header>
        <section id="section-input">
            <section id="wrap">
                <header id="input-header">
                    <input id="input-write-content"
                           type="text"
                           title="ToDoList-Write"
                           placeholder="What needs to be done?"
                           @keyup="inputKeyup"
                           @change="inputChange"
                           :value="changevalue"

                    />
                </header>
                <section id="li-section">
                    <label :class="{ 'opactity-0': tabDisplay, 'check-all-button': true }"
                           for="check-all-input"
                           @click="clickAll">
                    </label>
                    <input id="check-all-input"
                           type="checkbox">
                    <Create-li :list="fliterData"
                               @removeLi="removeLi"
                               @titleChange="titleChange"
                               @changeCompleted="changeCompleted"
                    >
                    </Create-li>
                </section>
                <footer :class="{ 'display-none': tabDisplay, 'tab-section': true }">
                    <div id="tab-div-wrap" >
                        <span id="item-span">{{countItem}} item left</span>
                        <ul id="tab-ul-item">
                            <li @click="clickBtn('all')">All</li>
                            <li @click="clickBtn('active')">Active</li>
                            <li @click="clickBtn('complete')">Completed</li>
                        </ul>
                        <button id="tab-button"
                                @click="clickClear">Clear completed</button>
                    </div>
                </footer>
            </section>
        </section>
        <footer id ="footer-content">
            <p id="p-content">MADE BY YOU</p>
        </footer>
    </div>
</template>

<script>
    import CreateLi from './CreateLi';
    export default {
        components: {
            CreateLi
        },
        name: 'Constructor',
        data() {
            return {
                list:
                    [
                        { id: 0, title: "11", completed: true},
                        { id: 1, title: 2, completed: true },
                        { id: 2, title: 3, completed: false }
                    ],
                changeInput: '',
                id: 4,
                status: 'all',
            }
        },

        computed:{
            countItem: function() {
                return this.list.length;
            },

            tabDisplay: function() {
                if (this.list.length < 1) return true;
                else return false
            },
            fliterData: function () {
                const activeItem = this.list.filter((item) => {
                    return item.completed === false;
                });
                const completedItem = this.list.filter((item) => {
                    return item.completed === true;
                });
                if (this.status ==='all') {
                    return this.list;
                } else if (this.status ==='active') {
                    return activeItem;
                }else return completedItem;
            },

            // 新时代黑科技。不懂不懂。。。
            changevalue: function() {return ''}
        },

        methods:{
            clickAll () {
                const handle = this.list.some((item) => {
                    return item.completed === false
                });
                if (handle) {
                    this.list.forEach((item) => {
                        item.completed = true
                    })
                } else {
                    this.list.forEach((item) => {
                        item.completed = false
                    })
                }

            },

            clickClear () {
                this.list = this.list.filter((item) => {
                    return item.completed === false
                })

            },

            inputKeyup (e) {
                if (e.keyCode === 13) {
                    const newItem ={
                        id: this.id,
                        title: this.changeInput,
                        completed: false
                    };
                    this.list.push(newItem);
                    this.id = ++this.id;
                }

            },

            inputChange (e) {
                this.changeInput = e.target.value;

            },

            removeLi (removeItem) {
                this.list = this.list.filter((item) => {
                   return item.id !== removeItem.id
                });

            },

            titleChange (changeItem, newItemTitle) {
                this.list.forEach((item) => {
                    if (item.id === changeItem.id ) {
                        if (newItemTitle !== '') {
                            item.title = newItemTitle;
                        } else this.removeLi(changeItem)
                    }
                });

            },

            changeCompleted (changeItem) {
                this.list.forEach((item) => {
                    if(item.id === changeItem.id) {
                        item.completed = !changeItem.completed
                    }
                })

            },

            clickBtn (btn) {
                this.status = btn;
            }

        }

    }
</script>

<style scoped>

    #contain-all{
        display:flex;
        flex-direction: column;
        height: 100%;

    }

    /* 头部样式*/
    #header-TodoList{
        flex: 0 0 10rem;
        display: flex;
        justify-content: center;

    }

    #toDoList-content{
        line-height: 10rem;
        font-size: 100px;
        color: rgba(175, 47, 47, 0.15);
        white-space: nowrap;

    }

    /* 输入部分样式*/
    #section-input{
        flex: 0 0 auto;
        display: flex;
        justify-content: center;


    }

    #wrap{
        flex: 0 1 40rem;
        display: flex;
        flex-direction: column;
        box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
                    0 25px 50px 0 rgba(0, 0, 0, 0.1);

    }

    /* 输入头部input*/
    #input-header{
        flex: 0 0 4.2rem;
        display: flex;
        background-color: white;

    }

    #input-write-content{
        outline: none;
        flex: 1 1 40rem;
        height: 4.2rem;
        border: none;
        font-size: 24px;
        padding-left: 60px;
        box-shadow: inset 0 -4px 4px rgba(0,0,0,0.03);


    }

    #input-write-content::placeholder{
        color: #9b9b9b;
        font-style: italic;
    }

    /*li显示部分*/
    #li-section{
        position: relative;
        display: flex;
        border-top: 1px solid #e6e6e6;

    }

    .check-all-button:before{
        content: '>';
        position: absolute;
        top: -45px;
        left: 22px;
        font-size: 30px;
        transform: rotateZ(90deg);

    }

    .opactity-1{
         opacity: 1;

     }

    .opactity-0{
        opacity: 0;

    }

    #check-all-input{
        display: none;

    }

    /* 按钮栏*/
    .tab-section{
        display: flex;
        box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.2),
                    0 8px 0 -3px #f6f6f6,
                    0 9px 1px -3px rgba(0, 0, 0, 0.2),
                    0 16px 0 -6px #f6f6f6,
                    0 17px 2px -6px rgba(0, 0, 0, 0.2);

    }

    #tab-div-wrap{
        display: flex;
        justify-content: space-around;
        flex: 0 1 40rem;
        height: 4.2rem;
        background: white;

    }

    #item-span{
        line-height: 4.2rem;

    }

    #tab-ul-item{
        list-style: none;
        padding: 0;
        margin: 0;
        display: inline;
    }

    #tab-ul-item li{
        line-height: 4.2rem;
        display: inline;
        padding:  5px;
        border: 1px solid rgba(175, 47, 47, 0.15);
        border-radius: 5px;
        cursor: pointer;
        margin: 0 2px;

    }

    #tab-button{
        padding: 0;
        margin: 0;
        outline: none;
        border: none;
        font-size: 14px;
        /*visibility: hidden;*/

    }

    #tab-button:hover{
        text-decoration: underline;
        cursor: pointer;

    }

    .display-none{
        display: none;

    }

    .display-block{
        display: block;

    }

    /* 尾部样式*/
    #footer-content{
        flex: 0 0 10rem;
        justify-items: flex-end;
        display: flex;
        justify-content: center;

    }

    #p-content{
        color: black;
        font-size: 30px;
        line-height: calc(10rem / 2);

    }

</style>

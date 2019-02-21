<template>
    <ul id="ul-content">
        <li v-for="item in list" :key="item.id">
            <div id="view" :class="classDiv(item)">

                <input class="li-input-check"
                       type="checkbox"
                       :checked="item.completed"
                       @click="changeCompleted(item)"
                >
                <label class="li-input-label"
                       @dblclick="openInput(item)"
                >{{item.title}}</label>
                <button id="li-input-button" @click="removeLi(item)">X</button>
            </div>
            <input :class="classInput(item)"
                   :key="item.id"
                   type="text"
                   @change="(e) => titleChange(e, item)"
                   @keyup="(e) => enterEvent(e,item)"
                   :value="item.title"
            >
        </li>
    </ul>
</template>

<script>
    export default {
        name: 'CreateLi',
        props: ['list'],
        data() {
            return {
                dblClickID: -1
            }
        },

        computed:{

        },

        methods:{
            classDiv (item) {
                if (item.id === this.dblClickID) {
                    return 'view-display-none'
                } else return 'view-display-block'
            },

            openInput (item) {
              this.dblClickID = item.id;


            },

            removeLi (item) {
                this.$emit('removeLi', item)

            },

            titleChange (e, item) {
                this.$emit('titleChange', item, e.target.value);
                this.dblClickID = -1;

            },

            changeCompleted (item) {
                this.$emit('changeCompleted', item);

            },

            enterEvent (e, item) {
                if(e.keyCode === 13) {
                    this.$emit('titleChange', item, item.title);
                    this.dblClickID = -1;
                }
            },

            // 修改样式的methods 使用methods合适否？ 本来使用的computed 但是无法传入参数比较 故改为methods
            // 逻辑是点击兄弟节点改变本身的样式 考虑维护一个变量通过点击改变数据促使样式转变 同时不能影响其他兄弟节点
            // 所以必须含有本节点的特征信息比如index或者ID属性 必须传送ID属性并核对ID属性从而改变

            // 修改输入input样式
            //
            classInput (item) {
                if (item.id === this.dblClickID) {
                    return 'li-input-text '
                } else return 'li-input-text li-input-text-display'

            },

        },

    }




</script>

<style scoped>
    #ul-content{
        list-style: none;
        margin: 0;
        padding: 0;

    }

    #ul-content li{
        box-sizing: border-box;
        width: 40rem;
        min-height: 4.2rem;
        background-color: white;
        font-size: 28px;
        line-height: 4.2rem;
        padding-left: 60px;
        border-bottom: 1px solid #ededed;

    }

    #view{
        position: relative;

    }

    .view-display-block{
        display: block;
    }

    .view-display-none{
        display: none;

    }

    #view:hover > #li-input-button{
        opacity: 1;

    }

    .li-input-check{
        position: absolute;
        width: 60px;
        height: 4.2rem;
        left: -60px;
        top: calc(50% - 30px);
        margin: 0;
        opacity: 0;
        z-index: 2;

    }

    .li-input-label::before{
        content: '';
        background: url('../assets/0.svg') no-repeat left center;
        position: absolute;
        width: 55px;
        height: 4.2rem;
        left: -60px;
        top: calc(50% - 30px);
        margin-left: 5px;

    }


    .li-input-check:checked + .li-input-label:before{
        background: url('../assets/1.svg') no-repeat left center;

    }

    .li-input-check:checked + .li-input-label{
        text-decoration: line-through;
        color: #d9d9d9;

    }

    .li-input-label{
        width: 31rem;
        display: inline-block;
        word-wrap: break-word;

    }

    #li-input-button{
        float: right;
        outline: none;
        border: none;
        font-size: 20px;
        height: 4rem;
        padding-right: 20px;
        color: #cc9a9a;
        opacity: 0;

    }

    .li-input-text{
        box-sizing: border-box;
        outline: none;
        height: 4.2rem;
        border: none;
        font-size: 28px;
        line-height: 28px;
        width: 100%;
        box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
        padding: 0;
        display: block;

    }

    .li-input-text-display{
        display: none;
    }

</style>

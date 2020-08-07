<template>
    <div class="list">
        <ul>
            <li v-for="(item,index) in list" :key="index" >
                <div class="inp">
                    <input type="checkbox" :checked="item.check" @click="click(index)" />
                </div>
                <div class="img">
                    <img :src="'/static/img/'+item.img" alt="">
                </div>
                <div class="content">
                    <h4>{{item.name}}</h4>
                    <p>￥{{item.price}}</p>
                </div>
                <div class="btn">
                    <span>
                        <button @click="jia(index)">+</button>{{item.num}}<button @click="jian(index)">-</button>
                    </span>
                </div>
            </li>
        </ul>
        <footer>
            <input type="checkbox" :checked="num == list.length ? true : false" @change="all($event)"/>全选  <p class="hj">合计：￥{{money}}</p>
        </footer>
    </div>
</template>

<script>
export default {
    name: 'List',
    data () {
        return {
            list : JSON.parse(window.localStorage.getItem('cart')) || [],
            num : 0
        }
    },
    methods:{
        jia(index){
            this.list[index].num++
            window.localStorage.setItem('cart',JSON.stringify(this.list))
        },
            //减法运算
        jian(index){
            if(this.list[index].num<=1){
                alert('在减就没有了')
                return false
            }
            this.list[index].num--
            localStorage.setItem('cart',JSON.stringify(this.list))
        },
        all(e){
            this.num = 0;
            this.list.forEach((item)=>{
                item.check = e.target.checked
                if(item.check){
                    this.num++
                }else{
                    this.num = 0;
                }
            })
        },
        click(index){
            this.list[index].check = !this.list[index].check;
            this.list[index].check ? this.num++ : this.num--
        }
    },
    computed:{
        money(){
            let sum = 0;
            this.list.forEach((item)=>{
                if (item.check == true) {
                    sum += item.price * item.num
                }
            })
            return sum
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list{
    background: rgb(135, 190, 204);
    margin-top:-10px; 
}
.hj{
    display: flex;
    color:black; 
    font-weight: 700;
    background: red;
    padding-left:10px; 
    align-items: center;
    border-radius:15px; 
    height: 28px;
    width: 110px;
    border: 1px red solid;
    margin-left:280px; 
    position: relative;
    bottom:30px; 
}
ul{width: 100%;}
li{width: 95%;height: 100px;box-shadow: 0 2px 3px #ccc;margin: 10px auto;display: flex;}
.inp{width: 10%;}
.inp input{display: block;margin: auto;margin-top: 40px;}
.img{width: 20%;}
img{width: 50px;height: 85px;display: block;margin: auto;}
.content{width: 55%;}
.content h4{margin-top: 5px;}
.content p{margin-top: 25px;}
.btn{width: 15%;position: relative;}
span{position: absolute;right: 10px;bottom: 10px; width: 50%;}
button{
    background: red;
    border: white 3px solid;
    border-radius:15px; 
    width: 20px;
    height: 20px;
    color: white;
    align-items: center;
}
footer{width: 100%;height: 30px; position: absolute; bottom: 0;left: 0;}
footer>input{margin-left: 20px;}
</style>

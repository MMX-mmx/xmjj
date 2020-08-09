<template>
    <div class="hello">
        <div class="sort">
            <p v-show="!flag" @click="click()">升序</p>
            <p v-show="flag" @click="click()">降序</p>
        </div>



        <ul>
            <li v-for="(item,index) in list" :key="index">
                <dt>
          <img :src="'/static/img/'+item.productImage" alt class="img" />
        </dt>
        <dd class="content">
          <p class="p1">{{item.productName}}</p>
          <p class="p2">￥{{item.salePrice}}</p>
        </dd>
                <div class="btn">
                    <button @click="add(item)">+</button>
                </div>
            </li>
        </ul>
        <div class="top" id="one">
            <a href="#top">↑</a>  
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Home',
    data () {
        return {
            list : ''
        }
    },
    created(){
        axios.get('static/data.json').then((res)=>{
            console.log(res.data)
            this.list = res.data.result.list;
        })
    },
    methods:{
        add(item){
            let car = JSON.parse(window.localStorage.getItem('cart')) || [];
            let index = car.findIndex((a,b)=>{
                return item._id == a.id
            })
            let obj = {id:item._id,img:item.productImage,name:item.productName,price:item.salePrice,num:1,check: false}
            if(index == -1){
                car.push(obj)
            }else{
                car[index].num++
            }
            window.localStorage.setItem('cart', JSON.stringify(car))
			this.$router.push('/list')
        },
        click(){
            this.flag = !this.flag;
            if(this.flag){
                this.list.sort(function (a,b){
                    return b.salePrice - a.salePrice;
                });
            }else{

                this.list.sort(function (a,b){
                    return a.salePrice - b.salePrice;
                });
            }
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.top{
    
    width: 27px;
    height: 40px;
    border: 5px solid red;
    border-radius:24px; 
    align-items: center;
    padding-left:13px;
    font-size: 30px;
    color: black;
    background: white;
}
#one{
    position: fixed;
    bottom: 10px;
    left: 350px;
}
.hello{
    overflow: scroll;
}
.sort{
    background: skyblue;
    padding-bottom:10px; 
    width: 100%;
    height: 20px;
    padding-left:180px; 
    font-size:20px; 
    font-weight: 600;
    color: whitesmoke;
}
ul{
    width: 100%;
}
li{
    width: 95%;
    height: 100px;
    box-shadow: 0 2px 3px #ccc;
    margin: 10px auto;display: flex;
}
.img{
    width: 100px;
    }
img{
    width: 50px;
    height: 85px;
    display: block;
    margin: auto;
    }
.content{
    position: relative;
    left: 150px;
    width: 65%;
    
}
.content h4{
    margin-top: 5px;

}
.content p{
    margin-top: 25px;
    
    }
.btn{
    width: 15%;
    position: relative;
    }
button{
    position: absolute;
    right: 5px;
    bottom: 10px;
    width: 30px;
    height: 20px;
    background: purple;
    color: #fff;
    border-radius: 20px;
    border: none;
    outline: none;
    }
</style>

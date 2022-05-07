<template>
    <h2>Личный кабинет</h2>
    <p>
        <span>Баланс {{ user.balance }}₽</span>
        <span v-if="user.basket.booksBoughtPrice > 0">
             - {{ user.basket.booksBoughtPrice }}₽ &nbsp; = &nbsp; 
            <span :style="user.balance - user.basket.booksBoughtPrice>=0?'color: white;':'color: rgb(240, 10, 10);'">
                {{ user.balance - user.basket.booksBoughtPrice }}₽
            </span>
        </span>
    </p>
    <h3>Корзина:</h3>
    <booksList showAmount @discardBook="discardBook" :books="user.basket.books" action="discard"/>
    <div class="buyButtonContainer">
        <myButton v-if="user.basket.booksBought > 0" @click="buy">Купить</myButton>
        <notification style="" ref="notification">Недостаточно денег на балансе</notification>
    </div>
</template>

<script>
import booksList from './booksList.vue';
import myButton from './myButton.vue';
import notification from './notification.vue';

export default {
    components: { booksList, myButton, notification },
    emits: ["discardBook"],
    props: {
        user: {
            type: Object,
            default: () => {}
        }
    },
    methods: {
        buy(){
            if(this.user.balance - this.user.basket.booksBoughtPrice >= 0){
                this.user.balance -= this.user.basket.booksBoughtPrice;
                this.user.basket.booksBoughtPrice = 0;
                this.user.basket.booksBought = 0;
                this.user.basket.books = [];
            }else{
                this.$refs.notification.notify();
            }
        },
        discardBook(id){
            let index = this.user.basket.books.findIndex(e => e.id == id);
            if(this.user.basket.books[index].amount > 1){
                this.user.basket.books[index].amount--;
            }else{
                this.user.basket.books.splice(index, 1);
            }
            this.$emit("discardBook", id);
        }
    }
}
</script>
<style>
    .buyButtonContainer{
        margin-top: 10px;
        display: flex;
        height: 35px;
        width: 105%;
        justify-content: space-between;
        align-items: center;
    }
    @media (max-width: 768px){
        .buyButtonContainer{
            width: 108%;
        }
    }
</style>
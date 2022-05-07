<template>
    <div class="parentContent">
        <div class="mainContent">
            <div class="leftContent scroll">
                <h2>Доступные книги</h2>
                <booksList @toBasket="toBasket" :books="books"/>
            </div>
            <div class="separator"></div>
            <div class="rightContent">
                <profile :user="user" @discardBook="discardBook"/>
            </div>
        </div>
    </div>
</template>

<script>
import booksList from './components/booksList.vue';
import profile from './components/profile.vue';

export default {
    components: { booksList, profile },
    data(){
        return {
            user: {
                balance: 100,
                basket: {
                    books: [],
                    booksBought: 0,
                    booksBoughtPrice: 0,
                }
            },
            books: [
                {
                    id: 1,
                    name: 'Совершенный код. Мастер-класс',
                    price: 100,
                    amount: 3
                },
                {
                    id: 2,
                    name: 'Rapid Development, Steve McConnell',
                    price: 180,
                    amount: 2
                },
                {
                    id: 3,
                    name: 'Искусство программирования, Д.Кнут',
                    price: 210,
                    amount: 1
                }
            ]
        }
    },
    methods: {
        toBasket(id){
            let book = this.books.find(e => e.id == id);
            book.amount--;
            this.user.basket.booksBought++;
            this.user.basket.booksBoughtPrice += book.price;

            let index = this.user.basket.books.findIndex(e => e.id == book.id);
            if(index != -1){
                this.user.basket.books[index].amount++;
            }else{
                let newBook = Object.assign({}, book);
                newBook.amount = 1;
                this.user.basket.books.push(newBook);
            }
        },
        discardBook(id){
            let book = this.books.find(e => e.id == id);
            book.amount++;
            this.user.basket.booksBought--;
            this.user.basket.booksBoughtPrice -= book.price;
        }
    },
    mounted(){
        for(let i = 4; i < 50; i++){
            this.books.push({
                id: i,
                name: 'Искусство программирования, Д.Кнут',
                price: 210,
                amount: 1
            });
        }
    }
}
</script>
<style>
    body{
        background-color: #181818;
        color: white;
    }
    .parentContent{
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 100%;
    }
    .mainContent{
        position: absolute;
        width: 80%;
        height: 80%;
        box-shadow: 1px 1px 6px  4px rgb(0 255 255 / 20%),
                    0px 2px 2px      rgb(0 255 255 / 14%),
                    0px 3px 4px -2px rgb(0 255 255 / 12%);
        border-radius: 5px;
        display: flex;
    }
    h2{
        margin-top: 0;
    }
    .leftContent{
        width: 60%;
        padding: 20px;
    }
    .separator{
        border-left: 1px solid rgb(0 255 255 / 50%);
        height: 90%;
        margin-top: auto;
        margin-bottom: auto;
    }
    .rightContent{
        width: 40%;
        padding: 20px;
    }

    @media (min-width: 769px){
        .leftContent{
            overflow: scroll;
            overflow-x: hidden;
            max-height: 94%;
        }
        .leftContent::-webkit-scrollbar {
            width: 9px;
        }
        .leftContent::-webkit-scrollbar-thumb {
            background-color: rgb(59, 59, 82);
            border-radius: 20px;
            border-style: solid;
            border-width: 2px;
            border-color: #181818;
        }
        .rightContent{
            overflow: scroll;
            overflow-x: hidden;
            max-height: 94%;
        }
        .rightContent::-webkit-scrollbar {
            width: 9px;
        }
        .rightContent::-webkit-scrollbar-thumb {
            background-color: rgb(59, 59, 82);
            border-radius: 20px;
            border-style: solid;
            border-width: 2px;
            border-color: #181818;
        }
    }
    @media (max-width: 1024px){
        .mainContent{
            width: 95%;
            height: 95%;
        }
    }
    @media (max-width: 768px){
        .mainContent{
            display: block;
            overflow: scroll;
            overflow-x: hidden;
        }
        .mainContent::-webkit-scrollbar {
            width: 9px;
        }
        .mainContent::-webkit-scrollbar-thumb {
            background-color: rgb(59, 59, 82);
            border-radius: 20px;
            border-style: solid;
            border-width: 2px;
            border-color: #181818;
        }
        .leftContent{
            width: 90%;
            padding-top: 0px;
        }
        .separator{
            border-left: none;
            border-bottom: 1px solid rgb(0 255 255 / 50%);
            height: 0;
            width: 85%;
            margin-left: auto;
            margin-right: auto;
        }
        .rightContent{
            width: 90%;
            padding-top: 0;
        }
    }
</style>
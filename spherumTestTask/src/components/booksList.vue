<template>
    <template v-for="book in books" :key="book.id">
        <bookCard v-if="book.amount > 0" @action="toBasket" :book="book" :showAmount="showAmount" :actionLabel="actions[action].label"/>
    </template>
</template>

<script>
import bookCard from './bookCard.vue';

export default {
    components: { bookCard },
    emits: ["discardBook", "toBasket"],
    props: {
        books: {
            type: Array,
            default: () => []
        },
        action: {
            type: String,
            default: () => "toBasket"
        },
        showAmount: {
            type: Boolean,
            default: () => false
        }
    },
    data(){
        return {
            actions: {
                toBasket: {
                    label: "В корзину",
                    emit: "toBasket"
                },
                discard: {
                    label: "Удалить",
                    emit: "discardBook"
                }
            }
        }
    },
    methods: {
        toBasket(id){
            this.$emit(this.actions[this.action].emit, id);
        }
    }
}
</script>
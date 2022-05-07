<template>
    <div class="bookCard">
        <div style="flex-grow: 2;">
            <p style="margin-bottom: 5px;"><b>{{ book.name }}</b></p>
            <p>
                <span>Цена: {{ book.price }}₽</span>
                <span v-if="showAmount">; {{ book.amount }}</span>
            </p>
        </div>
        <div style="display: flex;">
            <myButton @click="$emit('action', book.id);">{{ actionLabel }}</myButton>
        </div>
    </div>
</template>

<script>
import myButton from './myButton.vue';

export default {
    components: { myButton },
    props: {
        book: {
            type: Object,
            default: () => {}
        },
        actionLabel: {
            type: String,
            default: () => "В корзину"
        },
        showAmount: {
            type: Boolean,
            default: () => false
        }
    }
}
</script>
<style>
    .bookCard{
        margin: 5px;
        border-style: solid;
        border-width: 2px;
        padding: 5px;
        display: flex;
    }
    .bookCard p{
        margin: 0;
    }
</style>
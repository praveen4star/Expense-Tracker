<script setup>
    import { defineProps, defineEmits } from 'vue';
    import { useToast } from 'vue-toastification';

    const toast = useToast();
    const emit = defineEmits(['deleteTransaction'])

    defineProps({
        transactions: {
            type: Array,
            required: true
        }
    })
    const deleteTransaction = (id) =>{
        emit('deleteTransaction', id);
        toast.info('Transaction removed');
    }
</script>
<template>
    <h3> History </h3>
    <ul class="list" id="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount > 0 ? 'plus' : 'minus'">
            {{ transaction.text}} 
            <span>
                {{  transaction.amount > 0 ? '+' : '-' }}
                ${{ Math.abs(transaction.amount) }}
            </span> 
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
        <!-- <li class="minus">
            Cash <span>-$400</span> <button class="delete-btn">x</button>
        </li>
        <li class="plus">
            PayCheck <span>-$800</span> <button class="delete-btn">x</button>
        </li> -->
    </ul>
</template>
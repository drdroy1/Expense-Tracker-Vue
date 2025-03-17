<template>
    <div>
        <h3>Add New Transaction</h3>
        <form id="form" @submit.prevent="onSubmit">
            <div class="form-control">
                <label for="text">Text</label>
                <input type="text" id="text" v-model="text" placeholder="Enter text..." />
            </div>
            <div class="form-control">
                <label for="amount">Amount<br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount..."/>
            </div>
            <button class="btn">Add Transaction</button>
        </form>
    </div>
</template>

<script setup>

import {ref} from 'vue';
import{useToast} from 'vue-toastification';
const text = ref('');
const amount = ref('');

const toast = useToast();

const emit = defineEmits(['submittedTransaction']);

//Errored Transaction
    const onSubmit = () => {
        if(!text.value || !amount.value){
            //Toast notification error for fields
            toast.error('Both fields must be filled'); 
            return;
        };

//Submitted Transaction
    const transactionData = {
            text: text.value,
            amount: parseFloat(amount.value),
        };
        emit('submittedTransaction', transactionData);

        //Clear fields from form after submit
        text.value ='';
        amount.value ='';
    };

</script>

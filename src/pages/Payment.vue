<script setup>
import { useVuelidate } from '@vuelidate/core'
import { required, numeric, minValue, minLength, maxLength, helpers } from '@vuelidate/validators'
import { reactive } from 'vue'
import {  useRouter } from 'vue-router'
import Button from '@/components/Button.vue'

const router = useRouter()

const state = reactive({
    cardholder_name: '',
    card_number: '',
    exp_year: '',
    exp_month: '',
    cvc: '' 
})

const rules = {
    cardholder_name: { 
        required: helpers.withMessage("can't be blank", required), 
        $lazy: true 
    }
    , 
    card_number: { 
        required: helpers.withMessage("can't be blank", required), 
        numeric: helpers.withMessage("Wrong format, numbers only", numeric), 
        $lazy: true 
    },
    exp_year: { 
        required: helpers.withMessage("can't be blank", required), 
        numeric: helpers.withMessage("Wrong format, numbers only", numeric), 
        minLengthValue: helpers.withMessage("Wrong format, year only", minLength(2)), 
        maxLengthValue: helpers.withMessage("Wrong format, year only",  maxLength(2)), 
    },
    exp_month: { 
        required: helpers.withMessage("can't be blank", required), 
        numeric: helpers.withMessage("Wrong format, numbers only", numeric), 
         minLengthValue: helpers.withMessage("Wrong format, month only", minLength(2)), 
        maxLengthValue: helpers.withMessage("Wrong format, month only",  maxLength(2)), 
    },
    cvc: { 
        required: helpers.withMessage("can't be blank", required), 
        minLengthValue: minLength(3), 
        maxLengthValue: maxLength(3),
    },
}

const v$ = useVuelidate(rules, state)

async function submitForm() {
    const result = await this.v$.$validate() // checks all inputs
    if (result) {
        router.push('/thank-you')
    } 
}
</script>
<template>
    <form class="w-full max-w-lg">
        <div class="flex flex-wrap mb-6 -mx-3">
            <div class="w-full px-3 ">
                <label class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase" for="cardholder-name">
                    Cardholder Name
                </label>
                <input 
                    class="block w-full px-4 py-3 mb-3 leading-tight text-gray-700 border rounded appearance-none focus:outline-none focus:border-indigo-900 focus:bg-white" 
                    id="cardholder-name" 
                    :class="{ 'border-red-500': v$.cardholder_name.$errors.length }"
                    type="text"
                    name="cardholder-name"
                    v-model="state.cardholder_name"
                    @input="v$.cardholder_name.$touch()"
                    placeholder="e.g. Jane Appleseed"
                >
                <p v-for="error of v$.cardholder_name.$errors" :key="error.$uid" class="text-xs italic text-red-500 ">
                    {{ error.$message }}
                </p>
            </div>
        </div>
        <div class="flex flex-wrap mb-6 -mx-3">
            <div class="w-full px-3 ">
                <label class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase" for="cardholder-number">
                    Card Number
                </label>
                <input 
                    class="block w-full px-4 py-3 mb-3 leading-tight text-gray-700 border rounded appearance-none focus:outline-none focus:border-indigo-900 focus:bg-white" 
                    id="cardholder-number" 
                    type="text" 
                    :class="{ 'border-red-500': v$.card_number.$errors.length }"
                    name="cardholder-number"
                    v-model="state.card_number"
                    @input="v$.card_number.$touch()"
                    placeholder="e.g. 1234 5678 9123 0000"
                >
                <p v-for="error of v$.card_number.$errors" :key="error.$uid" class="text-xs italic text-red-500">
                    {{ error.$message }}
                </p>
            </div>
        </div>
        <div class="flex flex-wrap mb-6 -mx-3">
            <div class="w-full px-3 mb-6 md:w-1/2 md:mb-0">
                <label class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase" >
                    Exp. Date (MM/YY)
                </label>
                <div class="flex space-x-3">
                    <input 
                        class="block w-full px-4 py-3 leading-tight text-gray-700 border border-gray-200 rounded appearance-none focus:outline-none focus:border-indigo-900" 
                        type="text" 
                        :class="{ 'border-red-500': v$.exp_month.$errors.length }"
                        name="exp_year"
                        v-model="state.exp_month"
                        @input="v$.exp_month.$touch()"
                        placeholder="MM"
                    >
                     <input 
                        class="block w-full px-4 py-3 leading-tight text-gray-700 border border-gray-200 rounded appearance-none focus:outline-none focus:border-indigo-900" 
                        type="text" 
                        :class="{ 'border-red-500': v$.exp_year.$errors.length }"
                        name="exp_year"
                        v-model="state.exp_year"
                        @input="v$.exp_year.$touch()"
                        placeholder="YY"
                    >
                </div>
                 <p v-for="error of v$.exp_year.$errors" :key="error.$uid" class="text-xs italic text-red-500 ">
                    {{ error.$message }}
                </p>
                <p v-for="error of v$.exp_month.$errors" :key="error.$uid" class="text-xs italic text-red-500 ">
                    {{ error.$message }}
                </p>
            </div>
            <div class="w-full px-3 mb-6 md:w-1/2 md:mb-0">
                <label class="block mb-2 text-xs font-bold tracking-wide text-gray-700 uppercase" for="cvc">
                    CVC
                </label>
                <input 
                    class="block w-full px-4 py-3 leading-tight text-gray-700 border border-gray-200 rounded appearance-none focus:outline-none focus:border-indigo-900" 
                    type="password" 
                    :class="{ 'border-red-500': v$.cvc.$errors.length }"
                    id="cvc"
                    v-model="state.cvc"
                    @input="v$.cvc.$touch()"
                    placeholder="e.g. 123"
                >
                 <p v-for="error of v$.cvc.$errors" :key="error.$uid" class="text-xs italic text-red-500 ">
                    {{ error.$message }}
                </p>
            </div>
        </div>
        <div class="flex flex-wrap mb-2">
            <Button @click="submitForm()" title="Confirm" />
        </div>
    </form>
</template>
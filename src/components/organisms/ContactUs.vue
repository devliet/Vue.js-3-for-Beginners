<template>
 <Form  @submit="handleSubmit" :validation-schema="schema"  v-slot="{ values, validate }">
    <Field name="email2" id="email2" type="email" value="" />
    <ErrorMessage name="email2" />

    <Field name="password" id="password2" type="password" value="wsxq" />
    <ErrorMessage name="password" />

    <pre>{{ values }}</pre>

    <TheButton>Send</TheButton>
    <TheButton @click="myFunction()" type="button">not submit</TheButton>
    <button type="button" @click="validate">Validate only</button>
  </Form>

    <!-- <Form ref="contactForm2" @submit="onSubmit2" :validation-schema="schema">
        <label for="email2">Enter your message type 2</label>
        <Field name="email2" type="email" :rules="validateEmail"></Field>
        <ErrorMessage name="email2" />
    </Form> -->
</template>

<script setup>
import { Field, Form, ErrorMessage, defineRule } from 'vee-validate';
import * as yup from 'yup';
//const validateEmail = yup.string().required().email();

import TheButton from '../atoms/TheButton.vue';
import { required, email, min } from '@vee-validate/rules';
import { ref} from 'vue';

// defineRule('required', value => {
//     if(!value || !value.length){
//         return 'This field is required';
//     }
//     return true;
// })
const schema = yup.object({
  email2: yup.string().required().email(),
  password: yup.string().required().min(8),
});



defineRule('required', required);
//defineRule('email', email);
defineRule('min', min);

const myFunction = () => {
    console.log("val:",  values.email2)
    console.log("pass:", values.password2)
  //  return false

}

// function myFunction(){
//     console.log("not submitting")
// }

function onSubmit2(values){
    alert(JSON.stringify(values, null, 2));
}

// const handleSubmit = ({ email, message }) => {
//     console.log("email:", email)
//     console.log("message:", message)

// }

function handleSubmit(values){
    alert(JSON.stringify(values, null, 2));
    console.log(values.email2)
}

</script>
<style scoped>
form {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 16px;
    padding-bottom: 16px;
    width: 60%;
}

.error {
    color: red;
}
</style>
<template>
  <modal
        title="Modal with form"
        @close="$emit('close')">


      <!--  BODY  -->
        <div slot="body">
          <form @submit.prevent="onSubmit">
              <!--  Name  -->
            <div class="form-item" :class="{ errorImput: $v.name.$error }">
                <label>Name:</label>
                <p class="errorText" v-if="$v.name.required"> Filed is required! </p>
                <p class="errorText" v-if="$v.name.minLength"> Name must have at least {{ $v.name.$params.minLength.min }} !</p>

                <input
                    v-modal="name"
                    :class="{error: $v.name.$error}"
                    @change="$v.name.$touch()">
            </div>
            <!-- email -->
            <div class="form-item" :class="{ errorImput: $v.email.$error }">
                <label>Email:</label>
                <p class="errorText" v-if="$v.email.required"> Filed is required! </p>
                <p class="errorText" v-if="$v.email.email"> Email is not correct! </p>

                <input
                    v-modal="email"
                    :class="{error: $v.email.$error}"
                    @change="$v.email.$touch()">
            </div>

            <!-- button -->
            <label>Email:</label>
            <input v-bind="email" > 
            <button class="btn btnPrimary">Submit!</button>
          </form>
        </div>
        </modal>  
</template>

<script>

import{ required, minLength, email} from 'vuelidate/lib/validators'

import modal from '@components/UI/Modal.vue'

export default {
    components: {modal},
    data (){
        return{
            name:'',
            email:''
        }
    },
        validations: {
            name:{
                required,
                minLength: minLength(4)
            },
        email: {
            required,
            email
        }
    },
    onSubmit () {
        this.$v.$touch()
        if (this.$v.$invalid){
            const user = {
                name: this.name,
                email: this.email
            }
            console.log(user)
            //DONE!
            this.name = ''
            this.email = ''
            this.$v.$reset()
            this.$emit('close')
        }
    }
}
</script>


<style lang="scss">
    .form-item .errorText{
        display: none;
        margin-bottom: 8px;
        font-size: 13.4px;
        color: #de4040;
    }
    
    .form-item {
        &.errorInput .errorText{
            display: block;
        }
}
    input.error{
        border-color: #de4040;
    }
</style>
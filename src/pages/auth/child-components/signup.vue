<template lang="pug">
form(@submit.prevent="signup")
    alert(:error="errorMessage", :success="successMessage")
    .row
        .col-md-6
            md-input-container(:class="{'md-input-invalid': errors.has('email')}")
                label Email
                md-input(type="text", name="email", v-model="email", v-validate="'required|email'", required)
                span.md-error {{errors.first('email')}}
        .col-md-6
            md-input-container
                label Company
                md-input(type="text", name="company", v-model="company")
    .row
        .col-md-6
            md-input-container(:class="{'md-input-invalid': errors.has('name')}")
                label Name
                md-input(type="text", name="name", v-model="name", v-validate="'required'", required)
                span.md-error {{errors.first('name')}}
        .col-md-6
            md-input-container
                label Surname
                md-input(type="text", name="surname", v-model="surname")
    .row
        .col-md-6
            md-input-container(:class="{'md-input-invalid': errors.has('password')}")
                label Password
                md-input(type="password", name="password", v-model="password", v-validate="'required'", required)
                span.md-error {{errors.first('password')}}
        .col-md-6
            md-input-container(:class="{'md-input-invalid': errors.has('repeated')}")
                label Repeated password
                md-input(type="password", name="repeated" v-model="repeated", v-validate="'required|confirmed:password'", required)
                span.md-error {{errors.first('repeated')}}
    .submit-block
        md-button.md-raised.md-primary.submit(type="submit", :disabled="errors['errors'].length > 0") Sign up
</template>

<script>
    import { Validator } from 'vee-validate';
    import FormAlert from '../../../components/FormAlert';

    export default {
        name: 'sign-up',
        components: {
            'alert': FormAlert
        },
        props: ['errorMessage', 'successMessage'],
        data: () => {
            return {
                email: '',
                name: '',
                company: '',
                surname: '',
                password: '',
                repeated: '',
                errors: null
            };
        },
        watch: {
            email(value) {
                this.validator.validate('email', value);
            },
            name(value) {
                this.validator.validate('name', value);
            },
            password(value) {
                this.validator.validate('password', value);
            },
            repeated(value) {
                this.validator.validate('repeated', value);
            }
        },
        methods: {
            signup() {
                this.validator.validateAll()
                    .then(() => {
                        this.$emit('signup', {
                            email: this.email,
                            name: this.name,
                            company: this.company,
                            surname: this.surname,
                            password: this.password
                        });
                    })
                    .catch((errors) => {
                        console.log(errors);
                    });
            }
        },
        created() {
            this.validator = new Validator({
                email: 'required|email',
                name: 'required',
                password: 'required',
                repeated: 'required|confirmed:password'
            });
            this.$set(this, 'errors', this.validator.errorBag);
        }
    }
</script>

<style lang="stylus">
    
</style>
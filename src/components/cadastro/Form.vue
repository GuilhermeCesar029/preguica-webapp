<template>
    <div>
        <v-form
            ref="form"
            v-model="valid"
        >
            <v-text-field
                v-model="name"
                required
                filled
                prepend-icon="mdi-account-circle"
                label="Nome"
                :rules="nameRules"
            />
            <v-text-field
                v-model="email"
                required
                filled
                prepend-icon="mdi-email"
                label="E-mail"
                :rules="emailRules"
            />
            <v-text-field
                v-model="password"
                :append-icon="e1 ? 'mdi-visibility' : 'mdi-visibility_off'"
                :type="e1 ? 'password' : 'text'"
                :rules="senhaRules"
                label="Senha"
                required
                filled
                prepend-icon="mdi-lock"
                @click:append="() => (e1 = !e1)"
            />
            <v-card-actions>
                <v-row>
                <v-col cols="12">
                    <v-btn
                        :disabled="!valid"
                        color="success"
                        block
                        @click="submit()"
                    >
                       Cadastrar
                    </v-btn>
                </v-col>
                <v-col cols="12">
                    <cancel-btn 
                        :to="{ name: 'login'}" 
                        block 
                    />
                    </v-col>
                </v-row>
            </v-card-actions>
        </v-form>
    </div>
</template>

<script>
    import { mapActions, mapGetters } from 'vuex';
    export default {
        data() {
            return {
                valid: false,
                e1: 'visibility',
                password: '',
                senhaRules: [
                    v => !!v || 'Preencher Senha!',
                ],
                nameRules: [
                    v => !!v || 'Preencher o nome!',
                ],
                email: '',
                emailRules: [
                    v => !!v || 'E-mail is required',
                    v => /.+@.+\..+/.test(v) || 'E-mail precisa ser valido',
                ],
                name: ''
            };
        },
        computed: {
            ...mapGetters({
                usuarioGetter: 'usuario/usuarioGetter',
            }),
        },
        watch: {
            usuarioGetter(value) {
                if (value.status) {
                    window.location.replace(value.redirect);
                }
            },
        },
        methods: {
                ...mapActions({
                insertUserAction: 'usuario/insertUserAction',
            }),
            submit() {
                const user = { name: this.name, 
                    email: this.email, 
                    password: this.password ,
                    redirect: true
                };
                this.insertUserAction(user);
            },
        }
    }
    
</script>
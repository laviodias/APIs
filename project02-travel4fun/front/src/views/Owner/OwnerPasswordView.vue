<script>
import Header from '../../components/Owner/HeaderPage.vue'
import Footer from '../../components/Owner/FooterPage.vue'
import Modal from '../../components/AlertModal.vue'
import { instance } from '../../services';

export default {
    components: {
        Header,
        Footer,
        Modal
    },
    data() {
        return {
            password: '',
            password_confirmation: '',
            activeClass: '',
            titleModal: '',
            messageModal: 'Are you sure?'
        }
    },
    methods: {
        updateModal() {
            this.titleModal = 'Change Password'
            this.activeClass = 'is-active'
        },
        update() {
            instance.patch('http://127.0.0.1:8000/api/user/password',
                {
                    password: this.password,
                    password_confirmation: this.confirmationPassword,
                })
                .then((response) => {
                    this.error = ''
                    this.activeClass = ''
                    this.password = ''
                    this.password_confirmation = ''
                    this.$store.state.notification = 'Password updated successfully'
                    this.$store.state.show = true
                    this.$store.state.toastClass = 'is-success'
                    this.$store.dispatch('show')
                })
                .catch((error) => {
                    this.error = error.response.data
                    this.activeClass = ''
                    this.$store.state.notification = 'Something went wrong'
                    this.$store.state.show = true
                    this.$store.state.toastClass = 'is-danger'
                    this.$store.dispatch('show')                    
                })
        }
    }
}
</script>

<template>
    <Header></Header>

    <main>
        <div class="container is-max-desktop my-4">
            <form class="box" @submit.prevent="updateModal">
                <router-link to="/dashboard" class="button mb-5">Return</router-link>
                <h1 class="has-text-centered is-size-4 has-text-weight-bold mb-5">Change Password</h1>
                <div class="field-body">
                    <div class="field">
                        <p class="control has-icons-left">
                            <input class="input" :class="[error?.password?.[0] ? 'is-danger' : '']" type="password"
                                placeholder="Password" v-model="password">
                            <span class="icon is-small is-left">
                                <i class="fas fa-lock"></i>
                            </span>
                        </p>
                        <p class="help is-danger">{{ error?.password?.[0] }}</p>
                    </div>
                    <div class="field">
                        <p class="control has-icons-left">
                            <input class="input" :class="[error?.password_confirmation?.[0] ? 'is-danger' : '']"
                                type="password" placeholder="Confirmation password" v-model="confirmationPassword">
                            <span class="icon is-small is-left">
                                <i class="fas fa-lock"></i>
                            </span>
                        </p>
                        <p class="help is-danger">{{ error?.password_confirmation?.[0] }}</p>
                    </div>
                </div>
                <div class="field mt-5 is-grouped is-grouped-centered">
                    <div class="control">
                        <button type="submit" class="button is-info px-6">Change Password</button>
                    </div>
                </div>
            </form>
        </div>
    </main>

    <Footer></Footer>

    <Modal :activeClass="this.activeClass" :update="updateItem" :title="this.titleModal" :message="this.messageModal">
    </Modal>
</template>
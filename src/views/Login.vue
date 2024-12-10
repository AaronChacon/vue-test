<template>
  <div class="main">
    <main>  
      <div class="container col-xl-10 col-xxl-8 px-4 py-5">
        <div class="row align-items-center g-lg-5 py-5">
          
          <div class="col-lg-6 text-center text-lg-start">
            <h1 class="display-4 fw-bold lh-1 mb-3">aaronch.dev</h1>
            <p class="col-lg-10 fs-4">Code Test - interview 🙌</p>
          </div>

          <div class="col-md-10 mx-auto col-lg-6">

            <form class="p-4 p-md-5 border rounded-3 bg-light" @submit.prevent="onSignIn">
              <div class="form-floating mb-3">
                <input 
                  class="form-control"
                  type="text" 
                  id="username" 
                  placeholder="Username"
                  v-model="username"
                >
                <label for="username">Username</label>
              </div>
              <div class="form-floating mb-3">
                <input 
                  class="form-control"
                  type="password" 
                  id="inputPassword" 
                  placeholder="Password"
                  v-model="password"
                  >
                <label for="inputPassword">Password</label>
              </div>

              <button class="w-100 btn btn-lg btn-dark" type="submit" :disabled="loading">
                <span>Sign in</span>
                <!-- Loader -->
                <span v-if="loading" class="spinner-border spinner-border-sm mx-2" role="status" aria-hidden="false"></span>
              </button>
              <hr class="my-4">
              <small class="text-muted"><center>Every user who becomes a member accepts the <i>terms</i> agreement.</center></small>
            </form>
          </div>
        </div>
    
      <footer class="d-flex flex-wrap justify-content-between align-items-center py-3 my-4 border-top">
        <p class="col-md-4 mb-0 text-muted">© 2024 </p>
  
    
        <ul class="nav col-md-4 justify-content-end">
          <li class="nav-item"><a href="#" class="nav-link px-2 text-muted">Terms</a></li>
          <li class="nav-item"><a href="#" class="nav-link px-2 text-muted">Policy</a></li>
          <li class="nav-item"><a href="#" class="nav-link px-2 text-muted">Help</a></li>
          <li class="nav-item"><a href="#" class="nav-link px-2 text-muted">Contact</a></li>
        </ul>
      </footer>
      </div>
    </main>

    <div class="toast-container position-fixed top-0 end-0 p-3">
      <!-- This toast will be shown when an error occurs -->
      <div id="liveToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="toast-header">
          <strong class="me-auto">Error</strong>
          <small class="text-body-secondary">Just now</small>
          <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
        </div>
        <div class="toast-body">
          {{ errorToastMessage }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { TOKEN_STORAGE_NAME, POST_LOGIN_ENDPOINT } from '../helpers/constants.js'
import { Toast } from 'bootstrap'

export default {
  name: 'Login',
  data() {
    return {
      // Initial state
      username: 'mor_2314',
      password: '83r5^_',
      errorMessage: '',
      loading: false,
      errorToast: null,
      errorToastMessage: '',
    }
  },
  mounted() {
    // Initial toast setup
    const toastEl = document.getElementById('liveToast')
    if (toastEl) {
      this.errorToast = new Toast(toastEl, { autohide: true })
    }
  },
  methods: {
    // TOAST HANDLING
    showErrorToast(message) {
      this.errorToastMessage = message
      if (this.errorToast) {
        this.errorToast.show()
      }
    },
    // AUTHENTICATION
    async onSignIn () {
      const resetErrorMessage = () => {
        this.errorToastMessage = '';
      };

      // // Validation for empty fields
      if (!this.username || !this.password) {
        this.showErrorToast('Please enter both username and password.')
        return
      };

      if (this.username.length < 3 ) {
        this.showErrorToast('Username must be at least 3 characters long.')
        return
      };

      if (this.password.length < 3) {
        this.showErrorToast('Password must be at least 3 characters long.')
        return
      };
      
      // Reset error message and init loading
      resetErrorMessage();
      this.loading = true;


      // API Call and token handling
      try {
        const response = await axios.post(POST_LOGIN_ENDPOINT, {
          username: this.username,
          password: this.password
        });
        
        const { token } = response.data;

        if (token) {
          localStorage.setItem(TOKEN_STORAGE_NAME, token)
          this.$router.push('/')
        } else {
          this.showErrorToast('Invalid response from server. Please try again.')
        }
        
      } catch (error) {
        console.error(error.response);
        const message = error.response?.data || 'An error occurred. Please try again.'
        this.showErrorToast(message);
      } finally {
        this.loading = false;
      }
    }
  }
}
</script>

<style scoped>
/* CSS RESET */
</style>

<template>
<div>
    <nav v-if="!isLogin" class="sidebar">
        <div class="dismiss">
            <i class="fas fa-chevron-left"></i>
        </div>

        <div class="sidebar-header">
            <img src="../../public/logo.png" alt="logo" style="width:150px;">
        </div>

        <ul class="list-unstyled components">
            <img src="../../public/logo-text2.png" alt="logo" style="width:180px;">
            <li class="active">
                <router-link to="/">Home</router-link>
            </li>
            <li>
                <a href="#">Shop</a>
            </li>
            <li>
                <a href="#">How it works</a>
            </li>
            <li>
                <a href="#">Contact</a>
            </li>
        </ul>

        <ul class="list-unstyled CTAs">
            <li>
                <router-link to="/user/register" class="register">Register</router-link>
            </li>
            <li>
                <router-link to="/user/login" class="login">Login</router-link>
            </li>
        </ul>
    </nav>
    <!-- after login -->
    <nav v-if="isLogin" class="sidebar">
        <div class="dismiss">
            <i class="fas fa-chevron-left"></i>
        </div>

        <div class="sidebar-header">
            <img src="../../public/logo.png" alt="logo" style="width:150px;">
        </div>

        <ul class="list-unstyled components">
            <img src="../../public/logo-text2.png" alt="logo" style="width:180px;">
            <li class="active">
                <router-link to="/">Home</router-link>
            </li>
            <li>
                <a href="#">Shop</a>
            </li>
            <li>
                <a href="#">How it works</a>
            </li>
            <li>
                <router-link to="/user/transaction" class="trans">Transaction</router-link>
            </li>
        </ul>

        <ul class="list-unstyled CTAs">
            <li>
                <a @click.prevent="logout()" class="register" style="color:black; cursor:pointer;">Logout</a>
            </li>
        </ul>
    </nav>
</div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import Swal from 'sweetalert2'
export default {
  name: 'Sidebar',
  methods: {
    ...mapActions([
      'getCart'
    ]),
    logout () {
      let gapi
      if (gapi === undefined) {
        this.getCart()
        const Toast = Swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000
        })
        this.$store.commit('setLogout')
        localStorage.removeItem('token')
        localStorage.removeItem('name')
        localStorage.removeItem('email')
        localStorage.removeItem('setting')
        this.$router.push('/')
        Toast.fire({
          icon: 'success',
          title: 'See you again'
        })
      } else {
        this.getCart()
        var auth2 = gapi.auth2.getAuthInstance()
        auth2.signOut().then(() => {
          const Toast = Swal.mixin({
            toast: true,
            position: 'top-end',
            showConfirmButton: false,
            timer: 3000
          })
          this.$store.commit('setLogout')
          localStorage.removeItem('token')
          localStorage.removeItem('name')
          localStorage.removeItem('email')
          localStorage.removeItem('setting')
          this.$router.push('/')
          Toast.fire({
            icon: 'success',
            title: 'See you again'
          })
        })
      }
    }
  },
  computed: {
    ...mapState([
      'isLogin'
    ])
  },
  mounted () {
    //   $(document).ready(function () {
    $('.sidebar').mCustomScrollbar({
      theme: 'minimal'
    })

    $('.dismiss, .overlay').on('click', function () {
      $('.sidebar').removeClass('active')
      $('.overlay').fadeOut()
    })

    $('.sidebarCollapse').on('click', function () {
      $('.sidebar').addClass('active')
      $('.overlay').fadeIn()
      $('.collapse.in').toggleClass('in')
      $('a[aria-expanded=true]').attr('aria-expanded', 'false')
    })
    //  });
  },
  created () {
    if (localStorage.getItem('token')) {
      this.$store.commit('setLogin')
    } else {
      this.$store.commit('setLogout')
    }
  }
}
</script>

<style scoped>
a {
    color: white;
    text-decoration: none;
}
a:hover {
    color: black !important;
    text-decoration: none;
}
/* ------- */
.sidebar {
    width: 250px;
    position: fixed;
    top: 0;
    left: -250px;
    height: 100vh;
    z-index: 1021 !important;
    background: linear-gradient(#4caf50, #8ac554);
    color: #fff;
    transition: all 0.3s;
    overflow-y: scroll;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.2);
}

.sidebar.active {
    left: 0;
}

.dismiss {
    width: 35px;
    height: 35px;
    line-height: 35px;
    text-align: center;
    background: #4caf50;
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    -webkit-transition: all 0.3s;
    -o-transition: all 0.3s;
    transition: all 0.3s;
}
.dismiss:hover {
    background: #fff;
    color: #4caf50;
}

.overlay {
    position: fixed;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.7);
    z-index: 998;
    display: none;
}

.sidebar .sidebar-header {
    padding: 20px;
    background: #4caf50;
}

.sidebar ul.components {
    padding: 20px 0;
    border-bottom: 1px solid #4caf50;
}

.sidebar ul p {
    color: #fff;
    padding: 10px;
}

.sidebar ul li a {
    padding: 10px;
    font-size: 1.1em;
    display: block;
}
.sidebar ul li a:hover {
    color: #7386D5;
    background: #fff;
}

</style>

<template>
  <div class="container-fluid p-0" id="navbar">
    <div class="container pl-0 pr-0">
    <b-navbar toggleable="md" type="light">
      <b-navbar-brand to="/" class=" d-flex">
        <img src="../assets/icons/Peworld Logo.svg" alt="Logo">
      </b-navbar-brand>

      <b-navbar-toggle target="nav-collapse" class="mr-n3 mr-md-0"
        ><img src="../assets/icons/align-right.png"
      /></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav class="ml-sm-auto">
        <router-link to="/home" class="text-dark font-weight-bold ml-5" v-if="type !== 'home' && isLogin === true">Home</router-link>
        <b-navbar-nav class=" ml-auto" v-if="type === 'home' && isLogin === true">
          <b-navbar-nav class=" mr-xl-auto notif-chat">
            <b-nav-item href="#" class="d-block d-md-none nav-ChatNotif">Chats</b-nav-item>
            <b-nav-item href="#" class="d-block d-md-none nav-ChatNotif"
              >Notifications</b-nav-item
            >
            <b-nav-item href="/notifications" class="d-none mr-2 d-md-flex"
              ><img src="../assets/icons/bell.png"
            /></b-nav-item>
            <b-nav-item href="#" class="d-none mr-2 d-md-flex"
              ><img src="../assets/icons/mail.png"
            /></b-nav-item>
          </b-navbar-nav>
          <b-nav-item-dropdown
            toggle-class=" text-decoration-none"
            no-caret
            right
          >

            <template v-slot:button-content>
              <div class=" borderprofile rounded-circle ">
                <img class="rounded-circle"
                  :src="`${url}/${detailEmploye.image_employe}`" alt="photo profile" v-if="role === '0' && id"/>
                <img class="rounded-circle"
                  :src="`${url}/${detailRecruiter.image_company}`" alt="photo profile" v-if="role === '1'"/>
              </div>
            </template>
            <b-dropdown-item v-if="role === '0'" @click="toProfile">Profile</b-dropdown-item>
            <b-dropdown-item to="/profile-company" v-if="role === '1'">Profile</b-dropdown-item>
            <b-dropdown-item @click="logout">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto" v-else-if="isLogin === true">
          <router-link to="/profile-company" class="btn btnsignup" v-if="role === '1'">
            Profile
          </router-link>
          <button @click="toProfile" class="btn btnsignup" v-else>
            Profile
          </button>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto mr-lg-n3" v-else>
          <button
            type="button"
            class="btn mr-2 btn-outline-signup"
            data-toggle="modal" data-target="#loginfor"
            >Masuk</button
          >
          <button
            data-toggle="modal" data-target="#registerfor"
            type="button"
            class="btn btnsignup"
            >Daftar</button
          >
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <div class="modal" tabindex="-1" id="loginfor">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <h5 class="text-center">
              <img src="../assets/images/logoPurple.png" alt="logo" class="logo" />
            </h5>
            <router-link
            to="/login"
            data-dismiss="modal"
            class="btn btn-fluid btnsignup">
              Masuk sebagai Pekerja
            </router-link>
            <router-link
            to="/loginHire"
            data-dismiss="modal"
            class="btn btn-fluid btn-outline-signup">
              Masuk sebagai Perekrut
            </router-link>
          </div>
        </div>
      </div>
    </div>
    <div class="modal" tabindex="-1" id="registerfor">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <h5 class="text-center">
              <img src="../assets/images/logoPurple.png" alt="logo" class="logo" />
            </h5>
            <router-link
            to="/register"
            data-dismiss="modal"
            class="btn btn-fluid btnsignup">
              Daftar sebagai Pekerja
            </router-link>
            <router-link
            to="/registerHire"
            data-dismiss="modal"
            class="btn btn-fluid btn-outline-signup">
              Daftar sebagai Perekrut
            </router-link>
          </div>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
const { url } = require('../helper/env')

export default {
  props: ['type'],
  data () {
    return {
      id: localStorage.getItem('id'),
      role: localStorage.getItem('role'),
      url: url
    }
  },
  computed: {
    ...mapGetters({
      isLogin: 'auth/isLogin',
      detailEmploye: 'employe/getDetail',
      detailRecruiter: 'recruiter/getDetail'
    })
  },
  methods: {
    ...mapActions({
      onDetail: 'employe/onDetail',
      onDetailRecruiter: 'recruiter/onDetail'
    }),
    logout () {
      localStorage.removeItem('token')
      localStorage.removeItem('refreshtoken')
      localStorage.removeItem('id')
      localStorage.removeItem('role')
      window.location = '/'
    },
    toProfile () {
      this.$router.push({ path: '/profile', query: { id: this.id } })
    }
  },
  mounted () {
    this.onDetail(this.id)
    this.onDetailRecruiter(this.id)
  }
}
</script>

<style scoped>
#navbar {
  background: white;
  box-shadow: 0px 10px 70px rgba(214, 214, 214, 0.25);
}
.bg-lighto {
  background: #fff;
}
.navbar-light .navbar-toggler {
  color: rgba(0, 0, 0, 0.5);
  border: none;
}
/* .navbar {
    padding: 0.5rem 1rem;
    margin: 10px 0px;
} */
.notif-chat {
  width: 80px;
}
.notif-chat img {
  width: 100%;
}
.notif-chat li.nav-item {
  display: flex;
  align-items: center;
}
.borderprofile {
  border: 2px #5E50A1 solid;
  width: 40px;
  height: 40px;
  padding: 2px;
}
.borderprofile img {
  width: 100%;
  height: 100%;
}
.navlink.dropdown-toggle::after {
  display: none;
  margin-left: 0.255em;
  vertical-align: 0.255em;
  content: "";
  border-top: 0.3em solid;
  border-right: 0.3em solid transparent;
  border-bottom: 0;
  border-left: 0.3em solid transparent;
}
.navigation {
  color: #000;
  font-family: 'Open Sans', sans-serif;
  font-weight: 500;
  margin: 0px 10px;
}
.btn {
  width: 100%;
  font-size: 15px;
  font-family: 'Open Sans', sans-serif;
  font-weight: 700;
  margin: 10px 0px;
  border-radius: 10px;
}
.btnsignup {
  background: #5E50A1;
  border-radius: 4px;
  border: none;
  color: white;
}
.btnsignup:hover {
  color: whitesmoke
}
.btn-outline-signup {
  border: 2px #5E50A1 solid;
  border-radius: 4px;
  background: none;
  color: #5E50A1;
}
@media (max-width: 767px) {
  #navbar {
  box-shadow: 0px 10px 70px rgba(214, 214, 214, 0);
}
  .active::after {
    display: none;
  }
  #nav-collapse {
    background-color: white;
    padding: 10px;
    border-radius: 0px 0px 10px 10px;
    border-bottom: 2px #5E50A1 solid;
  }
  .borderprofile {
    border: 2px #5E50A1 solid;
  }
  .nav-ChatNotif {
    color: #5E50A1 !important;
  }
}
</style>

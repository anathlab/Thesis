<template>
<div id="panel">
    <Admin-nav/>
    <div class="grid-container mt-5">
    <div class="row">
    <div class="col-md-2">
        <router-link to="/meals">
        <img src="../../../src/assets/mycollection/png/new/recipe-book.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Dania</p>
        </div>
        </div>
         <div class="col-md-2">
        <router-link to="/ingredients">
        <img src="../../../src/assets/mycollection/png/new/food.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Składniki</p>
        </div>
        </div>
         <div class="col-md-2">
        <router-link to="/categories">
        <img src="../../../src/assets/mycollection/png/new/subfolder.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Kategorie</p>
        </div>
        </div>
        <div class="col-md-2">
        <router-link to="/adminmenu">
        <img src="../../../src/assets/mycollection/png/new/menu-3.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Menu</p>
        </div>
        </div>
        <div class="col-md-2">
        <router-link to="/">
        <img src="../../../src/assets/mycollection/png/new/www.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Strona internetowa</p>
        </div>
        </div>
    <div class="col-md-2">
        <router-link to="/orders">
        <img src="../../../src/assets/mycollection/png/new/delivery-courier.png" class="img-fluid" alt="Responsive image">
        </router-link>
        <div class="text-center">
        <p>Zamówienia</p>
        </div>
    </div>

    <div class="col-md-2">
      <router-link to="/ordersstats">
<img src="../../../src/assets/mycollection/png/new/money.png" class="img-fluid" alt="Responsive image">
      </router-link>
        <div class="text-center">
        <p>Płatności</p>
        </div>
    </div>
    <div class="col-md-2">
        <router-link to="/usersmanagement">
<img src="../../../src/assets/mycollection/png/new/programmer-1.png" class="img-fluid" alt="Responsive image">
        </router-link>
       <div class="text-center">
        <p>Użytkownicy </p>
        </div>
    </div>
    <div class="col-md-2">
        <img @click="logout" src="../../../src/assets/mycollection/png/new/exit-4.png" class="img-fluid" alt="Responsive image">
        <div class="text-center">
        <p>Wyloguj się</p>
        </div>
    </div>   
</div>
</div>
</div>
</template>

<script>
import AdminNav from '../../components/admin/AdminNav.vue'
import {computed} from 'vue';
import {useStore} from "vuex";
import {useRouter} from "vue-router";
export default {
  components: { AdminNav },
    name: 'Panel',
    methods: {
  imgPush() {
    return this.$router.push('/');
  }
  },
  setup() {
    const store = useStore();
    const router = useRouter();
    const auth = computed(() => store.state.authenticated)

    const logout = async () => {
      await fetch('http://localhost:8000/api/logout', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        credentials: 'include',
      });
      localStorage.setItem('jwt', null)
      localStorage.setItem('role', null)
      await router.push('/');
    }
    return {
      auth,
      logout
    }
  }
}
</script>

<style>

.row{
 display: flex;
 justify-content: center;

}
.col-md-2 {
    font-family: 'DM Sans', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border-style: solid;
    border-width: 2px;
    border-color: #222222;
    flex: 0 0 33.333333%;
    border-radius: 14px;
    margin: 5vw;
    color: black;
}

.col-md-2 img{
    padding: 1vw;
    margin: 20px;
}
</style>
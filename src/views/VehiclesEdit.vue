<template>
  <div id="" class="h-full flex flex-col dark:text-white w-96 mx-auto max-w-sm">
    <div class="logo pt-6">
      <h1>
        <img class="mx-auto" src="@/assets/logos/covoit.svg" alt="">
      </h1>
    </div>
    <div class="grow flex flex-col justify-between gap-4 pb-20">
      <form class="flex flex-col gap-4 ">
        <h2 class="text-2xl font-bold m-6">Informations du véhicule :</h2>
        <div class="flex relative">
          <p class="grow text-1xl text-left">{{ VehiclesForUser.vehicle_name }}</p>
          <button>
            <img src="@/assets/logos/Pen.svg" class="w-8 h-8 mx-1" />
          </button>
          <span class="bg-gray-900 h-0.5 w-full absolute bottom-0 rounded-full"></span>
        </div>
        <div class="flex relative">
          <p class="grow text-1xl text-left">{{ VehiclesForUser.color }}</p>
          <button>
            <img src="@/assets/logos/Pen.svg" class="w-8 h-8 mx-1" />
          </button>
          <span class="bg-gray-900 h-0.5 w-full absolute bottom-0 rounded-full"></span>
        </div>
        <div class="flex relative">
          <p class="grow text-1xl text-left">{{ VehiclesForUser.motorization }}</p>
          <button>
            <img src="@/assets/logos/Pen.svg" class="w-8 h-8 mx-1" />
          </button>
          <span class="bg-gray-900 h-0.5 w-full absolute bottom-0 rounded-full"></span>
        </div>
        <div class="flex relative">
          <p v-show="VehiclesForUser.nb_places > 1" class="grow text-1xl text-left">{{ VehiclesForUser.nb_places }} places</p>
          <p v-show="VehiclesForUser.nb_places == 1" class="grow text-1xl text-left">{{ VehiclesForUser.nb_places }} place</p>
          <button>
            <img src="@/assets/logos/Pen.svg" class="w-8 h-8 mx-1" />
          </button>
          <span class="bg-gray-900 h-0.5 w-full absolute bottom-0 rounded-full"></span>
        </div>
      </form>
      <div class="grid gap-4">
        <button @click="deleteVehicleForUser" class="drop-shadow-[0_2px_4px_rgba(0,0,0,0.2)] bg-red-600 p-2 rounded-full text-white font-bold dark:shadow-[0_-5px_15px_-8px_rgba(255,0,0,0.5)]">Supprimer</button>
        <button class="drop-shadow-[0_2px_4px_rgba(0,0,0,0.2)] bg-neutral-800 p-2 rounded-full text-white font-bold dark:shadow-[0_-5px_15px_-8px_rgba(255,255,255,0.5)]">Sauvegarder</button>
        <button @click="$router.go(-1)" class="drop-shadow-[0_2px_4px_rgba(0,0,0,0.2)] bg-white p-2 rounded-full text-neutral-900 font-bold dark:shadow-[0_-5px_15px_-8px_rgba(255,255,255,0.5)]">Retour</button>
      </div>
    </div>
  </div>
  <navbar/>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export default {
  components: { Navbar },
  data() {
    return {
      vehicleRowId: this.$route.params.id,
      VehiclesForUser: '',
    };
  },
  computed: {
    VehicleColor() {
      return this.$store.state.VehicleColor;
    },
  },
  methods: {
    fetchVehiclesForUser() {
      axios.post('https://covoit.erwan-decoster.com/actions.php', {
        action: 'fetch_vehicle_for_user',
        tel: '0625306813',
        vehicleRowId: this.vehicleRowId,
      }).then((response) => {
        this.VehiclesForUser = response.data;
        console.log(response.data);
      });
    },
    deleteVehicleForUser() {
      axios.post('https://covoit.erwan-decoster.com/actions.php', {
        action: 'delete_vehicle_for_user',
        vehicleId: this.VehiclesForUser.id_vehicles,
        tel: '0625306813',
      }).then((response) => {
        console.log(response.data);
        this.$router.push({ path: '/Vehicles' });
      });
    },
  },
  mounted() {
    this.fetchVehiclesForUser();
    // Permet de ne pas aller sur la page si on est pas connecter
    if (!localStorage.tel) {
      this.$router.push({ path: '/' });
    }
  },
};
</script>

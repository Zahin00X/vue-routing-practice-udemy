<template>
<button @click="confirmInput">Confirm</button>
<button @click="saveChanges">Save Changes</button>
  <ul>
    <user-item v-for="user in users" :key="user.id" :name="user.fullName" :role="user.role"></user-item>
  </ul>
</template>

<script>
import UserItem from '../components/users/UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  data()
  {
    return { changesSaved: false};
  },
  methods:{
  confirmInput()
  {
    this.$router.push('/teams');
  },
  saveChanges()
  {
    this.changesSaved= true;
  }
  },
  beforeRouteLeave(to, from, next)
  {
    console.log('UserList component before route!');
    console.log(to, from);
    if(this.changesSaved)
    {
      next();
    }
    else
    {
      const userWantsToLeave = confirm('Are you sure you want to leave? You have unsaved changes!');
      next(userWantsToLeave);
    }

  },
  beforeRouteEnter(to, from , next)
  {
    console.log("User's List Component beforeRoute Enter");
    console.log(to, from);
    next();
  },
  unmounted()
  {
    console.log('unmounted');
  }

};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 20rem;
  padding: 0;
}
</style>
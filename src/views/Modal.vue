<template>
  <div class="modal-open" @click="close($event)">
    <div class="modal fade show d-block" id="exampleModal"
         tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document" @click="$event.stopPropagation();">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">{{$route.params.type}}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"
                    @click="close($event)">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <table>
              <tr v-for="user in users" v-bind:key="user.id">
                <td>{{user.name}}</td>
                <td>{{user.username}}</td>
                <td><button class="btn btn-primary" @click="open(user)">Open</button></td>
              </tr>
            </table>
          </div>
          <div class="modal-footer">
            <router-link :to="{ name: 'Modals', params: { type: 'all' } }">
              All Users
            </router-link>
            <router-link :to="{ name: 'Modals', params: { type: 'even' } }">
              Even Users
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
  <ChildList v-if="selectedUser" v-model:selectedUser="selectedUser"></ChildList>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import router from '@/router';
import axios from 'axios';
import ChildList from '@/components/ChildList.vue';

@Options({
  props: {
  },
  components: {
    ChildList,
  },
  watch: {
    $route: (previous, next) => {
      console.log(previous);
      console.log(next);
    },
  },
})
export default class Modal extends Vue {
  users = [];

  selectedUser = null;

  mounted() {
    axios.get('https://jsonplaceholder.typicode.com/users')
      .then((response) => {
        const { data } = response;
        if (this.$route.params.type === 'all') {
          this.users = data;
        } else {
          this.users = data.filter((user: any) => user.id % 2);
        }
      });
  }

  open(user: any) {
    this.selectedUser = user;
  }

  close($event: Event) {
    router.push('/');
  }
}
</script>

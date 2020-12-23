<template>
  <div @click="close($event)">
    <div class="modal fade show d-block" id="exampleModal" style="z-index: 1051"
         tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document" @click="$event.stopPropagation();">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel" @click="selectedUser.name = 'samet'">
              Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"
                    @click="close($event)">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            {{ selectedUser.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="modal-backdrop fade show" style="z-index: 1050"></div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';

@Options({
  props: {
    selectedUser: Object,
  },
})
export default class ChildList extends Vue {
  selectedUser: any;

  mounted() {
    console.log(this.selectedUser);
  }

  close($event: Event) {
    this.$emit('update:selectedUser', null);
  }
}
</script>

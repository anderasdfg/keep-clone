<template>
  <div id="app">
    <div class="logos">
      <span class="logo keep">
        <img src="https://media4.giphy.com/media/eerYQwIuHp36dV5Umk/giphy.gif" alt="Notes">
      </span>      
    </div>

    <CreateNoteForm /> 

    <Notes />

     <transition name="modal">
      <UpdateModal v-if="showModal" :note="selectedNote" />
    </transition> 
  </div>
</template>

<script>
import Notes from '@/components/Notes';
import { EventBus } from '@/EventBus.js';
import CreateNoteForm from '@/components/Create';
import UpdateModal from '@/components/UpdateModal';
import { autoExpand } from '@/utils';

export default {
  name: 'App',
  components: {
     Notes,
     CreateNoteForm,
     UpdateModal
  },
  data() {
    return {
      selectedNote: null,
      showModal: false,
    };
  },
   created() {
    EventBus.$on('note-selected', note => {
      this.selectedNote = note;
      this.showModal = true;
      document.body.classList.add('freeze');
    });

    EventBus.$on('modal-dismissed', () => {
      this.selectedNote = null;
      this.showModal = false;
      document.body.classList.remove('freeze');
    });
  },
 mounted() {  
   console.log("hola");
   console.log(process.env);  
    document.addEventListener('input', event => {
      if (event.target.tagName.toLowerCase() !== 'textarea') {
        return;
      }
      autoExpand(event.target);
    }, false);
  },
}
</script>


<style lang="scss">
@import './styles/main.scss';

.logos {
  @include flex-center;
  margin: 0 auto 30px;

  & > * {
    vertical-align: middle;
  }

  span {
    color: $vue-navy;
    font-family: $ff-cute;
    font-weight: 700;
    font-size: $fz-lg;
  }

  .logo {
    width: 120px;

    &.vue {
      width: 40px;
    }
    &.firebase {
      width: 30px;
      margin: 0 8px;
    }
  }

  .plus {
    margin: 0 10px;
    font-weight: 400;
  }
}
</style>

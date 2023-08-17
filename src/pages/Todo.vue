<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">

      <q-input @keyup.enter="addTask" filled class="col" square placeholder="add some task" bg-color="white"
        v-model="newTask" label="Label" dense>


        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>

    </div>

    <q-list separator bordered>

      <q-item @click="task.done = !task.done" clickable :class="{ 'done bg-green-2': task.done }"
        v-for="(task, index) in tasks" :key="task.title" v-ripple>
        <q-item-section avatar>
          <q-checkbox class="no-pointer-events" v-model="task.done" val="teal" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" dense flat round color="primary" icon="delete" />
        </q-item-section>
      </q-item>

    </q-list>


  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [
        {
          title: 'Hello',
          done: true
        },
        {
          title: 'Hello 2',
          done: false
        },
        {
          title: 'Hello 3',
          done: false
        }
      ]
    }
  },
  methods: {
    deleteTask(index) {

      this.$q.dialog({
        title: 'Confirm',
        message: 'do u wanna realy delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify('delete!')
      }).onOk(() => {
        // console.log('>>>> second OK catcher')
      }).onCancel(() => {
        // console.log('>>>> Cancel')
      }).onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      })


    },
    addTask() {
      if (this.newTask != '') {
         this.tasks.push({ title: this.newTask, done: false });
          this.newTask = '';
         } else{
         this.$q.notify('input is empty!')}
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: grey;
  }
}
</style>
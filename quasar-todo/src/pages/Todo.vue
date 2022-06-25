<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
    
      <q-input 
      filled v-model="newTask" 
      @keyup.enter="addTask"
      label="Add task" 
      :dense="dense" 
      bg-color="white" 
      square class="col">

        <template v-slot:before>
          <q-avatar>
            <img src="logo.png">
          </q-avatar>
        </template>

        <template v-slot:append>
          <q-icon v-if="text !== ''" name="close" @click="text = ''" class="cursor-pointer" />
        </template>

        <template v-slot:hint>
          Add task
        </template>

        <template v-slot:after>
          <q-btn @click="addTask" round dense flat icon="send" />
        </template>
      </q-input>
      
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
     <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary">

      </q-icon>
    <div  class="text-h6 text-primary text-center">
      No tasks
    </div>
  </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",

  data() {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: "Get bananas",
        //   done: false,
        // },
        // {
        //   title: "Eat bananas",
        //   done: true,
        // },
        // {
        //   title: "Poo bananas",
        //   done: false,
        // },
      ],
    };
  },

  methods: {
    deleteTask(index) {
        this.$q.dialog({
          title: "Confirm",
          message: "Rly delete?",
          cancel: true,
          persistent: true,
        }).onOk(() => {
          this.tasks.splice(index, 1);
            this.$q.notify('Task deleted')
        });

    
    },

    addTask(){
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
  .no-tasks{
    opacity: 0.5;
  }
</style>

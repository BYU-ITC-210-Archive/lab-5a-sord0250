<template>
 
    <div id='task-list'>
      <v-list>
        <!-- TODO: Add a v-for attribute on this tag -->
        <!-- Remember to use the :key attribute to define a key for each task! -->
        <v-list-item v-for="task in tasks" :key="task._id">
  
            <template v-slot:prepend>
              <!-- TODO: Add a click event handler attribute here that updates an task -->
              <v-btn variant="text" @click="clickHandler(task)">
              <!-- TODO: Add a v-if and v-else check here based on the current task's Done property -->
                <v-icon v-if="task.done" icon="mdi-checkbox-marked" ></v-icon>
                <v-icon v-else icon="mdi-checkbox-blank-outline"  ></v-icon>
              </v-btn>
            </template>
  
            <v-list-item-title>
              <!-- TODO: Show the value of the task's Text property here -->
               {{task.text}}
            
  
            </v-list-item-title>
            <v-list-item-subtitle>
              <!-- TODO: Show the value of the task's Date property here -->
               {{task.date}}
            
  
            </v-list-item-subtitle>
            <template v-slot:append>
              <!-- TODO: Add a click event handler attribute here that deletes this task -->
              <v-btn @click="deleteHandler(task)" icon="mdi-trash-can" color="red" variant="text">
              </v-btn>
            </template>
  
        </v-list-item>
      </v-list>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TaskList',
    props: {
      // TODO: Figure out what you need to bring in from props, then and then add those variables here
      // Make sure to include these props on the component's tag when using it in another file
      // HINT: For the tasks prop, set the default value to:
      tasks: {
        type: Array,
        required: true,
        default: () => []
      },
      clickHandler: {
        type: Function,
        required: true
      },
      deleteHandler: { 
        type: Function,
        required: true
      },
      user: {
        type: Object,
        required: false
      },
    },
    
    
  }
  </script>
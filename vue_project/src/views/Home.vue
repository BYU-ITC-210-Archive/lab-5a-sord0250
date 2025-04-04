<template>
    <div class="home">
      <v-card class="mx-auto" max-width="900" v-if="fetched">
  
        <!-- TODO: Add your NewTaskForm component -->
        <!-- Make sure to pass in any necessary props -->
        <NewTaskForm :submitHandler="createTask" :user="user" :form="form" :dateHandler="dateHandler"></NewTaskForm>
  
        <v-divider></v-divider>
        <v-list subheader two-line flat>
          <v-list-subheader>
  
            <!-- TODO: use the `user` prop to display the user's username here -->{{user?.username || "User"}}'s Tasks:
  
          </v-list-subheader>
  
          <!-- TODO: Add your TaskList component -->
          <!-- Make sure to pass in any necessary props -->
           <TaskList :clickHandler="updateTask" :deleteHandler="deleteTask" :user="user" :tasks="tasks"></TaskList>
  
        </v-list>
      </v-card>
    </div>
  </template>
  
  <script>
  import { getCurrentDate, formatDate } from '@/util'
  // TODO: Import the components you want to use from their files
  import AppBar from "@/components/AppBar.vue";
  import NewTaskForm from "@/components/NewTaskForm.vue";
  import TaskList from "@/components/TaskList.vue";

  export default {
    name: 'Home',
    components: {
      // TODO: Use the Vue Documentation to find out how to use this property
      AppBar,
      NewTaskForm,
      TaskList
    },
    props: {
      // TODO: Add the user object as a prop that's passed in from the App.vue component
      user: Object,
    },
    data: () => ({
      fetched: false, // This keeps us from getting an error when the page loads, but there's no data
      tasks: [], // This will hold the list of tasks you get from your API
      form: {
        // TODO: Add 2 properties to this form data object to track the Text and the Date
        // HINT: Capitalize the "Text" and "Date" properties to make it easier to pass the data to your API
        // HINT: You can use the `getCurrentDate()` method to get today's date in the proper format for the date picker
        text: '',
        date: getCurrentDate(),
      },
    }),
    mounted() {
      // TODO: Call the method that gets the tasks from your API
      this.readTasks();
    },
    methods: {
      myClickHandler() {
        console.log("Task clicked! Implement the function.");
      },
      dateHandler(callback) {
        if (callback === 'getCurrentDate') {
          // Logic to handle getting the current date
          this.form.date = getCurrentDate();
        } else if (callback === 'formatDate') {
          // Logic for formatting the date
          this.form.date = formatDate(this.form.date);
        }
      },
      createTask(form) {
        // TODO: Use fetch() to send a POST request to your API that includes the data from this.form
        // TODO: Remember to get the updated task list when it's done
        // TODO: Remember to reset the values in this.form to their initial values when it's done
        fetch(
          `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks`,
          // The second parameter is an object with options. You can include request
          // headers here, options for credentials, which method, which mode, etc.
          {
            method: `POST`,
            credentials: `include`,
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              text: form.text,
              date: form.date,
              userId: this.user.id,
            }),
          }
          // Note: The default for method is GET, so you don't need to include the
          // method on any GET requests.
        ).then(response => {
          // Here we're just checking if the response was successful or not before
          // trying to do anything about it.
          if (!response.ok) {
            return response.json().then(error => {
              console.error('Error from server:', error);
            });
          }
          if (response.ok) {
            // If it is successful, we want to update the task list.
            this.readTasks()
            this.form.text = '';
            this.form.date = getCurrentDate();
          }
        })
      },
      readTasks() {
        // TODO: Use fetch() to send a GET request to your API and update this.fetched and this.tasks with the data that's returned
        fetch(
          `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks`,
          // The second parameter is an object with options. You can include request
          // headers here, options for credentials, which method, which mode, etc.
          {
            credentials: `include`,
            headers: {
              "Content-Type": "application/json",
            },
          }
          // Note: The default for method is GET, so you don't need to include the
          // method on any GET requests.
        ).then(response => response.json())
          .then(data => {
            this.tasks = data;
            this.fetched = true;
          })
      },
      updateTask(task) {
        // TODO: Use fetch() to send a PUT request to your API to update an task to be Done/not Done.
        // TODO: Remember that the task's ID should be included in the path of the request, i.e. http://yourserverurl/api/v1/tasks/2r984hfiwufw948feoi
        // TODO: Remember to get the updated task list when it's done
        fetch(
          `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`,
          // The second parameter is an object with options. You can include request
          // headers here, options for credentials, which method, which mode, etc.
          {
            method: `PUT`,
            credentials: `include`,
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              done: !task.done,
            }),
          }
          // Note: The default for method is GET, so you don't need to include the
          // method on any GET requests.
        ).then(response => {
          // Here we're just checking if the response was successful or not before
          // trying to do anything about it.
          if (response.ok) {
            // If it is successful, we want to update the task list.
            this.readTasks()
          }
        })
      },
      // This method is given to you. Use it to see how to make fetch() requests.
      deleteTask(task) {
        fetch(
          // The first parameter is a string that contains the full URL to your endpoint
          `${process.env.VUE_APP_API_ORIGIN}/api/v1/tasks/${task._id}`,
          // The second parameter is an object with options. You can include request
          // headers here, options for credentials, which method, which mode, etc.
          {
            method: `DELETE`,
            credentials: `include`,
          }
          // Note: The default for method is GET, so you don't need to include the
          // method on any GET requests.
        ).then(response => {
          // Here we're just checking if the response was successful or not before
          // trying to do anything about it.
          if (response.ok) {
            // If it is successful, we want to update the task list.
            this.readTasks()
          }
        })
      }
    }
  }
  </script>
  
  <style scoped>
  .form {
    padding: 0 1rem;
  }
  </style>
  
  
  
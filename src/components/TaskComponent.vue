<template>
  <div>
    <div>
      <h1 class="display-4 text-center">List of tasks</h1>
      <hr />
      <div class="row">
        <div class="col-lg-8 offset-lg-2">
          <div class="card mt-4">
            <div class="card-body">
              <div class="input-group">
                <input
                  type="text"
                  v-model="task.name"
                  class="form-control form-control-lg"
                  placeholder="new task "
                />
                <div class="input-group-append">
                  <button
                    v-on:click="addTask(task)"
                    class="btn btn-success btn-lg"
                  >
                    Add
                  </button>
                </div>
              </div>
              <br />
              <div class="text-center">
                <div
                  v-if="loading"
                  class="spinner-border text-success"
                  role="status"
                ></div>
              </div>

              <h5 v-if="listtasks.length == 0">You dont have any task</h5>
              <ul v-if="!loading" class="list-group">
                <li
                  v-for="(task, index) of listtasks"
                  :key="index"
                  class="list-group-item d-flex justify-content-between"
                >
                  <button
                    type="button"
                    class="btn btn-primary"
                    data-bs-toggle="modal"
                    :data-bs-target="'#staticBackdrop' + task.id"
                  >
                    update
                  </button>
                  {{ task.name }}
                  <button
                    class="btn btn-danger"
                    v-on:click="deleteTask(task.id)"
                  >
                    Delete
                  </button>

                  <!-- start modal -->
                  <div
                    class="modal fade"
                    :id="'staticBackdrop' + task.id"
                    data-bs-backdrop="static"
                    data-bs-keyboard="false"
                    tabindex="-1"
                    aria-labelledby="staticBackdropLabel"
                    aria-hidden="true"
                  >
                    <div class="modal-dialog">
                      <div class="modal-content">
                        <div class="modal-body">
                          <form id="taskform">
                            <div class="mb-3">
                              <label for="exampleInputEmail1" class="form-label"
                                >Task</label
                              >
                              <input
                                class="form-control"
                                id="exampleInputEmail1"
                                v-model="task.name"
                              />
                              <div id="emailHelp" class="form-text">
                                Change this task
                              </div>
                            </div>
                          </form>
                        </div>
                        <div class="modal-footer">
                          <button
                            type="button"
                            class="btn btn-secondary"
                            data-bs-dismiss="modal"
                          >
                            Close
                          </button>
                          <button
                            type="submit"
                            v-bind:class="{ 'text-success': task.status }"
                            v-on:click="updateTask(task, task.id)"
                            class="btn btn-primary"
                            data-bs-dismiss="modal"
                          >
                            save
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                  <!-- end modal -->
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent, onMounted, ref } from "vue";

interface Task {
  id: number;
  name: string;
  status: boolean;
}

export default defineComponent({
  name: "TaskComponent",
  setup() {

    const task = {
      name: "" as string,
      status: false as boolean,
    };

    const tasks = ref([]);
    const loading = ref(false);

    function getTasks(): void {
      loading.value = true;
      axios
        .get("http://dev.taskapi.com/api/Task")
        .then((response) => {
          //console.log(response);
          tasks.value = response.data;
          loading.value = false;
        })
        .catch(() => (loading.value = false));
    }

    function addTask(task: Task): void {
      let newTask = {
        name: task.name,
        status: task.status,
      };
      loading.value = true;
      axios
        .post("http://dev.taskapi.com/api/Task/", newTask)
        .then((response) => {
          console.log(response);
          getTasks();
          loading.value = false;
        })
        .catch((error) => {
          console.error(error);
          loading.value = false;
        });
      task.name = "";
    }

    function deleteTask(id: number): void {
      loading.value = true;
      axios
        .delete("http://dev.taskapi.com/api/Task/" + id)
        .then((response) => {
          console.log(response);
          getTasks();
          loading.value = false;
        })
        .catch((error) => {
          console.log(error);
          loading.value = false;
        });
    }

    function updateTask(task: Task, id: number): void {
      loading.value = true;
      axios
        .put("http://dev.taskapi.com//api/Task/" + id, task)
        .then(() => {
          getTasks();
          loading.value = false;
        })
        .catch(() => (loading.value = false));
    }


    onMounted(getTasks);


    return {
      listtasks: tasks,
      loading: loading,
      addTask: addTask,
      deleteTask: deleteTask,
      updateTask: updateTask,
      task: task,
    };
  },
});
</script>

<style>
</style>
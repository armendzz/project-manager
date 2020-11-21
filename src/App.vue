<template>
  <div id="app">
    <div class="container">
      <div class="card mt-2">
        <div class="card-header">Project Manager</div>
        <div class="row">
          <div v-for="job in jobs" :key="job.id" class="col-lg-6">
            <div class="card-body">
              <div class="card text-info mb-3">
                <div class="card-body">
                  <h5 class="card-title">{{ job.name }}</h5> 
                  <div
                    v-for="task in job.task"
                    :key="task.id"
                    class="form-check"
                  >
                    <input
                      class="form-check-input"
                      type="checkbox"
                      value=""
                      :checked="task.completed"
                      @click="togleTask(!task.completed, task.id)"
                    />
                    <label class="form-check-label" for="defaultCheck1">
                      {{ task.title }}
                    </label>
                  </div>
                </div>
                <div class="card-footer bg-transparent border-info">
                  <div class="form-inline">
                    <div class="form-group mx-sm-3 mb-2">
                      <label for="addtask" class="sr-only">Name</label>
                      <input
                        type="text"
                        class="form-control form-control-sm"
                        placeholder="title"
                        v-model="job.newTitle"
                      />
                    </div>
                    <button
                      type="submit"
                      class="btn ml-2 btn-sm btn-primary mb-2"
                      @click="addTask(job.newTitle, job.id)"
                    >
                      add Task
                    </button>
                    <button
                      type="submit"
                      class="btn btn-sm ml-2 btn-danger mb-2"
                      @click="deleteJob(job.id)"
                    >
                      Delete Job
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="card-footer">
          <div class="form-inline">
            <div class="form-group mx-sm-3 mb-2">
              <label for="inputPassword2" class="sr-only">Name</label>
              <input
                type="text"
                class="form-control"
                v-model="jobName"
                placeholder="title"
              />
            </div>
            <button type="submit" class="btn btn-success mb-2" @click="addJob">
              add Job 
            </button>
            
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      jobs: [],
      taskTitle: "",
      jobName: "",
    };
  },
  mounted() {
    this.getAll();
  },
  methods: {
    addTask(jobNewTitle, jobId) {
      axios
        .post("http://localhost:8000/api/task", {
          title: jobNewTitle,
          jobs_id: jobId,
        })
        .then(() => {
          this.getAll()
        })
        .catch(function(error) {
          console.log(error);
        });
     
        
    },
      togleTask(checked, taskId) {
         axios
        .put("http://localhost:8000/api/task/" + taskId, {
         completed: checked
        })
        .then(() => {
          this.getAll()
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    deleteJob(id){
       axios
        .delete("http://localhost:8000/api/jobs/" + id)
        .then(() => {
          this.getAll()
        })
        .catch(function(error) {
          console.log(error);
        });
    },
     addJob() {
      axios
        .post("http://localhost:8000/api/jobs", {
          name: this.jobName,
        })
        .then(() => {
          this.getAll()
        })
        .catch(function(error) {
          console.log(error);
        });
        this.jobName = '';
    },
    getAll() {
      axios.get("http://localhost:8000/api/jobs").then((response) => {
        this.jobs = response.data.data;
      });
    },
  },
};
</script>

<style></style>

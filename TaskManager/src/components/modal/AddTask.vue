<script lang="ts" setup>
import { defineProps, ref , watch } from "vue";
const props = defineProps({
  activeModal: Boolean,
  title: String,
  editModal : Object
});

const taskName = ref("");
const status = ref("");
const priority = ref("");
const isTaskRequired = ref(false);
const isStatus = ref(false);
const taskRequired = ref(false);
const taskId = ref(null);
const isEditTask = ref(true)
// add data function
const emit = defineEmits(["closeModal", "sendingData"]);
watch(()=> props.editModal, (newValue)=>{
    if (newValue) {
      console.log('newValue: ', newValue);
      taskId.value = newValue.id;
      taskName.value = newValue.taskTitle;
      priority.value = newValue.prioritys;
      status.value = newValue.status
    }
  })

function addData() {
  if (taskName.value == "" && status.value == "" && priority.value == "") {
    isTaskRequired.value = true;
    isStatus.value = true;
    taskRequired.value = true;
  } else {
    const myItem = {
      id : taskId.value,
      task: taskName.value,
      status: status.value,
      prioritys: priority.value,
    };
    emit("sendingData", myItem);
  }
}
// check required
function radioSelected() {
  if (priority.value) {
    isTaskRequired.value = false;
  } else {
    isTaskRequired.value = false;
  }
}
// check required
function statusSelect() {
  if (status.value) {
    isStatus.value = false;
  } else {
    isStatus.value = false;
  }
}
// check required
function taskNameRequired() {
  if (taskName.value) {
    taskRequired.value = false;
  } else {
    taskRequired.value = true;
  }
}
</script>

<template>
  <div class="text-center pa-4">
    <v-dialog v-model="props.activeModal" max-width="800">
      <v-card :title="props.title" class="px-2">
        <div class="user_listing">
          <ul>
            <li>
              <label for="addtask">Task Name </label>
              <input
                type="text"
                id="addtask"
                v-model="taskName"
                @input="taskNameRequired"
              />
              <p class="error" v-if="taskRequired">fields are required*</p>
            </li>
            <li>
              <label for="status">Status</label>
              <select id="status" v-model="status" @input="statusSelect">
                <option value="Done" v-if="props.title == 'Edit Task'">Done</option>
                <option value="In progress">In Progress</option>
                <option value="In Planed">In Planed</option>
              </select>
              <p class="error" v-if="isStatus">fields are required*</p>
            </li>
            <li>
              <div class="selectRadio">
                <label for="high">High</label>
                <input
                  type="radio"
                  v-model="priority"
                  value="high"
                  id="high"
                  @input="radioSelected"
                />
              </div>
              <div class="selectRadio">
                <label for="medium">Medium</label>
                <input
                  type="radio"
                  v-model="priority"
                  value="medium"
                  id="medium"
                  @input="radioSelected"
                />
              </div>
              <div class="selectRadio">
                <label for="low">Low</label>
                <input
                  type="radio"
                  v-model="priority"
                  value="low"
                  id="low"
                  @input="radioSelected"
                />
              </div>
              <p class="error" v-if="isTaskRequired">fields are required*</p>
            </li>
          </ul>
        </div>
        <template v-slot:actions>
          <v-btn @click="addData">{{props.title == "Edit Task" ? 'Update' : 'Create'}}</v-btn>
          <v-btn text="Cancel" @click="$emit('closeModal')"></v-btn>
        </template>
      </v-card>
    </v-dialog>
  </div>
</template>

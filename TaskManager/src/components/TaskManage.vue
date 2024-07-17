<script lang="ts" setup>
import { reactive, ref } from "vue";
import AddTask from "../components/modal/AddTask.vue";
const isModalActive = ref(false);
const ketUpdate = ref(0);
const task = ref([
  {
    id: 1,
    taskTitle: "Lorem lorem lorem",
    status: "In progress",
    prioritys: "high",
  },
]);
const editModal = ref(false);
const editData = ref();
// open modal
function openModal() {
  isModalActive.value = true;
  ketUpdate.value++;
  console.log(isModalActive.value, "modal open");
}
// close modal
function closeTaskModal() {
  isModalActive.value = false;
}
// show data
function sendingData(myItem) {
  if (!myItem.id) {
    const taskid = task.value.length + 1;
    console.log(taskid);
    task.value.push({
      id: taskid,
      taskTitle: myItem.task,
      status: myItem.status,
      prioritys: myItem.prioritys,
    });
  }
  if (myItem.id) {
    const myIndex = task.value.findIndex((newid) => newid.id == myItem.id);
    (task.value[myIndex].id = myItem.id),
      (task.value[myIndex].taskTitle = myItem.task),
      (task.value[myIndex].status = myItem.status),
      (task.value[myIndex].prioritys = myItem.prioritys);
  }

  isModalActive.value = false;
}

function editText(taskList) {
  isModalActive.value = true;
  editModal.value = true;
  editData.value = taskList;
}
function deleteTask(remove){
    const myIndex = task.value.findIndex((newid) => newid.id == remove);
    task.value.splice(myIndex,1)
}
</script>

<template>
  <div class="card">
    <h1>Task Manager App</h1>
    <div class="flex_section">
      <button class="addTask" @click="openModal()">Add Task</button>
      <table v-if="task.length > 0">
        <tr>
          <th>Task</th>
          <th>Status</th>
          <th>Priority</th>
          <th>Action</th>
        </tr>

        <tr v-for="alltask in task" :key="alltask">
          <td>{{ alltask.taskTitle }}</td>
          <td>{{ alltask.status }}</td>
          <td v-if="alltask.prioritys == 'high'">
            <div class="high"></div>
          </td>
          <td v-if="alltask.prioritys == 'medium'">
            <div class="medium"></div>
          </td>
          <td v-if="alltask.prioritys == 'low'"><div class="low"></div></td>
          <td class="action_td">
            <button class="edit" @click="() => editText(alltask)">
              <img
                src="../assets/image/edit.png"
                alt="edit"
                height="20"
                width="20"
              />
            </button>
            <button class="delete" @click="deleteTask(alltask.id)">
              <img
                src="../assets/image/delete.png"
                alt="delete"
                height="20"
                width="20"
              />
            </button>
          </td>
        </tr>
      </table>
      <p v-else class="noData">No Data available</p>
    </div>
    <AddTask
      :activeModal="isModalActive"
      :key="ketUpdate"
      @closeModal="closeTaskModal"
      :title="editModal ? 'Edit Task' : 'Add Task'"
      @sendingData="sendingData"
      :editModal="editData"
    />
  </div>
</template>

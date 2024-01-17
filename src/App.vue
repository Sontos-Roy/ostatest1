<script setup>
import { ref } from 'vue';

const tasks = ref([
  { name: 'Task 1', time: 30 },
  { name: 'Task 2', time: 40 },
  { name: 'Task 3', time: 60 },
  { name: 'Task 4', time: 45 },
  { name: 'Task 5', time: 50 },
]);

const showPopup = ref(false);
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(-1);

const openEditPopup = (index) => {
  editedTask.value = { ...tasks.value[index] };
  editedTaskIndex.value = index;
  showPopup.value = true;
};

const closeEditPopup = () => {
  showPopup.value = false;
  editedTask.value = { name: '', time: 0 };
  editedTaskIndex.value = -1;
};

const updateTask = () => {
  if (editedTaskIndex.value !== -1) {
    tasks.value[editedTaskIndex.value] = { ...editedTask.value };
    closeEditPopup();
  }
};
</script>

<template>
  <div class="max-w-lg mx-auto my-10 bg-white p-8 rounded-xl shadow shadow-slate-300">
        <div class="flex flex-row justify-between items-center">
            <div>
                <h1 class="text-3xl font-medium text-black">Tasks list</h1>
            </div>
        </div>
        <p class="text-slate-500">Hello, here are your latest tasks</p>

        <div id="tasks" class="my-5">
            <div id="task" v-for="(task, index) in tasks" :key="index" class="flex justify-between items-center border-b border-slate-200 py-3 px-2 border-l-4  border-l-transparent bg-gradient-to-r from-transparent to-transparent hover:from-slate-100 transition ease-linear duration-150">
                <div class="inline-flex items-center space-x-2 text-black">
                    <div>
                        <i class="fa fa-check-circle"></i>
                                                 
                    </div>
                    <div class="">{{ task.name }} (Time : {{ task.time }})</div>
                </div>
                <div @click="openEditPopup(index)" class="bg-blue-100 hover:bg-blue-300 text-white font-bold py-2 px-4 rounded">
                    <i class="fa fa-edit text-black"></i>                    
                </div>
            </div>
        </div>
    </div>
    <teleport to="body">
    <div class="main-modal fixed w-full h-100 inset-0 z-50 overflow-hidden flex justify-center items-center animated fadeIn faster" v-if="showPopup"
      style="background: rgba(0,0,0,.7);">
      <div
        class="border border-teal-500 shadow-lg modal-container bg-white w-11/12 md:max-w-md mx-auto rounded shadow-lg z-50 overflow-y-auto">
        <div class="modal-content py-4 text-left px-6">
          <!--Title-->
          <div class="flex justify-between items-center pb-3">
            <p class="text-2xl font-bold text-black">Edit Task</p>
            <div class="modal-close cursor-pointer z-50"  @click="closeEditPopup">
              <svg class="fill-current text-black" xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                viewBox="0 0 18 18">
                <path
                  d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z">
                </path>
              </svg>
            </div>
          </div>
          <!--Body-->
          <div class="my-5">
            <form @submit.prevent="updateTask" >
              <label for="editName" class="text-black">Name:</label>
              <input v-model="editedTask.name" type="text" id="editName" class="shadow appearance-none border rounded w-full py-2 px-3 text-grey-darker" required>
              <br>
              <br>
              <label for="editTime" class="text-black">Time:</label>
              <input v-model.number="editedTask.time" type="number" id="editTime" class="shadow mb-3 appearance-none border rounded w-full py-2 px-3 text-grey-darker" required>
              <button type="submit" class="bg-blue-600 hover:bg-blue-800 text-white font-bold py-2 px-4 rounded">Submit</button>
              <button @click="closeEditPopup" type="button" class="bg-red-600 hover:bg-red-800 ml-3 text-white font-bold py-2 px-4 rounded">Cancel</button>
            </form>
          </div>
          <!--Footer-->
          
        </div>
      </div>
    </div>
  </teleport>
</template>

<style scoped>
.animated {
			-webkit-animation-duration: 1s;
			animation-duration: 1s;
			-webkit-animation-fill-mode: both;
			animation-fill-mode: both;
		}

		.animated.faster {
			-webkit-animation-duration: 500ms;
			animation-duration: 500ms;
		}

		.fadeIn {
			-webkit-animation-name: fadeIn;
			animation-name: fadeIn;
		}

		.fadeOut {
			-webkit-animation-name: fadeOut;
			animation-name: fadeOut;
		}

		@keyframes fadeIn {
			from {
				opacity: 0;
			}

			to {
				opacity: 1;
			}
		}

		@keyframes fadeOut {
			from {
				opacity: 1;
			}

			to {
				opacity: 0;
			}
		}
</style>

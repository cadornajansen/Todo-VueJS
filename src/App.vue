<script setup>
import { trackSlotScopes } from '@vue/compiler-core';
import { ref, watchEffect } from 'vue';
import Footer from './components/Footer.vue';

const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || [])
const taskInput = ref('');

if (!localStorage.getItem('functionExecuted')) {
  tasks.value.push({title: 'Take the laundry',
  isChecked: false})
  tasks.value.push({title: 'Buy groceries',
  isChecked: true})
  tasks.value.push({title: 'Feed the cats',
  isChecked: true})
  tasks.value.push({title: 'Clean out inbox',
  isChecked: false})
  localStorage.setItem('functionExecuted', true);
}

watchEffect(() => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
});


const deleteTask = (t) => {
  tasks.value = tasks.value.filter(task => task !== t)
}

const handleSubmit = (e) => {
  e.preventDefault()
  if (taskInput.value.trim().length === 0) {
    return;
  }
  const form = document.getElementById("taskForm");
  const task = taskInput.value;
  tasks.value.push({
    title: task,
    isChecked: false,
  })
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
  form.reset();
}

const handleInput = (e) => {
  taskInput.value = e.target.value
}

</script>

<template>

  <div class="h-max-screen font-sans ">
  <!-- Page Header Section: Simple Dark -->
    <div class="space-y-6 container xl:max-w-7xl mx-auto px-4 py-10 lg:px-8 ">
      <div class="text-center">
        <div class="text-sm uppercase font-bold tracking-wider mb-1 text-blue-300">
          To-Do App
        </div>
        <h2 class="text-3xl md:text-4xl font-extrabold mb-4 text-white">
          Manage and Organize your Tasks
        </h2>
        <h3 class="text-lg md:text-xl md:leading-relaxed font-medium text-gray-400 lg:w-2/3 mx-auto">
          A simple project but efficient and intuitive app that helps you stay on top of your tasks as well as increase productivity.
        </h3>
      </div>
        <div class="w-full lg:w-1/3 mx-auto">
          <form id="taskForm" class="space-y-6 justify-center items-center text-center mb-5" v-on:submit="handleSubmit">
            <div class="space-y-1">
              <label for="mixed3" class="font-medium text-white">What needs to be done? <span class="font-normal text-gray-500">(tasks)</span></label>
              <div class="flex items-center">
                <button type="button" class="inline-flex justify-center items-center space-x-2 border font-semibold focus:outline-none flex-none px-3 py-2 leading-6 rounded-l active:z-1 focus:z-1 -mr-px border-blue-700 bg-blue-700 text-white">
                  All
                </button>
                <input class="block border border-gray-200 px-3 py-2 leading-6 w-full active:z-1 focus:z-1 -mr-px outline-none" type="text" id="mixed3" name="mixed3" placeholder="Clean and organize" 
                ref="taskInput"
                @input="handleInput"
                >
                <button type="button" class="inline-flex justify-center items-center space-x-2 border font-semibold focus:outline-none flex-none px-3 py-2 leading-6 rounded-r active:z-1 focus:z-1 border-blue-200 bg-blue-200 text-blue-700 hover:text-blue-700 " @click="(e) => handleSubmit(e)">
                  Enter
                </button>
              </div>
            </div>
          </form>
          <!-- List Group with badges -->
          <div class="bg-white rounded max-h-[240px] overflow-auto overflow-x-hidden">
          <ul v-for="task in tasks" :key="task">
            
            <li class="bg-white border-gray-200 rounded transition duration-300 ease-in-out hover:scale-[1.02] hover:bg-gray-100 transform-gpu p-4 flex items-center overflow-auto">
              <div class="flex items-center space-x-2">
                  <input type="checkbox" v-model="task.isChecked" id="checkbox2" name="checkbox2" class="rounded-full border-none outline-none h-4 w-4">
                  <div class="overflow-auto">
                    <p :style="{ 'text-decoration': task.isChecked ? 'line-through' : 'none' }" class="font-semibold text-sm max-w-3/4 overflow-wrap break-word">
                      {{task.title}}
                    </p>
                  </div>
              </div>

              <button  href="javascript:void(0)" class="inline-flex justify-center items-center outline-none font-semibold px-2 py-1 leading-5 text-sm rounded bg-white text-gray-800 transition duration-300 ease-in-out hover:shadow-2xl  shadow-sm ml-auto"
              @click="deleteTask(task)">
                <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
                width="20" height="20"
                viewBox="0,0,256,256"
                style="fill:#000000;">
                  <g fill="#214cea" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt" stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0" font-family="none" font-weight="none" font-size="none" text-anchor="none" style="mix-blend-mode: normal"><g transform="scale(10.66667,10.66667)"><path d="M10,2l-1,1h-4c-0.552,0 -1,0.448 -1,1c0,0.552 0.448,1 1,1h2h10h2c0.552,0 1,-0.448 1,-1c0,-0.552 -0.448,-1 -1,-1h-4l-1,-1zM5,7v13c0,1.105 0.895,2 2,2h10c1.105,0 2,-0.895 2,-2v-13z"></path></g></g>
                </svg>
              </button>
              <!-- Modal Container -->
        
            </li>
          </ul>
          </div>
          <Footer/>
          
        </div>
    
    </div>
  <!-- END Page Header Section: Simple Dark -->
  </div>
</template>
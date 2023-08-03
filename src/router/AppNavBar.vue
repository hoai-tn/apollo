<script setup>
import { ref } from 'vue';
const tab = ref('review')


const increment = (item) => {
  tab.value = item;
}

</script>

<template>
    <div class="overflow-auto relative z-[500] w-[280px] content-between">
      <div
        class="review gap-y-3 font-lemonada overflow-y-auto p-2 content-between h-full"
        id="nav">
        <div>
        <div class="tab-menu">
          <button :class="{ active: tab === 'review' }" id="review_btn" @click="increment('review')" >
            Review
          </button>
          <button :class="{ active: tab === 'action' }" id="action_btn" @click="increment('action')">
            Action
          </button>
          <button :class="{ active: tab === 'tasks' }" id="tasks_btn" @click="increment('tasks')">
            Tasks
          </button>
        </div>

        <div class="search flex justify-stretch gap-x-1 py-2 text-[9px] bg-white border border-gray-300 rounded-md shadow-xs">
          <svg class="w-[12px] h-[12px] text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
          </svg>
          <input
            placeholder='Search document..'
            class="bg-white font-body text-gray-900 outline-none"
          />
        </div>

        <div v-if="tab === 'tasks' " class=" gap-y-2 pt-[26px]" id="task-input-action">
          <div>
            <div>
              <input type="text" placeholder="Add Task" />
            </div>
            <div class="add_task_btn">
              <button type="button" >Add</button>
            </div>
          </div>

          <div class="relative bg-white  grid-flow-col p-2 border rounded-md content">
            <p class="pr-2 desc">
              1. Write the proposal
            </p>
            <div class="flex flex-col gap-y-1 btn">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
            </div>
          </div>

          <div class="relative bg-white  grid-flow-col p-2 border rounded-md content">
            <p class="pr-2 desc">
              2. Reply to the client.
            </p>
            <div class="flex flex-col gap-y-1 btn">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
            </div>
          </div>

          <div class="relative bg-white  grid-flow-col p-2 border rounded-md content">
            <p class="pr-2 desc">
              3. Send document for signing.
            </p>
            <div class="flex flex-col gap-y-1 btn">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
            </div>
          </div>
        </div>


        <div v-if="tab === 'review' || tab === 'action'" class=" gap-y-2 pt-[26px]" id="review_select">
          <div class=" gap-y-[10px] text-[11px]">
            <select class="px-2 w-full h-[28px] rounded-md border-[#9095a1ff] border outline-none text-[9px]">
              <option selected disabled>select</option>
              <option value="Receive">I'm receiving this Document</option>
              <option value="Send">I'm sending this Document</option>
            </select>
            <select class="px-2 w-full h-[28px] rounded-md border-[#9095a1ff] border outline-none text-[9px]" >
              <option selected disabled>select</option>
              <option value="Points_To_Negotiate">
                  Points_To_Negotiate
              </option>
              <option value="Missing_Clauses">Missing_Clauses</option>
              <option value="Complex_Language">Complex_Language</option>
              <option value="Ambiguities">Ambiguities</option>
              <option value="Undefined_Terms">Undefined_Terms</option>
              <option value="Inconsistencies">Inconsistencies</option>
              <option value="Conflicting_Terms">Conflicting_Terms</option>
              <option value="Non_standard_Clause">
                  Non_standard_Clause
              </option>
          </select>
          </div>

          <div class="flex justify-end analyse_btn">
            <div >
              <img src="../static/img/maze.svg" class="w-[17px] h-[18px] hidden" id="maze" alt="icon" />
            </div>
            <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg w-[68px] h-[20px] text-[9px]" onclick="analyse_clicked()">
              Analyse
            </button>
          </div>
        </div>

        <div v-if="tab === 'review' || tab === 'action'" class=" gap-y-2 pt-[26px] hidden" id="Tasks_select">
          <div class="px-2 w-full h-[28px] rounded-md border-[#9095a1ff] border outline-none text-[9px] flex">
            <input placeholder='Add Task' class="bg-white font-body text-[9px] outline-none w-full"/>
            <img src="../static/img/person.svg" alt="icon" class="w-[20px] h-[20px] self-center justify-end"/>
          </div>
          <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg text-[9px] w-[68px] h-[20px] justify-self-end">
            Add
          </button>
        </div>

        <div v-if="tab === 'review'|| tab === 'action'" class=" gap-y-2 text-[11px] pt-10" id="review_select_response">
          <div class="button-view">
            <button class="h-4"><img src="../static/img/close.svg" alt="icon"/></button>
          </div>
          <div class="relative bg-white  grid-flow-col p-2 border rounded-md content">
            <p class="pr-2 desc">
              This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
              This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
            </p>
            <div class="flex flex-col gap-y-1 btn">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/edit.svg" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon"/></button>
            </div>
          </div>
        
          <div class="relative bg-white  grid-flow-col p-2 border rounded-md content">
            <p class="pr-2 desc">
              This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
              This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
            </p>
            <div class="flex flex-col gap-y-1 btn">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/edit.svg" alt="icon"/></button>
              <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon"/></button>
            </div>
          </div>
        </div>

        <div v-if="tab === 'review'|| tab === 'action'" class=" gap-y-2 text-[11px] pt-10 hidden" id="Tasks_select_response">
          <button class="w-4 h-4 justify-self-end"><img src="../static/img/close.svg" alt="icon"/></button>
          <div class="relative bg-white grid grid-flow-col p-2 border rounded-md flex-auto">
            <p class="pr-2">
              1. Write the proposal.
            </p>
            <div class="flex flex-col gap-y-1 justify-self-end">
            <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon" /></button>
            <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon" /></button>
            </div>
          </div>
          <div class="relative bg-white  grid-flow-col p-2 border rounded-md flex-auto">
            <p class="pr-2">
              2. Reply to the client.
            </p>
            <img src="../static/img/person.svg" alt="icon" class="w-[20px] h-[20px] self-center"/>
            <div class="flex flex-col gap-y-1 justify-self-end">
            <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon" /></button>
            <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon" /></button>
            </div>
          </div>
          <div class="relative bg-white  grid-flow-col p-2 border rounded-md flex-auto">
            <p class="pr-2">
              3. Send document for signing.
            </p>
            <img src="../static/img/person.svg" alt="icon" class="w-[20px] h-[20px] self-center"/>
            <div class="flex flex-col gap-y-1 justify-self-end">
            <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
            <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon"/></button>
            </div>

          </div>

        </div>
      <!--discuss window start 
      <div class="fixed bottom-5 right-5 grid opacity-100 y-[30px] w-[259px]">
        <div class="dialog-overlay right-3 bottom-0 2-[100px]">
          <div class="w-full dialog bg-[#3659e2ff] rounded-[4px] text-white px-8 py-4 text-[14px]">
            <div class="flex">
              <div class="text-center mr-10">
                <img src="../static/img/amelia.svg" class="w-12 h-[60px]"/>
              </div>
              <div>

              </div>
              <h2 class="self-center text-[14px]">Amelia</h2>
              <p calss=> </p>
              <button class="absolute top-3 right-3 bg-white text-gray-900 rounded-[4px]">
                <img src="Expand" class="w-5 h-5" />
              </button>
            </div>
          </div>
          <div class="chat-history w-full h-[600px] bg-white rounded-[4px] border boder-gay-200 px-8 py-4 mb-1 text-[11px]">
            <div class="grid dialog">
              <p class="bg-gray-200 w-fit p-3 rounded-[8px] m-3 justify-self-start text-left">
                How can I help  you?
              </p>
            </div>
          </div>  
          <div class="w-full text-[12px] relative">
            <input class="w-full dialog rounded-[4px] border border-gay-900 px-8 py-4" placeholder="Send a message..."/>
          </div>
        </div>
        <button class="w-6 h-6 bg-[#3659e2ff] justify-self-end rounded-[4px] text-white">
          <img src="../static/img/down.png" class="w-[18] h-[19]"/>
        </button>
      </div>
      discuss window end-->
      </div>
      
      <div 
        v-if="tab === 'review'"
        class="flex gap-x-1 p-1 bg-white border border-gray-300 rounded-md shadow-xs text-[9px] action_button"
      >
        <button class="active text-gray-100 py-1 px-4 rounded-lg w-1/3" id="discuss_doc" onclick="Discuss_document()">
          Discuss
        </button>
        <button class="py-1 px-4 rounded-lg w-1/3" id="research_doc" onclick="Research_document()">
          Research
        </button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>

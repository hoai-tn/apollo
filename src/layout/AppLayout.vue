<script setup>
import { computed, watch, ref, onBeforeUnmount } from 'vue';
import AppTopbar from './AppTopbar.vue';
import AppSidebar from './AppSidebar.vue';
import AppConfig from './AppConfig.vue';
import AppProfileSidebar from './AppProfileSidebar.vue';
import AppBreadCrumb from './AppBreadcrumb.vue';
import { useLayout } from '@/layout/composables/layout';

const { layoutConfig, layoutState, isSidebarActive } = useLayout();

const outsideClickListener = ref(null);
const sidebarRef = ref(null);
const topbarRef = ref(null);

watch(isSidebarActive, (newVal) => {
    if (newVal) {
        bindOutsideClickListener();
    } else {
        unbindOutsideClickListener();
    }
});

onBeforeUnmount(() => {
    unbindOutsideClickListener();
});

const containerClass = computed(() => {
    return {
        'layout-light': layoutConfig.colorScheme.value === 'light',
        'layout-dim': layoutConfig.colorScheme.value === 'dim',
        'layout-dark': layoutConfig.colorScheme.value === 'dark',
        'layout-colorscheme-menu': layoutConfig.menuTheme.value === 'colorScheme',
        'layout-primarycolor-menu': layoutConfig.menuTheme.value === 'primaryColor',
        'layout-transparent-menu': layoutConfig.menuTheme.value === 'transparent',
        'layout-overlay': layoutConfig.menuMode.value === 'overlay',
        'layout-static': layoutConfig.menuMode.value === 'static',
        'layout-slim': layoutConfig.menuMode.value === 'slim',
        'layout-slim-plus': layoutConfig.menuMode.value === 'slim-plus',
        'layout-horizontal': layoutConfig.menuMode.value === 'horizontal',
        'layout-reveal': layoutConfig.menuMode.value === 'reveal',
        'layout-drawer': layoutConfig.menuMode.value === 'drawer',
        'layout-static-inactive': layoutState.staticMenuDesktopInactive.value && layoutConfig.menuMode.value === 'static',
        'layout-overlay-active': layoutState.overlayMenuActive.value,
        'layout-mobile-active': layoutState.staticMenuMobileActive.value,
        'p-input-filled': layoutConfig.inputStyle.value === 'filled',
        'p-ripple-disabled': !layoutConfig.ripple.value,
        'layout-sidebar-active': layoutState.sidebarActive.value,
        'layout-sidebar-anchored': layoutState.anchored.value
    };
});

const bindOutsideClickListener = () => {
    if (!outsideClickListener.value) {
        outsideClickListener.value = (event) => {
            if (isOutsideClicked(event)) {
                layoutState.overlayMenuActive.value = false;
                layoutState.overlaySubmenuActive.value = false;
                layoutState.staticMenuMobileActive.value = false;
                layoutState.menuHoverActive.value = false;
            }
        };
        document.addEventListener('click', outsideClickListener.value);
    }
};
const unbindOutsideClickListener = () => {
    if (outsideClickListener.value) {
        document.removeEventListener('click', outsideClickListener);
        outsideClickListener.value = null;
    }
};
const isOutsideClicked = (event) => {
    const sidebarEl = sidebarRef?.value.$el;
    const topbarEl = topbarRef?.value.$el.querySelector('.topbar-menubutton');

    return !(sidebarEl.isSameNode(event.target) || sidebarEl.contains(event.target) || topbarEl.isSameNode(event.target) || topbarEl.contains(event.target));
};
</script>

<template>
    <div :class="['layout-container', { ...containerClass }]">
        <AppSidebar ref="sidebarRef" />

        <div class="layout-content-wrapper">
            <AppTopbar ref="topbarRef" />
            <div class="flex gap-x-4">
                <iframe src='http://13.210.182.133:9001?showChat=false&showLineNumbers=false' class="w-full" height=1028></iframe>
                  <!---Nav bar start-->
      <div class="overflow-auto relative z-[500] w-[330px] content-between">
        <div
          class="review gap-y-3 font-lemonada overflow-y-auto p-2 content-between h-full"
          id="nav">
          <div>
          <div class="flex gap-x-1 p-1 bg-white border border-gray-300 rounded-md shadow-xs text-[9px]">
            <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg w-1/3" id="review_btn" onclick="on_review()">
              Review
            </button>
            <button class="py-1 px-4 rounded-lg w-1/3" id="action_btn" onclick="on_action()">
              Action
            </button>
            <button class="py-1 px-4 rounded-lg w-1/3" id="tasks_btn" onclick="on_tasks()">
              Tasks
            </button>
          </div>
          <div class="flex justify-stretch gap-x-1 py-2 text-[9px] bg-white border border-gray-300 rounded-md shadow-xs">
            <svg class="w-[12px] h-[12px] text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
            </svg>
            <input
              placeholder='Search document..'
              class="bg-white font-body text-gray-900 outline-none"
            />
          </div>
          <div class=" gap-y-2 pt-[26px]" id="review_select">
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
            <div class="flex justify-end">
              <div >
                <img src="../static/img/maze.svg" class="w-[17px] h-[18px] hidden m-0" id="maze" alt="icon" />
              </div>
              <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg w-[68px] h-[20px] text-[9px]" onclick="analyse_clicked()">
                Analyse
              </button>
            </div>
          </div>
          <div class="grid gap-y-2 pt-[26px] hidden m-0" id="Tasks_select">
            <div class="px-2 w-full h-[28px] rounded-md border-[#9095a1ff] border outline-none text-[9px] flex">
              <input placeholder='Add Task' class="bg-white font-body text-[9px] outline-none w-full"/>
              <img src="../static/img/person.svg" alt="icon" class="w-[20px] h-[20px] self-center justify-end"/>
            </div>
            <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg text-[9px] w-[68px] h-[20px] justify-self-end">
              Add
            </button>
          </div>
          <div class=" gap-y-2 text-[11px] pt-10" id="review_select_response">
            <div class="flex flex-row justify-end px-[8px]">
              <button class="w-4 h-4 flex justify-end"><img src="../static/img/close.svg" alt="icon"/></button>
            </div>
            <div class="flex flex-row justify-between bg-white p-2 border rounded-md mb-2">
              <p class="pr-2 w-[85%]">
                This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
                This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
              </p>
              <div class="flex flex-col gap-y-1 w-[15%] items-end">
                <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/edit.svg" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon"/></button>
              </div>
            </div>
            <div class="flex flex-row justify-between bg-white grid-flow-col p-2 border rounded-md mb-2">
              <p class="pr-2 w-[85%]">
                This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
                This project is about editing documentation so you can edit some doc or docx files by uploading from your computer.
              </p>
              <div class="flex flex-col gap-y-1 w-[15%] items-end">
                <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/check.svg" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/edit.svg" alt="icon"/></button>
                <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon"/></button>
              </div>
            </div>
          </div>
          <div class="flex-col grid gap-y-2 text-[11px] pt-10 hidden m-0" id="Tasks_select_response">
            <div class="flex flex-row justify-end px-[8px]">
              <button class="w-4 h-4 flex justify-end"><img src="../static/img/close.svg" alt="icon"/></button>
            </div>
            <div class="flex relative bg-white grid m-0 grid-flow-col p-2 border rounded-md flex-auto">
              <p class="pr-2">
                1. Write the proposal.
              </p>
              <div class="flex flex-col gap-y-1 justify-self-end">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon" /></button>
              <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon" /></button>
              </div>
            </div>
            <div class="flex  relative bg-white grid m-0 grid-flow-col p-2 border rounded-md flex-auto">
              <p class="pr-2">
                2. Reply to the client.
              </p>
              <img src="../static/img/person.svg" alt="icon" class="w-[20px] h-[20px] self-center"/>
              <div class="flex flex-col gap-y-1 justify-self-end">
              <button class="w-3 h-3"><img src="../static/img/miniclose.png" alt="icon" /></button>
              <button class="w-3 h-3"><img src="../static/img/copy.svg" alt="icon" /></button>
              </div>
            </div>
            <div class="flex relative bg-white grid m-0 grid-flow-col p-2 border rounded-md flex-auto">
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
        <div class="justify-around flex p-1 bg-white border border-gray-300 rounded-md shadow-xs text-[9px]">
          <button class="text-gray-100 bg-[#4069e5ff] py-1 px-4 rounded-lg w-1/3" id="discuss_doc" onclick="Discuss_document()">
            Discuss
          </button>
          <button class="py-1 px-4 rounded-lg w-1/3" id="research_doc" onclick="Research_document()">
            Research
          </button>
        </div>
      </div>
    </div>
      <!----Nav end-->
            </div>
        </div>
        <AppProfileSidebar />
        <AppConfig />
        <Toast></Toast>
        <div class="layout-mask"></div>
    </div>
</template>

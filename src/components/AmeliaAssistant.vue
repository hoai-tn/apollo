<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    messages: Array
});
const onCopy = async (s) => {
    await navigator.clipboard.writeText(s);
};
</script>

<template>
    <div class="absolute bottom-[480px] right-0 text-sm max-w-[350px] w-[350px] z-[700] rounded">
        <div class="bg-[#4069E5FF] flex justify-between text-white items-end p-2">
            <div>
                <img src="images/amelia.png" alt="amelia" />
            </div>
            <div>
                <div class="text-xl text-center pb-2">Amelia</div>
                <div>Legal Assistant</div>
            </div>
            <div>"Let's discuss this document"</div>
        </div>
        <div class="bg-white px-2 py-4 pb-8 flex-grow overflow-y-auto h-[359px]">
            <div class="flex flex-col space-y-5">
                <div v-for="(message, index) in props.messages" :key="index" :class="['relative flex items-center  rounded-xl te py-2 px-3', message.type === 0 ? 'self-start bg-[#F0F1F4FF]' : 'self-end bg-[#DCE2E9FF]']">
                    <p class="pr-10">{{ message.text }}</p>
                    <div v-if="message.isResponse" @click="() => onCopy(message.text)" class="absolute top-0 right-0 cursor-pointer active:bg-gray-300 rounded">
                        <img width="20" src="/images/copy.svg" alt="amelia" />
                    </div>
                </div>
            </div>
        </div>
        <div class="mt-1 text-center">
            <InputText type="text" placeholder="Send a message..." class="w-full" />
        </div>
    </div>
</template>
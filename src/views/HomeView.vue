<script setup>
    import { ref,reactive,onMounted,nextTick } from 'vue';
    import axios from 'axios'
    import ModalComponent from '../components/modalcomponent.vue'

    const baseUrl = `http://todoapi.test/api`
    const pendingTasks = ref(null);
    const completeTasks = ref(null);
    const pendingTaskCount = ref(0);
    const completeTaskCount = ref(0);
    const modalOpen = ref(false);

    const form = reactive({
        'name'  : ''
    })
    
    const UpdateForm = reactive({
        'id' : '',
        'name'  : ''
    })

    async function getPendingTaskFromApi(){
       let response = await axios.get(`${baseUrl}/pending/data`)
       pendingTasks.value = response.data; 
       pendingTaskCount.value = pendingTasks.value.length
    }

    async function getCompleteTaskFromApi(){
       let response = await axios.get(`${baseUrl}/complete/data`)
       completeTasks.value = response.data; 
       completeTaskCount.value = completeTasks.value.length
    }

    async function taskStore(){
        let response = await axios.post(`${baseUrl}/store`,form)
        if(response.status == 200){
            getPendingTaskFromApi()
            getCompleteTaskFromApi()
            form.name = ''
        }
    }

    async function markDone(id){
        let response = await axios.put(`${baseUrl}/mark/complete/${id}`)
        if(response.status == 200){
            getPendingTaskFromApi()
            getCompleteTaskFromApi()
        }
    }


    async function markUnDo(id){
        let response = await axios.put(`${baseUrl}/mark/undo/${id}`)
        if(response.status == 200){
            getPendingTaskFromApi()
            getCompleteTaskFromApi()
        }
    }


    async function taskDelete(id){
        let response = await axios.delete(`${baseUrl}/delete/${id}`)
        if(response.status == 200){
            getPendingTaskFromApi()
            getCompleteTaskFromApi()
        }
    }


    async function editTask(id){
        let response = await axios.get(`${baseUrl}/edit/${id}`)
        if(response.status == 200){
            UpdateForm.id = response.data.id
            UpdateForm.name = response.data.name
            modalOpen.value = true;
        }
    }


    async function updateTask(id){
        let response = await axios.put(`${baseUrl}/update/${id}`,UpdateForm)
        if(response.status == 200){
            getPendingTaskFromApi()
            getCompleteTaskFromApi()
            UpdateForm.id = ''
            UpdateForm.name = ''
            modalOpen.value = false;
        }
        
    }


    onMounted(()=>{
        getPendingTaskFromApi()
        getCompleteTaskFromApi()
    });

</script>

<template>    
   <div class="min-h-screen w-screen bg-gradient-to-r from-red-300 to-sky-300 flex flex-col gap-2 p-10 items-center">
        <h1 class="text-xl text-red-900 font-bold">Simple Todo App</h1>  
        {{ UpdateForm }}
        <div class="max-w-4xl w-full bg-white p-2 rounded-sm shadow-lg">
            <form enctype="multipart/form-data" class="grid grid-cols-12 gap-2">
                <div class="col-span-9">
                    <input v-model="form.name" class="w-full p-2 border focus:outline-none focus:border-green-200 focus:shadow-lg" type="text" name="task" placeholder="Enter new Task Description">
                </div>
                <div class="col-span-3 flex items-center">
                    <button @click="taskStore()" class="w-full px-5 py-2 bg-sky-400 text-white rounded-md font-semibold hover:bg-sky-500  hover:cursor-pointer" type="button" name="submit">Add new Task</button>
                </div>
            </form>
        </div>

        <div class="max-w-4xl w-full bg-white p-2 raounded-sm shadow-lg">
            <table class="w-full table-fixed border-collapse border-slate-950 hover:border-blue-950">
                <thead>
                    <tr>
                        <th class="p-2 border border-slate-300">Todo Task Description</th>
                        <th class="p-2 border border-slate-300">Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td colspan="2" class="py-1 font-bold text-center bg-red-400 text-white">Pending</td>
                    </tr>
                    <tr v-if="0 === pendingTaskCount">
                        <td colspan="2">No pending task available</td>
                    </tr>

                    <tr v-for="(pendingTask, index) in pendingTasks" :key="index">
                        <td class="p-2 border border-slate-300">
                          {{ pendingTask.name }}
                        </td>
                        <td class="p-2 border border-slate-300 flex gap-1 justify-end">
                            <input @click="markDone(pendingTask.id)" type="button" name="submit" value="Mark as Done" class="px-2 bg-sky-400 text-white rounded-md hover:bg-sky-600 font-semibold hover:cursor-pointer">
                            <input @click="editTask(pendingTask.id)" type="button" name="submit" value="Edit" class="px-2 bg-yellow-400 text-white rounded-md hover:bg-yellow-600 font-semibold hover:cursor-pointer">
                            <input @click="taskDelete(pendingTask.id)" type="button" name="submit" value="Delete" class="px-2 bg-red-400 text-white rounded-md hover:bg-red-600 font-semibold hover:cursor-pointer">
                        </td>
                    </tr>
                    <tr>
                        <td colspan="2" class="py-1 font-bold bg-green-800 text-white text-center">Completed</td>
                    </tr>
                    <tr v-if="0 === completeTaskCount">
                        <td colspan="2">No Completed task available</td>
                    </tr>

                    <tr v-for="(completeTask, index) in completeTasks" :key="index">
                        <td class="p-2 border border-slate-300 line-through">
                         {{ completeTask.name }}
                        </td>
                        <td class="p-2 border border-slate-300 flex gap-1 justify-end">
                            <input @click="markUnDo(completeTask.id)" type="button" name="submit" value="Undo" class="px-2 bg-gray-400 text-white rounded-md hover:bg-gray-600 font-semibold hover:cursor-pointer">
                            <input @click="editTask(pendingTask.id)" type="button" name="submit" value="Edit" class="px-2 bg-yellow-400 text-white rounded-md hover:bg-yellow-600 font-semibold hover:cursor-pointer">
                            <input @click="taskDelete(completeTask.id)" type="button" name="submit" value="Delete" class="px-2 bg-red-400 text-white rounded-md hover:bg-red-600 font-semibold hover:cursor-pointer">
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
   </div>


   <ModalComponent v-if="modalOpen">
        <input v-model="UpdateForm.name" class="w-full p-2 mb-2 border focus:outline-none focus:border-green-200 focus:shadow-lg" type="text" name="task" placeholder="Enter new Task Description">
        <div class="space-y-4">
            <button @click="updateTask(UpdateForm.id)"  class="p-3 bg-black rounded-full text-white w-full font-semibold">Update Task</button>
            <button @click="modalOpen = false"  class="p-3 bg-white border rounded-full w-full font-semibold">Cancel</button>
        </div>
    </ModalComponent>
</template>

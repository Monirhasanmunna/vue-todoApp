<script setup>
    import { ref,reactive,onMounted,nextTick } from 'vue';
    import axios from 'axios'

    const task = ref(null);

   async function getTastFromApi(){
       let response = await axios.get('http://todoapi.test/api/index')
       console.log(response);
                    
    }

    onMounted(()=>{
        getTastFromApi()
    });
</script>

<template>    
   <div class="min-h-screen w-screen bg-gradient-to-r from-red-300 to-sky-300 flex flex-col gap-2 p-10 items-center">
        <h1 class="text-xl text-red-900 font-bold">Simple Todo App</h1>  
        <div class="max-w-4xl w-full bg-white p-2 rounded-sm shadow-lg">
            <form enctype="multipart/form-data" class="grid grid-cols-12 gap-2">
                <div class="col-span-9">
                    <input class="w-full p-2 border focus:outline-none focus:border-green-200 focus:shadow-lg" type="text" name="task" placeholder="Enter new Task Description">
                </div>
                <div class="col-span-3 flex items-center">
                    <input class="w-full px-5 py-2 bg-sky-400 text-white rounded-md font-semibold hover:bg-sky-500  hover:cursor-pointer" type="submit" name="submit" value="Add new Task">
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
                    <tr>
                        <td colspan="2">No pending task available</td>
                    </tr>
                    <tr>
                        <td class="p-2 border border-slate-300">
                          pending task
                        </td>
                        <td class="p-2 border border-slate-300 flex gap-1 justify-end">
                            <form enctype="multipart/form-data">
                                <input type="submit" name="submit" value="Mark as Done" class="px-2 bg-sky-400 text-white rounded-md hover:bg-sky-600 font-semibold hover:cursor-pointer">
                            </form>

                            <form enctype="multipart/form-data">
                                <input type="submit" name="submit" value="Edit" class="px-2 bg-yellow-400 text-white rounded-md hover:bg-yellow-600 font-semibold hover:cursor-pointer">
                            </form>

                            <form  enctype="multipart/form-data">
                                <input type="submit" name="submit" value="Delete" class="px-2 bg-red-400 text-white rounded-md hover:bg-red-600 font-semibold hover:cursor-pointer">
                            </form>
                        </td>
                    </tr>
                    <tr>
                        <td colspan="2" class="py-1 font-bold bg-green-800 text-white text-center">Completed</td>
                    </tr>
                    <tr>
                        <td colspan="2">No Completed task available</td>
                    </tr>

                    <tr>
                        <td class="p-2 border border-slate-300 line-through">
                         complete task
                        </td>
                        <td class="p-2 border border-slate-300 flex gap-1 justify-end">
                            <form enctype="multipart/form-data">
                                <input  type="submit" name="submit" value="Undo" class="px-2 bg-gray-400 text-white rounded-md hover:bg-gray-600 font-semibold hover:cursor-pointer">
                            </form>
                            <form enctype="multipart/form-data">
                                <input  type="submit" name="submit" value="Edit" class="px-2 bg-yellow-400 text-white rounded-md hover:bg-yellow-600 font-semibold hover:cursor-pointer">
                            </form>
                            <form enctype="multipart/form-data">
                                <input type="submit" name="submit" value="Delete" class="px-2 bg-red-400 text-white rounded-md hover:bg-red-600 font-semibold hover:cursor-pointer">
                            </form>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
   </div>
</template>

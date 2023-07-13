<template>
  <div>
    <section class="bg-gray-50 sm-full dark:bg-gray-900">
      <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
        <!-- component -->
        <!-- This is an example component -->
        <div class="max-w-2xl mx-auto">

          <div class="p-4 max-w-md bg-white rounded-lg border shadow-md sm:p-8 dark:bg-gray-800 dark:border-gray-700">

            <input type="text" class="inline-block w-full mb-10 border rounded-sm" @keyup.enter="onSave"
              placeholder="Kullanıcı ekle">
            <div class="flex justify-between items-center mb-4">

              <h3 class="text-xl font-bold leading-none text-gray-900 dark:text-white">Latest Customers</h3>

            </div>
            <div class="flow-root">
              <ul role="list" class="divide-y divide-gray-200 dark:divide-gray-700">
                <li v-for="item in itemsList" :key="item.title" class="py-3 sm:py-4">

                  <div class="flex items-center space-x-4">
                    <div class="flex-shrink-0">
                      <img class="w-8 h-8 rounded-full"
                        src="https://flowbite.com/docs/images/people/profile-picture-1.jpg" alt="Neil image">
                    </div>
                    <div class="flex-1 min-w-0">
                      <p class="text-sm font-medium text-gray-900 truncate dark:text-white">
                        {{ item.title }}

                      </p>
                      <p class="text-sm text-gray-500 truncate dark:text-gray-400">
                        email@windster.com
                      </p>
                    </div>
                    <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
                      $320
                    </div>
                    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" @click="onDelete(item)">sil</button>
                  </div>
                </li>

              </ul>
            </div>
          </div>
          <p class="text-white flex justify-end mt-3 font-bold">{{ itemsList.length }} Kullanıcı </p>
        </div>
      </div>
    </section>
  </div>
  <div>
  </div>
</template>

<script>

import axios from "axios"
export default {
  data() {
    return {
      itemsList: [

      ]
    }
  },
  mounted() {
    axios.get("http://localhost:3000/items").then(items_response => {
      console.log("items_response", items_response);
      this.itemsList = items_response.data || [];
      console.log("itemsList", this.itemsList);
    })
  },
  methods: {
    onSave(e) {
      const saveObject = {
        title: e.target.value,
        created_at: new Date(),
        completed: false,

      };

      axios.post("http://localhost:3000/items", saveObject).then(save_response => {
        console.log(save_response);
        this.itemsList.push(save_response.data)
        e.target.value = '',
          e.target.focus();
      })
    },

    onDelete(item) {
      axios.delete(`http://localhost:3000/items/${item.id}`).then(delete_response => {
        console.log(delete_response);
        this.itemsList = this.itemsList.filter(i => i.id != item.id)
      })
    }
  }
}
</script>

<style lang="scss" scoped></style>
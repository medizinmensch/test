<template>
  <div>
    <input
      type="text"
      name="search_string"
      id="search_string"
      v-model="this.search"
      class="focus:ring-indigo-500 focus:border-indigo-500 flex-1 block w-full rounded-r sm:text-sm border-gray-500 m-2 p-2"
      placeholder="Search for anything"
    />
  </div>
  <div class="flex flex-col">
    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
        <div
          class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg"
        >
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
              <tr>
                <th scope="col" class="py-3 px-0 font-medium text-gray-500">
                  Live
                </th>
                <th
                  scope="col"
                  class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Title
                </th>
                <th
                  scope="col"
                  class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Difficulty / Equipment
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Edit</span>
                </th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
              <tr v-for="item in filteredItems" :key="item.id">
                <td>
                  <div v-if="item.live" class="w-4 h-20 bg-red-300"></div>
                </td>
                <td class="p-0 m-0 whitespace-nowrap">
                  <div class="flex items-center">
                    <div class="flex-shrink-2 w-36">
                      <a :href="item.link">
                        <img
                          :src="getImagePath(item.id)"
                          :alt="item.id"
                        />
                      </a>
                    </div>
                    <div class="ml-4">
                      <div class="text-sm font-medium text-gray-900">
                        {{ item.heading }}
                      </div>
                      <div class="text-sm text-gray-500">
                        {{ item.subtitle }}
                      </div>
                    </div>
                  </div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div>
                    <div
                      v-if="item.difficulty_beginner"
                      class="text-sm rounded-full bg-green-100 text-gray-500 inline-flex px-1.5"
                    >
                      beginner
                    </div>
                    <div
                      v-if="item.difficulty_medium"
                      class="text-sm rounded-full bg-yellow-100 text-gray-600 inline-flex px-1.5"
                    >
                      medium
                    </div>
                    <div
                      v-if="item.difficulty_intense"
                      class="text-sm rounded-full bg-red-100 text-gray-800 inline-flex px-1.5"
                    >
                      intense
                    </div>
                  </div>

                  <div
                    class="text-sm rounded-full bg-blue-100 text-green-800 inline-flex px-1.5"
                  >
                    {{ item.equipment }}
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Fuse from "fuse.js";
import db_items from "../assets/data";

export default {
  methods: {
    getImagePath: function (id) {
      return "/assets/images/" + id + ".jpg";
    },
  },
  computed: {
    filteredItems() {
      if (this.search == "") {
        return this.items;
      }

      const options = {
        includeScore: false,
        keys: this.propsToSearch,
      };

      const fuse = new Fuse(this.items, options);

      return fuse.search(this.search).map((e) => {
        return e.item;
      });
    },
  },
  data() {
    console.log(db_items);
    return {
      search: "",
      calories: "",
      propsToSearch: [
        "orig_title",
        "heading",
        "subtitle",
        "length",
        "titleLength",
        "accent_color",
        "workout_type",
        "tags",
        "difficulty",
        "trains",
        "scenery",
        "equipment",
        "together",
        "easter_eggs",
        "songs",
      ],
      items: db_items,
    };
  },
};
</script>
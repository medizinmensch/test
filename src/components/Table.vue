<template>
  <div class="block w-full">
    <input
      type="text"
      name="search_string"
      id="search_string"
      v-model="this.search"
      class="w-full -mr-3 m-2 p-2 bg-gray-100 block rounded-lg"
      placeholder="Search for anything"
    />
  </div>
  <div class="flex p-1 items-center bg-gray-100">
    <p class="font-medium p-2">Difficulty:</p>
    <div>
      <Blob
        content="Beginner"
        @clicked="this.filter.simple.difficulty_beginner = $event"
        :state="this.filter.simple.difficulty_beginner"
      />
      <Blob
        content="Medium"
        @clicked="this.filter.simple.difficulty_medium = $event"
        :state="this.filter.simple.difficulty_medium"
      />
      <Blob
        content="Intense"
        @clicked="this.filter.simple.difficulty_intense = $event"
        :state="this.filter.simple.difficulty_intense"
      />
    </div>
  </div>
  <div class="flex p-1 items-center">
    <p class="font-medium p-2">Region:</p>
    <div>
      <Blob
        content="Abs"
        @clicked="this.filter.trains.abs = $event"
        :state="this.filter.trains.abs"
      />
      <Blob
        content="Arms"
        @clicked="this.filter.trains.arms = $event"
        :state="this.filter.trains.arms"
      />
      <Blob
        content="Back"
        @clicked="this.filter.trains.back = $event"
        :state="this.filter.trains.back"
      />
      <Blob
        content="Booty"
        @clicked="this.filter.trains.booty = $event"
        :state="this.filter.trains.booty"
      />
      <Blob
        content="Full Body"
        @clicked="this.filter.trains.fullbody = $event"
        :state="this.filter.trains.fullbody"
      />
      <Blob
        content="Legs"
        @clicked="this.filter.trains.legs = $event"
        :state="this.filter.trains.legs"
      />
      <Blob
        content="Upper body"
        @clicked="this.filter.trains.upperbody = $event"
        :state="this.filter.trains.upperbody"
      />
    </div>
  </div>
  <div class="flex p-1 items-center bg-gray-100">
    <p class="font-medium p-2">Workout:</p>
    <div>
      <Blob
        content="Warmup"
        @clicked="this.filter.type.warmup = $event"
        :state="this.filter.type.warmup"
      />
      <Blob
        content="Strength"
        @clicked="this.filter.type.strength = $event"
        :state="this.filter.type.strength"
      />
      <Blob
        content="Stamina"
        @clicked="this.filter.type.stamina = $event"
        :state="this.filter.type.stamina"
      />
      <Blob
        content="Dance"
        @clicked="this.filter.type.dance = $event"
        :state="this.filter.type.dance"
      />
      <Blob
        content="Cooldown"
        @clicked="this.filter.type.cooldown = $event"
        :state="this.filter.type.cooldown"
      />
      <Blob
        content="Stretch"
        @clicked="this.filter.type.stretch = $event"
        :state="this.filter.type.stretch"
      />
      <Blob
        content="No Workout"
        @clicked="this.filter.type.no_workout = $event"
        :state="this.filter.type.no_workout"
      />
    </div>
  </div>
  <div class="flex p-1 items-center">
    <p class="font-medium p-2">Misc. :</p>
    <div>
      <Blob
        content="Live"
        @clicked="this.filter.simple.together = $event"
        :state="this.filter.simple.together"
      />
      <Blob
        content="Song Workout"
        @clicked="this.filter.simple.song_workout = $event"
        :state="this.filter.simple.song_workout"
      />
    </div>
  </div>
  <div
    class="py-2 rounded-lg min-w-full px-6 overflow-x-auto -mx-6"
  >
    <table class="divide-y divide-gray-200 min-w-full">
      <thead class="bg-gray-100">
        <tr>
          <th scope="col" class="py-3 px-0 font-medium text-gray-500">🔴</th>
          <th scope="col" class="text-left text-gray-500 tracking-wider">
            Link
          </th>
          <th scope="col" class="text-left text-gray-500 tracking-wider">
            Title
          </th>
          <th scope="col" class="text-left text-gray-500 tracking-wider">
            Info
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
                  <!-- <img :src="getImagePath(item.id)" :alt="item.id" /> -->
                  <img :alt="item.id" />
                </a>
              </div>
            </div>
          </td>
          <td>
            <div class="ml-4">
              <div class="text-sm font-medium text-gray-900">
                {{ item.heading }}
              </div>
              <div class="text-sm text-gray-500">
                {{ item.subtitle }}
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
</template>

<script>
import Fuse from "fuse.js";
import db_items from "../assets/data.json";
import Blob from "./blob.vue";

export default {
  components: {
    Blob,
  },
  methods: {
    getImagePath: function (id) {
      return "/test/assets/images/" + id + ".jpg";
    },
  },
  computed: {
    filteredItems() {
      let reducedItems = this.items;

      // hard filters

      // 'Simple' (true/false) filters
      for (const [key, value] of Object.entries(this.filter.simple)) {
        if (value != "default") {
          const must = value == "must" ? true : false;
          reducedItems = reducedItems.filter((e) => e[key] == must);
        }
      }

      // workout Type type filter
      for (const [key, value] of Object.entries(this.filter.type)) {
        if (value != "default") {
          const must = value == "must" ? true : false;
          reducedItems = reducedItems.filter(
            (e) => e.type.includes(key) == must
          );
        }
      }

      // 'trains' (body section) filter
      for (const [key, value] of Object.entries(this.filter.trains)) {
        if (value != "default") {
          const must = value == "must" ? true : false;
          reducedItems = reducedItems.filter(
            (e) => e.trains.includes(key) == must
          );
        }
      }

      // Text-Search
      if (this.search != "") {
        const options = {
          includeScore: "default",
          keys: this.propsToSearch,
        };

        const fuse = new Fuse(reducedItems, options);
        return fuse.search(this.search).map((e) => {
          return e.item;
        });
      }

      return reducedItems;
    },
  },
  data() {
    console.log(db_items);
    return {
      search: "",
      calories: "",
      filter: {
        // default, must, mustnot
        simple: {
          difficulty_beginner: "default",
          difficulty_medium: "default",
          difficulty_intense: "default",
          together: "default",
          song_workout: "default",
        },
        type: {
          cooldown: "default",
          dance: "default",
          stamina: "default",
          strength: "default",
          stretch: "default",
          warmup: "default",
          no_workout: "mustnot",
        },
        trains: {
          abs: "default",
          arms: "default",
          back: "default",
          booty: "default",
          fullbody: "default",
          leg: "default",
          legs: "default",
          upperbody: "default",
          waist: "default",
        },
      },
      propsToSearch: ["full_title", "scenery"],
      items: db_items,
    };
  },
};
</script>

<template>
  <app-layout>
    <template #header>
      <div class="grid grid-cols-2 items-center">
        <h2 class="header">Years</h2>
        <div class="justify-end">
          <multiselect
            style="width: 50%; z-index: 10"
            class="float-right rounded-md border border-black"
            placeholder="Select Company."
            v-model="co_id"
            track-by="id"
            label="name"
            :options="options"
            @update:model-value="coch"
          >
          </multiselect>
        </div>
      </div>
    </template>

    <FlashMessage />

    <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-2">
      <!-- <jet-button @click="create" class="mt-4 ml-8">Create</jet-button> -->

      <form @submit.prevent="form.get(route('years.create'))">
        <!-- <div class="grid grid-cols-2"> -->
        <jet-button type="submit" @click="create" class="ml-2 buttondesign"
          >Add Year</jet-button
        >
        <!-- <div class="justify-end overflow-hidden">
            <multiselect
              style="width: 50%"
              class="float-right rounded-md border border-black"
              placeholder="Select Company."
              v-model="co_id"
              track-by="id"
              label="name"
              :options="options"
              @update:model-value="coch"
            >
            </multiselect>
          </div>
        </div> -->
        <!-- <button
          class="border bg-indigo-300 rounded-xl px-4  m-1 ml-2 mt-4"
          type="submit"
          :disabled="form.processing"
        >
          Add Year
        </button> -->
        <!-- <select
          v-model="co_id"
          class="pr-2 ml-2 pb-2 w-full lg:w-1/4 rounded-md float-right mt-2"
          label="company"
          @change="coch"
        >
          <option v-for="type in companies" :key="type.id" :value="type.id">
            {{ type.name }}
          </option>
        </select> -->
        <!-- <div v-if="errors.type">{{ errors.type }}</div> -->
        <div class="">
          <div class="obsolute  mt-2 ml-2 sm:rounded-2xl">
            <table class="table2">
              <thead>
                <tr class="tablerowhead">
                  <th class="py-1 px-4 rounded-l-2xl">Company</th>
                  <th class="py-1 px-4 ">Begin</th>
                  <th class="py-1 px-4 ">End</th>
                  <th class="py-1 px-4 rounded-r-2xl">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  class="tablerowbody2"
                  v-for="item in balances.data"
                  :key="item.id"
                >
                  <td class="w-4/12 px-4 border rounded-l-2xl w-2/5">
                    {{ item.company_name }}
                  </td>
                  <td class="w-2/12 px-4 border w-2/6 text-center">
                    {{ item.begin }}
                  </td>
                  <td class="w-2/12 px-4 border w-2/6 text-center">
                    {{ item.end }}
                  </td>
                  <td class="w-4/12px-4 border w-2/6 rounded-r-2xl text-center">
                    <button
                      class="
                        editbutton
                        px-4
                        m-1
                      "
                      @click="edit(item.id)"
                      type="button"
                    >
                      <span>Edit</span>
                    </button>
                    <button
                      class="
                        deletebutton
                        px-4
                        m-1
                      "
                      @click="destroy(item.id)"
                      type="button"
                      v-if="item.delete"
                    >
                      <span>Delete</span>
                    </button>
                    <button
                      v-if="item.closed == 0"
                      class="
                        border
                        bg-gray-600
                        text-white
                        font-bold
                        rounded-xl
                        px-4
                        m-1
                        hover:bg-gray-700 
                      "
                      @click="close(item.id)"
                      type="button"
                    >
                      <span>Close Fiscal</span>
                    </button>
                  </td>
                </tr>
                <tr v-if="balances.data.length === 0">
                  <td class="border-t px-6 py-4 " colspan="4">
                    No Record found.
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <paginator class="mt-6" :balances="balances" />
        </div>
      </form>
    </div>
  </app-layout>
</template>

<style src="@suadelabs/vue3-multiselect/dist/vue3-multiselect.css"></style>
<script>
import AppLayout from "@/Layouts/AppLayout";
import JetButton from "@/Jetstream/Button";
import { useForm } from "@inertiajs/inertia-vue3";
import Multiselect from "@suadelabs/vue3-multiselect";
import Paginator from "@/Layouts/Paginator";
import FlashMessage from "@/Layouts/FlashMessage";
// import { Head, Link } from "@inertiajs/inertia-vue3";

export default {
  components: {
    AppLayout,
    JetButton,
    useForm,
    Multiselect,
    Paginator,
    FlashMessage,
    // Link,
    // Head,
  },

  // props: ["data", "companies", "company"],
  props: {
    balances: Object,
    companies: Object,
    company: Object,
  },

  data() {
    return {
      // co_id: this.$page.props.co_id,
      co_id: this.company,
      options: this.companies,
    };
  },

  setup(props) {
    const form = useForm({});
    return { form };
  },

  methods: {
    create() {
      this.$inertia.get(route("years.create"));
    },

    edit(id) {
      this.$inertia.get(route("years.edit", id));
    },

    destroy(id) {
      this.$inertia.delete(route("years.destroy", id));
    },

    close(id) {
      this.$inertia.get(route("years.close", id));
    },

    coch() {
      // this.$inertia.get(route("companies.coch", this.co_id));
      this.$inertia.get(route("companies.coch", this.co_id["id"]));
    },
  },
};
</script>

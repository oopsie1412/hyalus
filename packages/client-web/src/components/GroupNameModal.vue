<template>
  <Modal>
    <div class="w-96">
      <div class="p-4 space-y-4">
        <div class="flex items-center space-x-2">
          <PencilIcon
            class="w-8 h-8 p-2 text-gray-400 rounded-full bg-gray-750"
          />
          <p class="text-xl font-bold text-gray-300">
            Change group name
          </p>
        </div>
        <div
          class="flex items-center p-3 space-x-3 text-sm text-gray-200 border border-gray-700 rounded-md bg-gray-750"
          v-if="error"
        >
          <ErrorIcon class="w-6 h-6" />
          <p>{{ error }}</p>
        </div>
        <div class="space-y-2">
          <p class="text-sm text-gray-500">Name</p>
          <input
            class="w-full px-4 py-2 text-gray-400 bg-gray-900 border rounded-sm border-gray-750 focus:outline-none focus:border-gray-650"
            type="text"
            v-model="name"
          />
        </div>
      </div>
      <div
        class="flex items-center justify-end p-4 space-x-2 text-sm bg-gray-900"
      >
        <p
          class="px-4 py-2 text-gray-500 cursor-pointer"
          @click="$emit('close')"
        >
          Cancel
        </p>
        <p
          class="px-4 py-2 text-white rounded-md shadow-sm cursor-pointer bg-primary-500"
          @click="setGroupName"
        >
          Change
        </p>
      </div>
    </div>
  </Modal>
</template>

<script>
export default {
  props: ["channel"],
  data() {
    return {
      error: null,
      name: this.channel.name,
    };
  },
  methods: {
    async setGroupName() {
      try {
        await this.$store.dispatch("setGroupName", {
          channel: this.channel.id,
          name: this.name,
        });
      } catch (e) {
        console.log(e);
        this.error = e?.response?.data?.error || e.message;
        return;
      }

      this.$emit("close");
    },
  },
  components: {
    Modal: () => import("./Modal"),
    PencilIcon: () => import("../icons/Pencil"),
    ErrorIcon: () => import("../icons/Error"),
  },
};
</script>

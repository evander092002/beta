<template>
  <div class="h-auto w-full p-5 flex flex-col gap-5">
    <div class="flex justify-between items-center">
      <h1 class="text-lg font-semibold text-custom-800 dark:text-white">Motion Feed</h1>
      <div class="lg:hidden block">
        <UButton @click="openPythonUI" label="Go Train" icon="i-lucide-hand"
          class="dark:text-custom-200 bg-custom-400 hover:bg-custom-500 dark:bg-custom-700 dark:hover:bg-custom-800 rounded p-2"
          size="xs" />
          <!-- to="/admin/motion-feed/train" -->
      </div>
    </div>
    <TableTrains />
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'sidebar'
})

const openPythonUI = async () => {
  try {
    const response = await fetch('http://localhost:5000/open-ui', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
    });

    if (response.ok) {
      console.log("UI opened");
    } else {
      console.error("Failed to open UI");
    }
  } catch (error) {
    console.error('There was an error opening the UI:', error);
  }
};

</script>
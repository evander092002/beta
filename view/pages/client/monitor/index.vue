<template>
  <div class="lg:h-screen h-[700px] w-full p-5">
    <Camera videoUrl="http://127.0.0.1:5000/video_feed" :isLive="true"/>
    <!-- <UButton label="Click me!" class="absolute top-52 right-10 rounded" @click="showToast"/> -->
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'sidebar'
})

import { ModalViewDetection } from '#components'

// initializations
const toast = useToast()
const modal = useModal()
const lastCheckTime = ref(null)

// shows and alters the warning widget
const showToast = () => {
  toast.add({
    title: 'Potential Theft Detected!',
    icon: 'i-lucide-triangle-alert',
    ui: {
      background : 'dark:bg-red-700 bg-red-200',
      progress: {background: 'dark:bg-white bg-custom-700 rounded-full'},
      ring: 'ring-1 ring-red-700 dark:ring-custom-900',
      default: {
        closeButton: { color: 'gray' },
        actionButton: { color: 'red' }
      },
      icon: 'text-custom-900'
    },
    actions: [
      {
        label: 'View',
        click: () => {
          modal.open(ModalViewDetection)
        }
      }
    ],
  });
}

// send sms
const sendSms = (date) => {
  // Send the request for SMS
  fetch('http://127.0.0.1:8000/api/send-twilio', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      phone: '+639810921795',
      message: 'Potential Theft Detected! At ' + date + '.'
    })
  })
  .then(response => response.json())
  .then(data => {
    if (data.success) {
      console.log('Message sent successfully!', data.success);
    } else {
      console.error('Failed to send message:', data.error);
    }
  })
  .catch(error => {
    console.error('Error:', error);
    console.log('Failed to send message:', error.message);
  });
};

// checks for any detected motion from the database
const checkForNewEntries = async () => {
  try {
    const response = await fetch('http://127.0.0.1:8000/api/trains')
    const data = await response.json()
    const latestEntry = data.trains.length ? data.trains[data.trains.length - 1] : null;
    const latestEntryTime = new Date(latestEntry.created_at).toISOString()

    // calls the notification functions if the condiition is met
    if (latestEntryTime > lastCheckTime.value) {
      // sendSms(new Date(latestEntryTime))
      showToast()

      // updates the latest detected motion
      lastCheckTime.value = latestEntryTime
    }

  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

// kicks start the app on mount
onMounted(async () => {
  try {
    // fetches the latest entry time on mount
    const response = await fetch('http://127.0.0.1:8000/api/trains')
    const data = await response.json()
    const latestEntry = data.trains[data.trains.length - 1]

    // initializes the variable lastCheckTime 
    lastCheckTime.value = new Date(latestEntry.created_at).toISOString()

    // loop logic to monitor the motions
    setInterval(checkForNewEntries, 5000) // Check every seconds
  } catch (error) {
    console.error('Error initializing last check time:', error)
 }
})
</script>


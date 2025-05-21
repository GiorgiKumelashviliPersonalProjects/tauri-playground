<script lang="ts">
  import {
    channels,
    isPermissionGranted,
    requestPermission,
    sendNotification,
  } from "@tauri-apps/plugin-notification";

  async function handleNotification(event: MouseEvent) {
    // Do you have permission to send a notification?
    let permissionGranted = await isPermissionGranted();

    console.log("=".repeat(20));
    console.log(permissionGranted);

    // If not we need to request it
    if (!permissionGranted) {
      const permission = await requestPermission();
      permissionGranted = permission === "granted";
    }

    // Once permission has been granted we can send the notification
    if (permissionGranted) {
      sendNotification({
        title: "Tauri",
        body: "Tauri is awesome!",
        summary: "Tauri is awesome!",
      });
    }

    console.log("=".repeat(20));
    console.log(await channels());
  }
</script>

<button
  class="bg-amber-500 border p-2 rounded cursor-pointer"
  onclick={handleNotification}
>
  send noticiation
</button>

<!-- <style>
  .context-area {
    padding: 10px;
    width: 300px;
    height: 200px;
    border: 2px dashed blue;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: default; /* Or 'context-menu' if you prefer */
    user-select: none; /* Prevent text selection on right-click drag */
    background-color: #f0f8ff;
  }
</style> -->

<script lang="ts">
  let { children } = $props();
</script>

<div class="flex gap-3">
  <a href="/" class="text-blue-700 underline"> Home </a>
  <a href="/native-context-menu" class="text-blue-700 underline">
    Native Context Menu
  </a>
  <a href="/notification" class="text-blue-700 underline"> Notification </a>
</div>

{@render children()}

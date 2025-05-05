<script lang="ts">
  let { children } = $props();
</script>

<div class="flex gap-3">
  <a href="/" class="text-blue-700 underline"> Home </a>
  <a href="/native-context-menu" class="text-blue-700 underline">
    Native Context Menu
  </a>
</div>

{@render children()}

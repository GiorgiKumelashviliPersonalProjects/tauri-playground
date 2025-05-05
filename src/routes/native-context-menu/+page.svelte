<script lang="ts">
  import { Menu, MenuItem, PredefinedMenuItem } from "@tauri-apps/api/menu";

  async function handleContextMenu(event: MouseEvent) {
    // Prevent the default browser context menu
    event.preventDefault();

    const item1 = await MenuItem.new({ id: "item-1", text: "First Action" });
    const itemGreet = await MenuItem.new({
      id: "greet",
      text: "Say Hello",
      action: () => {
        console.log("=".repeat(20));
        console.log(123);
      },
    });
    const separator = await PredefinedMenuItem.new({ item: "Separator" });
    const itemClose = await MenuItem.new({ id: "close-app", text: "Close" });

    const menu = await Menu.new({
      items: [item1, itemGreet, separator, itemClose],
    });

    await menu.popup();
  }
</script>

<div class="context-area" role="contentinfo" oncontextmenu={handleContextMenu}>
  Right-click here for the native context menu!
</div>

<style>
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
</style>

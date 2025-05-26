<script lang="ts">
  import {
    Menu,
    MenuItem,
    PredefinedMenuItem,
    type MenuItemOptions,
    type PredefinedMenuItemOptions,
  } from "@tauri-apps/api/menu";
  import { type MenuItemBase } from "@tauri-apps/api/menu/base";

  interface CustomMenuItem {
    type: "basic" | "predefined";
    item: Promise<MenuItemBase>;
  }

  interface CustomMenuConstructor {
    event: Event;
    items?: CustomMenuItem[];
  }

  class CustomMenu {
    private readonly event: Event;
    private readonly customItems: CustomMenuItem[] = [];

    constructor(params: CustomMenuConstructor) {
      this.event = params.event;
      this.customItems = params?.items ?? [];
    }

    public addMenuItem(options: MenuItemOptions) {
      this.customItems?.push({ type: "basic", item: MenuItem.new(options) });
      return this;
    }

    public addPredefined(options: PredefinedMenuItemOptions) {
      this.customItems?.push({
        type: "predefined",
        item: PredefinedMenuItem.new(options),
      });
      return this;
    }

    public addSeparator() {
      this.addPredefined({ item: "Separator" });
      return this;
    }

    public async createMenu(): Promise<Menu> {
      // Prevent the default browser context menu
      this.event.preventDefault();

      const items: MenuItem[] = this.customItems.length
        ? <MenuItem[]>await Promise.all(this.customItems.map((e) => e.item))
        : [];

      const menu = await Menu.new({ items });

      await menu.popup();

      return menu;
    }
  }

  const sayHello = (id: string) => {
    console.log("=".repeat(20));
    console.log(123);
    console.log(id);
    console.log("=".repeat(20));
  };

  async function handleContextMenu(event: MouseEvent) {
    const customMenu = new CustomMenu({ event })
      .addMenuItem({ id: "item-1", text: "First Action" })
      .addMenuItem({ id: "greet", text: "Say Hello", action: sayHello })
      .addSeparator()
      .addPredefined({ item: "Fullscreen" })
      .addMenuItem({ id: "close-app", text: "Close" });

    await customMenu.createMenu();
  }

  async function handleContextMenuAllPredefined(event: MouseEvent) {
    const customMenu = new CustomMenu({ event });

    const arr: PredefinedMenuItemOptions["item"][] = [
      "Separator",
      "Copy",
      "Cut",
      "Paste",
      "SelectAll",
      "Undo",
      "Redo",
      "Minimize",
      "Maximize",
      "Fullscreen",
      "Hide",
      "HideOthers",
      "ShowAll",
      "CloseWindow",
      "Quit",
      "Services",
    ];

    arr.forEach((e) => customMenu.addPredefined({ item: e }));

    customMenu.addPredefined({
      item: {
        About: {
          name: "test name",
          version: "0.5.6.9",
          credits: "test credits",
          copyright: "test copyright",

          authors: ["test"],
          comments: "test comment",
          license: "test license",
          shortVersion: "0.5.6.9",
          website: "test website",
        },
      },
    });

    await customMenu.createMenu();
  }
</script>

<div class="flex gap-2">
  <div
    class="context-area"
    role="contentinfo"
    oncontextmenu={handleContextMenu}
  >
    Right-click here for the native context menu!
  </div>
  <!-- <div
    class="context-area"
    role="contentinfo"
    oncontextmenu={handleContextMenuAllPredefined}
  >
    Right-click here for all "PREDEFINED" native context menu items!
  </div> -->
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

# JS-RTS-Menu

**JS-RTS-Menu** is a versatile, accessible, and script-driven JavaScript menu system. Originally designed with RTS (Real-Time Strategy) game UIs in mind, it's flexible enough for general web applications, context menus, and dynamic action lists. It is provided as a single HTML file (`JS-RTS-Menu-Demo.html`) containing all necessary HTML, CSS, and JavaScript, with no external dependencies (vanilla JS).

## Features

*   **Context Menus & Dropdown Menus:** Can be triggered by right-clicks, button clicks, or keyboard shortcuts.
*   **Accessibility (ARIA):** Implements ARIA roles (`menu`, `menuitem`, `menuitemcheckbox`, `menuitemradio`, `separator`), states (`aria-expanded`, `aria-haspopup`, `aria-disabled`, `aria-checked`), and properties (`aria-keyshortcuts`) for screen reader compatibility.
*   **Keyboard Navigation:**
    *   Arrow Up/Down: Navigate items.
    *   Enter/Space: Activate focused item.
    *   Escape: Clear active search, or close current menu level/entire menu.
    *   Home/End: Navigate to first/last item.
    *   Tab: Closes the menu and moves focus appropriately.
*   **Type-Ahead Find:** Users can type printable characters (excluding Space) to quickly find and focus menu items. Search query auto-clears after a short delay. Backspace modifies the search query.
*   **Submenus:** Supports nested submenus with "Back" button functionality for easy navigation.
*   **Item Types:**
    *   Standard action items.
    *   Checkbox items (`menuitemcheckbox`) that toggle state.
    *   Radio button items (`menuitemradio`) that work in exclusive groups.
    *   Separators for visual grouping.
*   **Dynamic Content Management:**
    *   Programmatically add, remove, and update menu items even while the menu is open.
    *   Dynamically change item labels, icons, tooltips, disabled states, costs, types (button, checkbox, radio), and hotkeys.
*   **Tooltips:** Built-in global tooltip system displays informative text on hover/focus for menu items, with support for separate tooltips for disabled states.
*   **Item Cooldowns:** Integrated cooldown manager for menu items, preventing rapid re-activation and providing visual feedback (timer overlay).
*   **Customizable Appearance:**
    *   Display icons next to item labels.
    *   Show "cost" text, with distinct styling for insufficient costs.
    *   Display hotkey hints.
*   **Theming:** Basic theming is supported via CSS Custom Properties. The demo includes a default light theme and an example dark theme.
*   **Positional Anchoring:** Menus can be anchored to HTML elements or specific x/y coordinates, with logic to attempt to keep the menu within the viewport.
*   **No External Dependencies:** Pure vanilla JavaScript, HTML, and CSS.

## Demo

The `index.html` file *is* the complete demonstration and source. To run the demo:

1.  Save the content as `index.html`.
2.  Open this file in a modern web browser.

The demo page showcases:
*   Triggering menus via buttons and context clicks (right-click).
*   Submenu navigation.
*   Checkbox items (e.g., "Enable Feature X").
*   Radio button items in groups (e.g., "Option Alpha/Beta").
*   Items with icons, costs, and hotkeys.
*   Dynamic item manipulation controls.
*   A theme switcher for default and dark themes.
*   An internal test suite to verify core functionality. Click "Run Health Check" to see all tests pass.

## How to Use (Within the Single Demo File)

The `JS-RTS-Menu` class and its supporting `CooldownManager` are defined within the `<script>` tags of `JS-RTS-Menu-Demo.html`.

1.  **HTML Structure:**
    Ensure these elements are present in your HTML body:
    ```html
    <div id="js-rts-menu-container" role="menu" aria-orientation="vertical"></div>
    <div id="js-rts-menu-global-tooltip" class="menu-tooltip" role="tooltip" aria-hidden="true" tabindex="-1"></div>
    ```

2.  **CSS:**
    The necessary CSS is included in the `<style>` block within `JS-RTS-Menu-Demo.html`. You can customize the CSS Custom Properties at the top of the style block (inside `:root { ... }`) to alter the default theme, or define your own theme classes (like `.js-rts-menu-dark-theme`).

3.  **JavaScript Initialization & Usage:**
    ```javascript
    // CooldownManager is available globally or can be instantiated
    const myCooldownManager = new CooldownManager(); 
    const myMenu = new JSRTSMenu('js-rts-menu-container', myCooldownManager);

    // Example: Define a function to build your menu
    const buildMyCustomMenu = () => {
        myMenu.addButton("Action 1", () => console.log("Action 1 fired!"), { hotkey: 'F5', tooltip: "Perform Action 1" });
        myMenu.addSeparator();
        myMenu.addButton("Toggle Setting", (isChecked) => { 
            console.log("Setting is now:", isChecked);
        }, { type: 'checkbox', checked: true, hotkey: 'T' });
    };

    // Example: Show the menu anchored to a button
    const myTriggerButton = document.getElementById('my-button');
    if (myTriggerButton) {
        myTriggerButton.addEventListener('click', (event) => {
            myMenu.showRoot(event.currentTarget, buildMyCustomMenu, event.currentTarget);
        });
    }
    ```

## `JSRTSMenu` Class API Highlights

### Constructor

*   `new JSRTSMenu(containerId, cooldownManagerInstance)`

### Core Methods

*   `showRoot(targetOrPosition, builderFn, triggerEl = null)`
*   `hide()`
*   `openSubmenu(submenuBuilderFn)`
*   `clear()`

### Item Creation

*   `addButton(label, callback, options = {})`
    *   **Key `options` properties:**
        *   `type`: `'button'` (default), `'checkbox'`, `'radio'`
        *   `checked`: (boolean) for checkable types
        *   `radioGroup`: (string) for radio types
        *   `disabled`: (boolean)
        *   `tooltip`: (string)
        *   `iconSrc`: (string)
        *   `hotkey`: (string) - Note: Single printable character hotkeys are superseded by type-ahead. Use F-keys (e.g., 'F1') or plan for modified hotkeys (e.g., 'ctrl+s') if needed with future enhancements.
        *   `opensSubmenu`: (boolean)
        *   `cooldownSeconds`, `abilityId`
*   `addBackButton()`
*   `addSeparator()`

### Dynamic Item Manipulation

*   `addItemAt(index, label, callback, options = {})`
*   `removeItemAt(index)`
*   `updateItemAt(index, newLabel, newOptions = {})`

## Development & Testing

The `JS-RTS-Menu-Demo.html` file includes an internal `MenuTester` class. Click the "Run Health Check" button on the demo page to execute a suite of automated tests verifying core functionalities.

## License

This project is licensed under the MIT License.

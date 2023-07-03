# Channel HTML 3D Product Viewer

## Setup
1. Copy `assets/` and `scripts/` folders from package to working project folder.

2. Add script references in HTML - ensure paths are accurate to where the folders are.

```
<script defer src="scripts/apple360spin.js"></script>
```

3.  Create and dispatch initializing event in JS.
```
<script>
  // Create an instance
  const init3D = new CustomEvent('channel-html:3djs:init');
  ...
  window.addEventListener('DOMContentLoaded', () => {
      window.dispatchEvent(init3D);
  });
</script>
```

4. Using a custom container
 - `data-3d-config` can be used to overwrite the properties listed below.
 For a full example, see the bottom of this document.

```
<div
  class="apple360spin-container"
  data-3d-config='{
    "axDeviceName": "",
    "pathTextures": "assets/textures/",
    "pathModels": "assets/models/",
    "containerClass": "apple360spin-container",
    "modalData": {
      "isModal": true,
      "customModalEntryID": "apple360Spin",
      "defaultModalEntryText": ""
    },
    "modelFileData": [{}],
    "iblFilename": "ibl.hdr",
    "actionPromptDesktop": "Drag or use keyboard to rotate",
    "actionPromptMobile": "Swipe to rotate"
  }'
></div>
```
 Note: The `data-3d-config` object will need to be minified in HTML (remove spaces)

5.  Adding a custom Model Entry Button
 - If a custom container is used this button should be placed inside it.
 - Ensure `id="apple360Spin"` is used (unless overwritten by the container).
 - The Model Entry Button has 3 optional data variables which can be used to override the texture, model, and svg paths:
`data-textures-path`
`data-models-path`
`data-svg-path`
```
<button
  id="apple360Spin"
  aria-label="Show 3D view for <device>"
  data-textures-path="assets/textures/"
  data-models-path="assets/models/"
  data-svg-path="assets/example/rotate.svg"
>
  Custom Entry
</button>
```

## Analytics Measurement

The 3d Product Viewer ships with rich analytics tracking various interactions from the end-user.

### Usage:
```
window.addEventListener('3dpv:event', (event) => {
    console.log('e', event.detail);
}
)
```

### The full list of events is as follows:
- `load:start` : denoting the moment the player becomes active
- `click:start-rotation` : the moment a user performs a mousedown action on the 3d model
- `click:end-rotation` : the moment a user performs a mouaseup action on the model
- `click:modal-open` : emitted when the modal is opened
- `click:modal-close` : emitted when the modal is opened
- `click:swatch:{color}` : emitted when a swatch is clicked

Each measurement broadcast reports the time between previous and current event,
as well as a payload containing the Event type proxied through the `event.detail` object.

#### Example:

```
// event.detail
{
     diff: 6
     key: "load:start"
     payload: {event: Quaternion}
     time: 1657833514597
}
```
## Accessibility Features

### Keyboard Interaction
- When the left or right arrow keys are pressed the product moves 15° on the X-axis in the direction pressed
- When the up and down arrow keys are pressed the product moves 15° on the Y-axis in the direction pressed

### Screen Reader
- Aria-labels and legend provided for the close button and colornav section
- Screen reader announcements for change of product angle
- Hide the `Drag to rotate` from the screen reader when the CTA disappears

### Reduced Motion
The opening of the modal is disabled when users with the reduced motion feature turned on.

###  Example HTML implementation

 - "EXAMPLE CUSTOM BUTTON ENTRY" shows how some parameters of the config can be overridden.
 - "EXAMPLE CUSTOM CONTAINER" show how the entire config can be overridden

```html
    <!--- EXAMPLE CUSTOM BUTTON ENTRY -->
    <!--
    <button
      id="apple360Spin"
      aria-label="Show 3D view for <device>"
      data-textures-path="assets/textures/"
      data-models-path="assets/models/"
      data-svg-path="assets/example/rotate.svg"
    >
      Custom Entry
    </button>
    -->

    <!-- EXAMPLE CUSTOM CONTAINER -->
    <div
      class="apple360spin-container"
      style="
        height: calc(100vh - 80px);
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      "
      data-3d-config='{"axDeviceName":"iPhone 13 Pro Max","pathTextures":"assets/textures/","pathModels":"assets/models/","containerClass":"apple360spin-container","modalData":{"isModal":true,"customModalEntryID":"apple360Spin","defaultModalEntryText":""},"modelFileData":[{"label":"Alpine Green","labelName":"Alpine Green","color":"#576856","filename":"iPhone13_ProMax_5G_AlpineGreen.gltf","index":0},{"label":"Silver","labelName":"Silver","color":"#F1F2ED","filename":"iPhone13_ProMax_5G_Silver.gltf","index":1},{"label":"Gold","labelName":"Gold","color":"#FAE7CF","filename":"iPhone13_ProMax_5G_Gold.gltf","index":2},{"label":"Graphite","labelName":"Graphite","color":"#54524F","filename":"iPhone13_ProMax_5G_Graphite.gltf","index":3},{"label":"Sierra Blue","labelName":"(Sierra)Blue","color":"#A7C1D9","filename":"iPhone13_ProMax_5G_SierraBlue.gltf","index":4}],"iblFilename":"","actionPromptDesktop":"Drag to rotate","actionPromptMobile":"Swipe to rotate"}'
    ></div>
```

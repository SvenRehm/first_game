<!-- <script context="module"> -->
<!--   export const prerender = true; -->
<!-- </script> -->

<script lang="ts">
  import { onMount } from 'svelte';
  import { Application, Sprite, Assets, Container, Graphics } from 'pixi.js';

  let app;

  onMount(async () => {
    const app = new Application();
    // await app.init({ background: '#242424', resizeTo: window });
    await app.init({ background: '#242424', width: 810, height: 610 });
    document.getElementById('content-div')?.appendChild(app.canvas);

    const gridContainer = new Container({ isRenderGroup: true });

    app.stage.addChild(gridContainer);
    const gridWidth = 800;
    const gridHeight = 600;
    let cellSize = 20;
    let isDragging = false;
    let dragData: any = null;
    let dropPosition = { x: 0, y: 0 };

    const draggableElement = document.getElementById('draggable');

    draggableElement?.addEventListener('dragstart', (e) => {
      isDragging = true;
      dragData = {
        width: draggableElement.offsetWidth,
        height: draggableElement.offsetHeight,
        color: 0xff0000 // Red color in hex
      };
    });

    app.canvas.addEventListener('dragover', (e) => {
      console.log(e.clientY, 'clienty');
      console.log(e.clientX, 'clientx');
      e.preventDefault();
      if (isDragging) {
        // Get position relative to canvas
        const rect = app.canvas.getBoundingClientRect();
        console.log(rect);
        dropPosition.x = e.clientX - rect.left;
        dropPosition.y = e.clientY - rect.top;
      }
    });

    const grid = createGrid(gridWidth, gridHeight, cellSize);

    grid.stroke({ width: 1, color: 'white' });
    gridContainer.addChild(grid);
    //   let isDragging = false;
    //   let previousPosition = { x: 0, y: 0 };
    //   app.canvas.addEventListener('mousedown', (e) => {
    //     isDragging = true;
    //     previousPosition.x = e.clientX;
    //     previousPosition.y = e.clientY;
    //   });
    //   window.addEventListener('mouseup', () => {
    //     isDragging = false;
    //   });
    //
    //   app.canvas.addEventListener('mousemove', (e) => {
    //     if (isDragging) {
    //       const dx = e.clientX - previousPosition.x;
    //       const dy = e.clientY - previousPosition.y;
    //
    //       gridContainer.x += dx;
    //       gridContainer.y += dy;
    //
    //       previousPosition.x = e.clientX;
    //       previousPosition.y = e.clientY;
    //     }
    //   });
    //
    //   app.canvas.removeEventListener('wheel', (e) => {});
    //   app.canvas.addEventListener('wheel', (e) => {
    //     e.preventDefault(); // Prevent the page from scrolling
    //
    //     let scaleFactor = e.deltaY < 0 ? 1.1 : 0.9; // Adjusting the zoom speed
    //     const mousePosition = { x: e.clientX, y: e.clientY };
    //     // Convert mouse position to the world container's local space
    //     const localPos = {
    //       x: (mousePosition.x - gridContainer.x) / gridContainer.scale.x,
    //       y: (mousePosition.y - gridContainer.y) / gridContainer.scale.y
    //     };
    //
    //     // Apply scale factor
    //     gridContainer.scale.x *= scaleFactor;
    //     gridContainer.scale.y *= scaleFactor;
    //
    //     console.log(gridContainer.scale.x);
    //     console.log(gridContainer.scale.y);
    //     // Adjust container position based on new scale
    //
    //     gridContainer.x = mousePosition.x - localPos.x * gridContainer.scale.x;
    //     gridContainer.y = mousePosition.y - localPos.y * gridContainer.scale.y;
    //   });

    app.canvas.addEventListener('drop', (e) => {
      e.preventDefault();
      if (isDragging && dragData) {
        let obj = new Graphics().rect(0, 0, 200, 100).fill(0xff0000);
        // Position the graphics object
        obj.x = dropPosition.x - dragData.width / 2;
        obj.y = dropPosition.y - dragData.height / 2;
        // obj.on('pointerdown', onDragStart, obj);

        obj.eventMode = 'static';
        app.stage.addChild(obj);
        // Reset drag state
        isDragging = false;
        dragData = null;
      }
    });
  });

  function createGrid(width: number, height: number, cellSize: number) {
    const grid = new Graphics();
    for (let x = 0; x <= width; x += cellSize) {
      grid.moveTo(x, 0);
      grid.lineTo(x, height);
    }

    for (let y = 0; y <= height; y += cellSize) {
      grid.moveTo(0, y);
      grid.lineTo(width, y);
    }

    grid.stroke({ width: 1, color: 'white' });
    grid.position.x = 1;
    grid.position.y = 1;
    return grid;
  }
</script>

<div class="container">
  <div id="draggable" draggable="true">Drag me!</div>
  <div id="content-div" style="display: flex;"></div>
</div>

<style>
  :global(body) {
    background: #222;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

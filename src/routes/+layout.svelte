<script>
  import Sidebar from "./Sidebar.svelte";
  import Header from './Header.svelte'
  import "../app.css";
</script>

<Sidebar />
<Header />
<div class="absolute left-16 right-0 bottom-0 top-16 min-h-full">
  <div class="p-8">
    <slot />
  </div>
</div>
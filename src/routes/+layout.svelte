<script>
  import Sidebar from "../components/Sidebar.svelte";
  import "../app.css";
</script>

<Sidebar />
<div class="absolute left-16">
  <slot />
</div>
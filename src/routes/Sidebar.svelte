<script>
    import SidebarIcon from "./SidebarIcon.svelte";
</script>

<div class="fixed top-0 left-0 h-screen w-16 flex flex-col bg-base-300 text-base-content
 shadow-lg z-10">
    <SidebarIcon dataTip="A">A</SidebarIcon>
    <SidebarIcon>B</SidebarIcon>
    <SidebarIcon dataTip="C">C</SidebarIcon>
</div>
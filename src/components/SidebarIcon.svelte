<script>
    export let dataTip = ""
</script>

<div class={!dataTip || "tooltip tooltip-right "} data-tip={dataTip}>
    <div class="sidebar-icon ">
        <slot />
    </div>
</div>

<style lang="postcss">
    .sidebar-icon {
        @apply relative flex items-center justify-center h-12 w-12 mx-auto m-2 shadow-lg bg-base-100 hover:bg-primary hover:text-primary-content rounded-3xl hover:rounded-xl hover:mask-squircle mask transition-all duration-300 ease-linear cursor-pointer;
    }
    .tooltip:hover:before,
    .tooltip:hover:after {
        @apply opacity-10
    }
</style>
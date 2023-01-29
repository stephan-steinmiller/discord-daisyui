<script>
    import Chat from './Chat.svelte'
    import Login from './Login.svelte'
    import { username } from '../stores/user'
</script>



{#if $username}
    <Chat />
{:else}
    <Login />
{/if}
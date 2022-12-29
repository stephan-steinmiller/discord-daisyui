<script>
    import Chat from './Chat.svelte'
    import Header from './Header.svelte'
    import Login from './Login.svelte'
    import { username } from '../stores/user'
</script>



<Header />
{#if $username}
    <Chat />
{:else}
    <Login />
{/if}
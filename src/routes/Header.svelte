<script>
  import Layout from '../routes/+layout.svelte';
    import { username, user } from '../stores/user'

    function signout() {
        user.leave()
        username.set('')
    }
</script>

{#if $username}
    <div class="user-bio">
        <span>Hello <strong>{$username}</strong></span>
    </div>
    <!-- <div class="avatar placeholder">
        <div class="bg-neutral-focus text-neutral-content rounded-full w-12">
          <span>MX</span>
        </div>
    </div>  -->
    <div class="avatar online">
        <div class="w-12 rounded-full">
          <img src="https://placeimg.com/192/192/people" />
        </div>
      </div>
{:else}
    Gun.js Chat
{/if}
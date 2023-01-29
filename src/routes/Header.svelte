<script>
    import { username, user } from '../stores/user'

    function signout() {
        user.leave()
        username.set('')
    }
</script>

<div class="fixed left-16 right-0 top-0 h-16 w-auto navbar bg-base-100 z-10">
  <div class="flex-1">
    <a class="btn btn-ghost normal-case text-xl">Discord daisyUI</a>
  </div>
  {#if $username}
     <div class="flex-none gap-2">
       <div class="form-control">
         <span>Hello <strong>{$username}</strong></span>
        </div>
        <div class="dropdown dropdown-end">
          <label tabindex="0" class="btn btn-ghost btn-circle avatar online">
            <div class="w-12 rounded-full">
              <img src="https://placeimg.com/192/192/people" />
            </div>
          </label>
          <ul tabindex="0" class="mt-3 p-2 shadow menu menu-compact dropdown-content bg-base-100 rounded-box w-52">
            <li>
              <a class="justify-between">
                Profile
                <span class="badge">New</span>
              </a>
            </li>
            <li><a>Settings</a></li>
            <li><button on:click={signout}>Logout</button></li>
          </ul>
        </div>
      </div>
      {/if}
    </div>
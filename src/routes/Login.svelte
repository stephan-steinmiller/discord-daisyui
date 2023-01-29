<script>
    import { user } from '../stores/user'

    let username
    let password

    function login() {
        user.auth(username, password, ({ err }) => err && alert(err))
    }
    function signup() {
        user.create(username, password, ({ err }) => {
            if(err) {
                alert(err)
            } else {
                login()
            }
        })
    }
</script>

<form class="flex flex-col gap-4 max-w-lg mx-auto">
    <label for="username">Username</label>
    <input type="text" placeholder="Enter in Username" name="username" bind:value={username} minlength="3" maxlength="16" class="input w-full" />
    <label for="password">Password</label>
    <input type="password" placeholder="Enter in Password" name="password" bind:value={password} minlength="8" maxlength="16" class="input w-full" />
    
    <button class="btn" on:click={login}>Login</button>
    <button class="btn" on:click={signup}>Signup</button>
</form>
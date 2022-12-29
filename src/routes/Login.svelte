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

<label for="username">Username</label>
<input type="text" name="username" bind:value={username} minlength="3" maxlength="16" />
<label for="password">Password</label>
<input type="password" name="password" bind:value={password} minlength="3" maxlength="16" />

<button class="login" on:click={login}>Login</button>
<button class="login" on:click={signup}>Signup</button>
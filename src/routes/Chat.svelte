<script>
    import ChatMessage from './ChatMessage.svelte'
    import { onMount } from 'svelte'
    import { username, user } from '../stores/user'

    import GUN from 'gun'
    const db = GUN()

    let newMessage
    let messages = []

    let scrollBottom
    let lastScrollTop
    let canAutoScroll = true
    let unreadMessages = false

    function autoScroll() {
      setTimeout(() => scrollBottom?.scrollIntoView({ behavior: 'auto' }), 50)
      unreadMessages = false
    }

    function watchScroll(e) {
      canAutoScroll = (e.target.scrollTop || Infinity) > lastScrollTop
      lastScrollTop = e.target.scrollTop
    }

    const debounce = (fn, delay) => {
        let timeoutID
        return function(...args) {
            timeoutID = setTimeout( () => {
                fn(...args)
            }, delay)
        }
    }

    $: debouncedWatchScroll = debounce(watchScroll, 1000)

    onMount(() => {
      var match = {
        // lexical queries are kind of like a limited RegEx or Glob.
        '.': {
          // property selector
          '>': new Date(+new Date() - 1 * 1000 * 60 * 60 * 3).toISOString(), // find any indexed property larger ~3 hours ago
        },
        '-': 1, // filter in reverse
      }

      // Get Messages
      db.get('chat')
        .map(match)
        .once(async (data, id) => {
          if (data) {
            // Key for end-to-end encryption
            const key = '#foo'

            var message = {
              // transform the data
              who: await db.user(data).get('alias'), // a user might lie who they are! So let the user system detect whose data it is.
              what: (await SEA.decrypt(data.what, key)) + '', // force decrypt as text.
              when: GUN.state.is(data, 'what'), // get the internal timestamp for the what property.
            }
            if (message.what) {
              messages = [...messages.slice(-100), message].sort((a, b) => a.when - b.when)
              if (canAutoScroll) {
                autoScroll()
              } else {
                unreadMessages = true
              }
            }
          }
        })
    })

    async function sendMessage() {
      const secret = await SEA.encrypt(newMessage, '#foo')
      const message = user.get('all').set({ what: secret })
      const index = new Date().toISOString()
      db.get('chat').get(index).put(message)
      newMessage = ''
      canAutoScroll = true
      autoScroll()
    }
</script>

<div class="container">
    <main>
        {#each messages as message (message.when)}
            <ChatMessage {message} sender={$username} date=""/>
        {/each}
        <div class="dummy" bind:this={scrollBottom} />
    </main>

    <form on:submit|preventDefault={sendMessage}>
        <input type="text" placeholder="Type here" bind:value={newMessage} class="input w-full max-w-xs" />
        <button type="submit" disabled={!newMessage} class="btn">Send</button>
    </form>
</div>
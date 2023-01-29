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

  let messageContainer
  let form
  
  function bindScrollHeight() {
    console.log(textarea.scrollHeight)
    textarea.style.height = 'auto'
    textarea.style.height = textarea.scrollHeight + 'px'
  }
  function bindScrollHeightOnEvent(e) {
    textarea = e.target
    textarea.style.height = 'auto'
    textarea.style.height = textarea.scrollHeight + 'px'
    messageContainer.style.marginBottom = textarea.scrollHeight + 'px'
  }

  const handleTextAreaKeydown = event => {
    if (event.key === 'Enter' && newMessage && !event.shiftKey) {
      // Submit the form
      event.preventDefault();
      event.target.form.submit()
    }
  }
</script>

<!-- <div class=""> -->
    <div bind:this={messageContainer} class="min-h-full flex flex-col justify-end">
        {#each messages as message (message.when)}
            <ChatMessage {message} user={$username} />
        {/each}
        <div class="dummy" bind:this={scrollBottom} />
    </div>

    <form ref="form" on:submit|preventDefault={sendMessage} class="fixed bottom-0 left-16 right-0 p-8 pt-2 flex bg-base-300">
        <textarea on:input={bindScrollHeightOnEvent} on:keydown={newMessage ? handleTextAreaKeydown : () => {}} type="text" placeholder="Type here" bind:value={newMessage} contenteditable autofocus rows="1" class="textarea flex-auto resize-none overflow-y-auto h-auto" />
        <button type="submit" disabled={!newMessage.trim()} class="btn btn-primary">Send</button>
    </form>
<!-- </div> -->

<style>
</style>
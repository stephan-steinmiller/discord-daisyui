<script>
    export let message
    export let user

    const ts = new Date(message.when)
    const hours = (ts.getHours())
    const minutes = (ts.getMinutes())

    const makeTwoDigits = x => (n<=9 ? "0"+x : x)

    $: initials = message.who.slice(0, 1)
    
    const isSentMessage = message.who === user
    const avatar = "https://placeimg.com/192/192/people"
    // const avatar = `https://avatars.dicebear.com/api/initials/${message.who}.svg`
</script>

<div class="chat chat-{isSentMessage ? "end" : "start"}">
    <div class="chat-image avatar placeholder">
        <div class="w-10 rounded-full bg-neutral-focus text-neutral-content ">
            <img class="text-3xl" src={avatar} alt={initials}/>
        </div>
    </div>
    <div class="chat-header">
        {message.who}
        <time class="text-xs opacity-50">{hours}:{minutes}</time>
    </div>
    <div class:chat-bubble-primary={isSentMessage} class="chat-bubble break-words">{message.what}</div>
</div>

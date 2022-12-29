<script>
    export let sender
    $: initials = sender.slice(0, 1)
</script>

<div class="chat chat-end">
    <div class="chat-image avatar placeholder">
        <div class="w-10 rounded-full bg-neutral-focus text-neutral-content ">
            <img class="text-3xl" src="https://placeimg.com/192/192/people" alt={initials}/>
        </div>
    </div>
    <div class="chat-header">
        {sender}
        <time class="text-xs opacity-50">time</time>
        </div>
    <div class="chat-bubble">{message}</div>
</div>
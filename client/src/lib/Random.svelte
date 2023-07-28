<script lang="ts">
    let random: number = -1;

    function getRandom() {
        fetch('./random')
        .then(response => response.text())
        .then(text => random = parseFloat(text));
    }
</script>

<h2>Your random digit is {random}</h2>
<button on:click={getRandom}>
    Get a random digit
</button>
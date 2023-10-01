<script>
    let username = "";
    let token = "";

    const handleSubmit = () => {
        console.log("form test");
    };
</script>

<div class="main">
    <form class="login-form" on:submit|preventDefault={handleSubmit}>
        <div class="form-row">
            <input bind:value={username} type="text" name="username" id="login-username" placeholder="Username">
        </div>
        <div class="form-row">
            <input bind:value={token} type="text" name="token" id="login-token" placeholder="Token">
        </div>
        <div class="form-row">
            <input type="submit" value="Log-in">
        </div>
    </form>
</div>
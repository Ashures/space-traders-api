<script>
    let username = "";
    let token = "";

    const createAgent = async () => {
        const options = {
            "method": "POST",
            "headers": {
                "Content-Type": "application/json",
            },
            "body": JSON.stringify({
                "symbol": username,
                "faction": "COSMIC",
            }),
        };

        const res = await fetch("https://api.spacetraders.io/v2/register", options);
        
        if (!res.ok) {
            console.log("User cannot be created!");
            return;
        }

        const data = await res.json();

        return data.data;
    };

    const handleSubmit = async () => {
        console.log(username, token);

        const agent = await createAgent(username);

        if (!agent) return;

        token = agent.token;

        console.log("Successfully created new agent!");
    };
</script>

<form class="register-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-row">
        <input bind:value={username} type="text" name="username" id="register-username" placeholder="Username">
    </div>
    <div class="form-row">
        <input bind:value={token} type="text" name="token" id="register-token" placeholder="Token" readonly>
    </div>
    <div class="form-row">
        <input type="submit" value="Register">
    </div>
</form>
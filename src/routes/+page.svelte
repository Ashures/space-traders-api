<script>
    import LoginForm from "$lib/LoginForm.svelte";

    let username = "";
    let token = "";

    const getAgentInfo = async (userToken) => {
        const options = {
            "headers": {
                "Content-Type": "application/json",
                "Authorization": `Bearer ${userToken}`,
            }
        };

        const res = await fetch("https://api.spacetraders.io/v2/my/agent", options);
        
        if (!res.ok) {
            console.log("User not found!");
            return;
        }

        const data = res.json();

        return data.data;
    };

    const handleSubmit = async () => {
        console.log(username, token);

        const agent = await getAgentInfo(token);

        if (!agent) return;

        if (agent.symbol !== username) {
            console.log("Invalid username!");
            return;
        }

        login();
    };
</script>

<div class="main">
    <LoginForm />

    <form class="register-form" on:submit|preventDefault={handleSubmit}>
        <div class="form-row">
            <input bind:value={username} type="text" name="username" id="register-username" placeholder="Username">
        </div>
        <div class="form-row">
            <input bind:value={token} type="password" name="token" id="register-token" placeholder="Token" readonly>
        </div>
        <div class="form-row">
            <input type="submit" value="Register">
        </div>
    </form>
</div>
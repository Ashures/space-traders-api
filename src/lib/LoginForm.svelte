<script>
    import { onMount } from "svelte";

    let username = "";
    let token = "";

    const skipLogin = () => {
        const cookies = document.cookie;

        if (cookies.includes("token=")) {
            window.location.href = "/home";
        }
    }

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

        const data = await res.json();

        return data.data;
    };

    const login = () => {
        document.cookie = `token=${token}`;

        window.location.href = "/home";
    }

    const handleSubmit = async () => {
        console.log(username, token);

        const agent = await getAgentInfo(token);

        if (!agent) return;

        if (agent.symbol !== username.toUpperCase()) {
            console.log("Invalid username!");
            return;
        }

        login();
    };

    onMount(async () => {
        skipLogin();
    });
</script>

<form class="login-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-row">
        <input bind:value={username} type="text" name="username" id="login-username" placeholder="Username">
    </div>
    <div class="form-row">
        <input bind:value={token} type="password" name="token" id="login-token" placeholder="Token">
    </div>
    <div class="form-row">
        <input type="submit" value="Log-in">
    </div>
</form>
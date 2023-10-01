<script>
    import { onMount } from "svelte";

    let token = "";
    let agentInfo;

    const getCookie = (cookieName) => {
        const cookies = document.cookie;
        const cookieList = cookies.split(";");

        let desiredCookie;
        cookieList.forEach(c => { if (c.includes(`${cookieName}=`)) desiredCookie = c.slice(c.indexOf("=") + 1); });

        return desiredCookie;
    };

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

        return await data.data;
    };

    onMount(async () => {
        token = getCookie("token");

        if (!token) {
            window.location.href = "/";
            return;
        }

        agentInfo = await getAgentInfo(token);
    });
</script>

{#if !agentInfo}
    <h1>Loading...</h1>
{:else}
    <h1>Welcome, {agentInfo.symbol}</h1>
    <ul>
        <li>name: {agentInfo.symbol}</li>
        <li>credits: {agentInfo.credits}</li>
        <li>ships: {agentInfo.shipCount}</li>
        <li>headquarters: {agentInfo.headquarters}</li>
        <li>starting faction: {agentInfo.startingFaction}</li>
    </ul>
{/if}
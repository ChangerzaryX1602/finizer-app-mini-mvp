<script>
    import { onMount } from 'svelte';
import { goto } from '$app/navigation'
    // Define form fields
    let name = "";
    let gender = "";
    let birthdate = "";
    let forgotTime = false;

    // Function to handle form submission
    const updateUserData = async () => {
        try {
            // Retrieve token from localStorage
            const token = localStorage.getItem('token');
            if (!token) {
                console.error("No token found in localStorage.");
                return;
            }

            // Adjust birthdate if "forgotTime" is checked
            let formattedBirthdate = forgotTime ? `${birthdate.split('T')[0]}T00:00:00+07:00` : birthdate+"+07:00";

            // Prepare request body
            const body = {
                name,
                gender,
                birthdate: formattedBirthdate
            };

            // Send PUT request
            const response = await fetch("https://app.finizer.co/api/v1/users/me", {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${token}`
                },
                body: JSON.stringify(body)
            });

            if (response.ok) {
                console.log("User data updated successfully.");
                goto("/home");
            } else {
                console.error("Failed to update user data:", response.status);
            }
        } catch (error) {
            console.error("Error updating user data:", error);
        }
    };
</script>

<style>
    form {
        max-width: 1000px;
        margin: 0 auto;
        padding: 1em;
        justify-content: center;
        align-items: center;
    }
    label {
        display: block;
        margin-bottom: 0.5em;
    }
</style>

<!-- Form Structure -->
<form on:submit|preventDefault={updateUserData}>
    <label>
        Name:
        <input type="text" bind:value={name} required />
    </label>
    <label>
        Gender:
        <input type="text" bind:value={gender} required />
        Male/Female
    </label>
    <label>
        Birthdate:
        <input type="datetime" bind:value={birthdate} required />
        <br>
        Format: YYYY-MM-DDTHH:MM:SS
        If you forgot the time, please check the box below and Enter format YYYY-MM-DDT
    </label>
    <label>
        <input type="checkbox" bind:checked={forgotTime} />
        Forgot birthdate-time?
    </label>

    <button type="submit">Update</button>
</form>

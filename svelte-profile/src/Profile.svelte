<script>
    export let user;

    const profileImage = user.profile_image || 'default-profile-image.png'; // Specify a default image if none is provided
</script>

<div class="card">
    <div class="card-body">
        <div class="avatar avatar-lg">
            <img src={profileImage} alt="{user.first_name_en}" class="avatar-img">
        </div>
        <h3>{user.first_name} {user.last_name}</h3>
        <p>{user.type} at {user.faculty}</p>
        <p><strong>Email:</strong> {user.email}</p>
        <p><strong>Gender:</strong> {user.gender}</p>
        <p><strong>BMI:</strong> {user.bmi}</p>
        <p><strong>Created At:</strong> {new Date(user.created_at).toLocaleString()}</p>
        <p><strong>Updated At:</strong> {new Date(user.updated_at).toLocaleString()}</p>
    </div>
</div>

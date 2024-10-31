<script>
    import { onMount } from 'svelte';

    // Define rows data
    let rows = [
        { type:"heavenly_stem", hour: "", day: "", month: "", year: "" },
        { type:"earthly_branch",hour: "", day: "", month: "", year: "" }
    ];
    let rows_2 = [
        {type:"hard_fovorable",element:"",fovorable:"",unfovorable:""},
        {type:"hard_fovorable",element:"",fovorable:"",unfovorable:""},
        {type:"soft_fovorable",element:"",fovorable:"",unfovorable:""},
        {type:"soft_fovorable",element:"",fovorable:"",unfovorable:""},
    ];
    // Function to fetch data with Authorization header
    const fetchBaziData = async () => {
        try {
            // Retrieve token from localStorage
            const token = localStorage.getItem('token');

            // Make sure token is available
            if (!token) {
                console.error("No token found in localStorage.");
                return;
            }

            // Fetch data with Authorization header
            const response = await fetch("https://app.finizer.co/api/v1/bazi/me", {
                headers: {
                    'Authorization': `Bearer ${token}`
                }
            });
                const data = await response.json();
                rows[0]["hour"] = data.result.four_pillar.hour_pillar.heavenly_stem.name;  // Update rows with fetched data
                rows[0]["day"] = data.result.four_pillar.day_pillar.heavenly_stem.name;  // Update rows with fetched data
                rows[0]["month"] = data.result.four_pillar.month_pillar.heavenly_stem.name;  // Update rows with fetched data
                rows[0]["year"] = data.result.four_pillar.year_pillar.heavenly_stem.name;  // Update rows with fetched data
                rows[1]["hour"] = data.result.four_pillar.hour_pillar.earthly_branch.element_name+"("+data.result.four_pillar.hour_pillar.earthly_branch.name+")";  // Update rows with fetched data
                rows[1]["day"] = data.result.four_pillar.day_pillar.earthly_branch.element_name+"("+data.result.four_pillar.day_pillar.earthly_branch.name+")";  // Update rows with fetched data
                rows[1]["month"] = data.result.four_pillar.month_pillar.earthly_branch.element_name+"("+data.result.four_pillar.month_pillar.earthly_branch.name+")";  // Update rows with fetched data
                rows[1]["year"] = data.result.four_pillar.year_pillar.earthly_branch.element_name+"("+data.result.four_pillar.year_pillar.earthly_branch.name+")";  // Update rows with fetched data
                rows_2[0]["element"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[0].hard_element;
                rows_2[0]["fovorable"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[0].favorable_element;
                rows_2[0]["unfovorable"] = data.result.hard_favorable_unfavorable.hard_unfovorable_map[0].unfavorable_element;
                rows_2[1]["element"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[1].hard_element;
                rows_2[1]["fovorable"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[1].favorable_element;
                rows_2[1]["unfovorable"] = data.result.hard_favorable_unfavorable.hard_unfovorable_map[1].unfavorable_element;
                rows_2[1]["element"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[1].hard_element;
                rows_2[1]["fovorable"] = data.result.hard_favorable_unfavorable.hard_fovorable_map[1].favorable_element;
                rows_2[1]["unfovorable"] = data.result.hard_favorable_unfavorable.hard_unfovorable_map[1].unfavorable_element;
                rows_2[2]["element"] = data.result.soft_favorable_unfavorable.soft_fovorable_map[0].soft_element;
                rows_2[2]["fovorable"] = data.result.soft_favorable_unfavorable.soft_fovorable_map[0].favorable_element;
                rows_2[2]["unfovorable"] = data.result.soft_favorable_unfavorable.soft_unfovorable_map[0].unfavorable_element;
                rows_2[3]["element"] = data.result.soft_favorable_unfavorable.soft_fovorable_map[1].soft_element;
                rows_2[3]["fovorable"] = data.result.soft_favorable_unfavorable.soft_fovorable_map[1].favorable_element;
                rows_2[3]["unfovorable"] = data.result.soft_favorable_unfavorable.soft_unfovorable_map[1].unfavorable_element;
        } catch (error) {
            console.error("Error fetching data:", error);
        }
    };

    // Fetch data on component mount
    onMount(() => {
        fetchBaziData();
    });
</script>

<style>
    table {
        width: 50%;
        border-collapse: collapse;
        margin: 20px auto;
    }
    th, td {
        border: 1px solid #333;
        padding: 10px;
        text-align: center;
    }
    th {
        background-color: #f2f2f2;
    }
</style>

<table>
    <thead>
        <tr>
            <th>Hour</th>
            <th>Day</th>
            <th>Month</th>
            <th>Year</th>
            <th>Type</th>
        </tr>
    </thead>
    <tbody>
        {#each rows as row}
            <tr>
                <td>{row.hour}</td>
                <td>{row.day}</td>
                <td>{row.month}</td>
                <td>{row.year}</td>
                <td style="color: brown">{row.type}</td>
            </tr>
        {/each}
    </tbody>
    <br>
    <br>
    <thead>
        <tr>
            <th>Element</th>
            <th>Favorable</th>
            <th>Unfavorable</th>
            <th>Life_element</th>
        </tr>
    </thead>
    <tbody>
        {#each rows_2 as row}
            <tr>
                <td>{row.element}</td>
                <td>{row.fovorable}</td>
                <td>{row.unfovorable}</td>
                <td style="color: brown">{row.type}</td>
            </tr>
        {/each}
    </tbody>
</table>

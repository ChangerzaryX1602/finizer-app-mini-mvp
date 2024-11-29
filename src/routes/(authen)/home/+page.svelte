<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  const branchRankings = writable([]);

  let ws;

  onMount(() => {
      ws = new WebSocket('wss://app.finizer.co/api/v1/business/ws');

      ws.onmessage = (event) => {
          try {
              const data = JSON.parse(event.data);
              processBranchData(data);
          } catch (err) {
              console.error('Error parsing WebSocket data:', err);
          }
      };

      return () => {
          if (ws) {
              ws.close();
          }
      };
  });

  function processBranchData(data) {
      const branchSums = data.reduce((acc, transaction) => {
          const branch = transaction.branch_name;
          const amount = parseFloat(transaction.amount);
          if (!isNaN(amount)) {
              acc[branch] = (acc[branch] || 0) + amount;
          }
          return acc;
      }, {});

      const rankings = Object.entries(branchSums)
          .map(([branch, totalAmount]) => ({ branch, totalAmount }))
          .sort((a, b) => b.totalAmount - a.totalAmount);

      branchRankings.set(rankings);
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

  .ranking {
      margin: 2em auto;
      width: 90%;
      max-width: 800px;
      font-family: 'Roboto', sans-serif;
  }
  table {
      width: 100%;
      border-collapse: collapse;
      margin: 1em 0;
      font-size: 1rem;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
      overflow: hidden;
  }
  th, td {
      border: 1px solid #ddd;
      padding: 12px;
      text-align: center;
  }
  th {
      background-color: #4CAF50;
      color: white;
      font-weight: bold;
      text-transform: uppercase;
  }
  tr:nth-child(even) {
      background-color: #f9f9f9;
  }
  tr:hover {
      background-color: #f1f1f1;
      transform: scale(1.01);
      transition: transform 0.2s ease-in-out;
  }
  td {
      position: relative;
  }
  td::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      transition: box-shadow 0.2s ease-in-out;
  }
  tr:hover td::after {
      box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.2);
  }
  h2 {
      text-align: center;
      font-size: 2rem;
      color: #333;
      margin-bottom: 1.5em;
  }
  .branch-item {
      margin-bottom: 0.5em;
  }
</style>

<div class="ranking">
  <h2>ลำดับยอดขาย</h2>
  <table>
      <thead>
          <tr>
              <th>ลำดับ</th>
              <th>ชื่อร้าน</th>
              <th>ยอดเงิน</th>
          </tr>
      </thead>
      <tbody>
          {#each $branchRankings as { branch, totalAmount }, index}
              <tr>
                  <td>{index + 1}</td>
                  <td>{branch}</td>
                  <td>{totalAmount.toFixed(2)}</td>
              </tr>
          {/each}
      </tbody>
  </table>
</div>

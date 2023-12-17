<script>
    // import Icon from "@iconify/svelte";
    let expenses = [];
    let description = "";
    let amount = "";
    let date = "";
    function addExpense() {
      if (description && amount && date) {
        expenses = [
          ...expenses,
          { description, amount: parseFloat(amount), date },
        ];
        description = "";
        amount = "";
        date = "";
      }
      localStorage.setItem("expenses", JSON.stringify(expenses));
    }
  
    const storedExpenses = localStorage.getItem("expenses");
    expenses = storedExpenses ? JSON.parse(storedExpenses) : [];
  
    function deleteEntry(i) {
      expenses.splice(i, 1);
      localStorage.setItem("expenses", JSON.stringify(expenses));
      expenses = [...expenses];
    }
  </script>
  
  <main>
    <h1>Expensipy : Tracking Made Simpler</h1>
  
    <div class="main-content">
      <div>
        <h2>Where did you spend?</h2>
        <form action="" on:submit|preventDefault>
          <label for="amount">Amount</label>
          <input
            type="text"
            id="amount"
            name="amount"
            bind:value={amount}
            placeholder="How much you spent today?"
          />
  
          <label for="description">Description</label>
          <input
            type="text"
            name="description"
            id="description"
            placeholder="Where did the money go?"
            bind:value={description}
          />
  
          <label for="date">Date</label>
          <input
            type="date"
            name="date"
            id="date"
            bind:value={date}
            placeholder="Mark this date"
          />
  
          <button type="submit" class="add-expense" on:click={addExpense}
            >Add Expense</button
          >
        </form>
      </div>
  
      <div class="display-table">
        <h2>Expenses</h2>
  
        <div class="table-result">
          <table>
            <thead>
              {#if expenses.length > 0}
                <tr>
                  <th>Ser No.</th>
                  <th>Amount</th>
                  <th>Description</th>
                  <th>Date</th>
                  <th>Delete</th>
                </tr>
              {/if}
            </thead>
  
            <tbody>
              {#if expenses.length > 0}
                {#each expenses as expense, i}
                  <tr>
                    <td>{i + 1}</td>
                    <td>{expense.amount}</td>
  
                    {#if expense.description}
                      <td>{expense.description}</td>
                    {:else}
                      <td>No description</td>
                    {/if}
  
                    {#if expense.date}
                      <td>{expense.date}</td>
                    {:else}
                      <td>Can't remember</td>
                    {/if}
                    <td>
                      <button
                        class="btn-delete"
                        on:click={() => {
                          deleteEntry(i);
                        }}
                      >
                        Del
                        <!-- <Icon icon="fluent:delete-12-filled"
                      ></Icon> -->
                      </button>
                    </td>
                  </tr>
                {/each}
              {:else}
                <tr>
                  <td colspan="3">No expenses yet.</td>
                </tr>
              {/if}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </main>
  
  <style>
    :global(body) {
      font-family: sans-serif;
      background: radial-gradient(
        circle at top right,
        #7a5cff 0%,
        rgb(38, 108, 247) 20%,
        #4780eb 35%,
        #93e4ff 100%
      );
      color: #333;
      font-weight: 550;
    }
    .main-content {
      display: flex;
      justify-content: center;
      gap: 8rem;
    }
    main {
      max-width: 1200px;
      margin: 0 auto;
    }
  
    h1 {
      text-align: center;
      color: #0a0032;
      font-weight: 600;
      font-family: Verdana, Geneva, Tahoma, sans-serif;
    }
  
    form {
      display: flex;
      flex-direction: column;
      width: 30rem;
    }
  
    label {
      margin-top: 1rem;
    }
  
    input {
      margin-top: 0.5rem;
      font-style: italic;
    }
  
    button {
      margin-top: 1rem;
      cursor: pointer;
    }
    .table-result {
      overflow-y: scroll;
      height: 18.5rem;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    .btn-delete {
      outline: none;
      background: transparent;
      border: none;
      cursor: pointer;
      color: #333;
    }
    .btn-delete:hover {
      color: #111;
    }
  
    th,
    td {
      text-align: center;
      border: 1px solid #ccc;
      background-color: white;
    }
  
    th {
      background-color: #eee;
      padding: 0.5rem;
    }
    #amount,
    #description,
    #date {
      background: transparent;
    }
    .add-expense:hover {
      background-color: #2b2b2b;
    }
    .display-table {
      width: 30rem;
    }
  
    @media (max-width: 600px) {
      table {
        border: 0;
      }
  
      table thead {
        display: none;
      }
  
      table tr {
        margin-bottom: 0.5rem;
        display: block;
        border: 1px solid #ccc;
      }
  
      table td {
        display: block;
        text-align: right;
        padding-left: 50%;
        position: relative;
      }
  
      table td::before {
        content: attr(data-label);
        position: absolute;
        left: 0;
        width: 50%;
        padding-left: 15px;
        font-size: 15px;
        font-weight: bold;
        text-align: left;
      }
    }
  </style>
  
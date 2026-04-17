<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let user;

  let activeTab = 'verification';
  let scannedCode = '';
  let inventoryAlerts = [];
  let showBarcodeScan = false;

  // Mock data
  const verificationRequests = [
    { id: 1, studentNum: '20190034', name: 'Juan Dela Cruz', email: 'juan@ateneo.edu.ph', date: '2026-01-15', status: 'Pending', batchYear: 2020 },
    { id: 2, studentNum: '20180045', name: 'Maria Santos', email: 'maria@ateneo.edu.ph', date: '2026-01-14', status: 'Verified', batchYear: 2019 },
    { id: 3, studentNum: '20210023', name: 'Carlos Reyes', email: 'carlos@ateneo.edu.ph', date: '2026-01-13', status: 'Under Review', batchYear: 2021 }
  ];

  const documentLogs = [
    { id: 'LOG-001', barcode: '202601-TOR-0001', doc: 'Transcript of Records', issued: '2026-01-15', status: 'Issued' },
    { id: 'LOG-002', barcode: '202601-DIPLOMA-0001', doc: 'Copy of Diploma', issued: '2026-01-14', status: 'Pending Pickup' },
    { id: 'LOG-003', barcode: '202601-GMC-0001', doc: 'Good Moral Character', issued: '2026-01-13', status: 'Mailed' }
  ];

  const paymentRecords = [
    { id: 'PAY-001', alumni: 'Juan Dela Cruz', amount: 150, date: '2026-01-15', refCode: 'ADDU-562847', status: 'Confirmed', doc: 'TOR' },
    { id: 'PAY-002', alumni: 'Maria Santos', amount: 650, date: '2026-01-14', refCode: 'ADDU-719284', status: 'Confirmed', doc: 'Diploma + Rush' },
    { id: 'PAY-003', alumni: 'Carlos Reyes', amount: 100, date: '2026-01-13', refCode: 'ADDU-384521', status: 'Pending', doc: 'GMC' }
  ];

  const inventory = [
    { name: 'Diploma Paper', stock: 150, warning: 500, status: 'good' },
    { name: 'TOR Blank Forms', stock: 80, warning: 200, status: 'warning' },
    { name: 'Certification Sheets', stock: 45, warning: 100, status: 'critical' },
    { name: 'Envelopes (Official)', stock: 1200, warning: 500, status: 'good' }
  ];

  // Filter and sort states
  let filterBatchYear = '';
  let filterStatus = '';
  let filterPaymentStatus = '';

  $: filteredRequests = verificationRequests.filter(req => {
    return (!filterBatchYear || req.batchYear == filterBatchYear) &&
           (!filterStatus || req.status === filterStatus);
  });

  $: filteredPayments = paymentRecords.filter(payment => {
    return !filterPaymentStatus || payment.status === filterPaymentStatus;
  });

  $: criticalInventory = inventory.filter(item => item.stock < item.warning);

  function handleBarcodeScan(barcode) {
    const log = documentLogs.find(l => l.barcode === barcode);
    if (log) {
      alert(`✓ Document logged: ${log.doc}\nBarcode: ${barcode}\nStatus: ${log.status}`);
    } else {
      alert('❌ Barcode not found in system');
    }
  }

  function verifyAlumni(req) {
    alert(`✓ Alumni verified: ${req.name}\nEmail confirmed: ${req.email}`);
  }

  function rejectRequest(req) {
    alert(`⚠ Request rejected for ${req.name}\nNotification sent to their email`);
  }

  function generateReorderAlert() {
    alert('📧 Reorder alerts have been sent to procurement!\nItems below threshold:\n- TOR Blank Forms\n- Certification Sheets');
  }

  function assignReferenceCode() {
    alert(`✓ Reference Code Assigned: ADDU-${Math.floor(Math.random() * 1000000)}\n✓ Payment matched to alumni profile`);
  }

  function matchPayment(payment) {
    alert(`✓ Payment Successfully Matched!\nAlumni: ${payment.alumni}\nAmount: ₱${payment.amount}\nReference: ${payment.refCode}`);
  }
</script>

<div class="staff-container">
  <!-- Tab Navigation -->
  <div class="staff-tabs">
    <button 
      class={activeTab === 'verification' ? 'active' : ''} 
      on:click={() => activeTab = 'verification'}
    >
      ✓ Verification Dashboard
    </button>
    <button 
      class={activeTab === 'doclog' ? 'active' : ''}
      on:click={() => activeTab = 'doclog'}
    >
      📋 Document Log
    </button>
    <button 
      class={activeTab === 'inventory' ? 'active' : ''}
      on:click={() => activeTab = 'inventory'}
    >
      📦 Inventory Alerts
    </button>
    <button 
      class={activeTab === 'payment' ? 'active' : ''}
      on:click={() => activeTab = 'payment'}
    >
      💳 Payment Verification
    </button>
  </div>

  <!-- Verification Dashboard Tab -->
  {#if activeTab === 'verification'}
    <div class="tab-content">
      <h3>Alumni Verification Dashboard</h3>
      <p class="tab-subtitle">Manage and verify all alumni registration requests</p>

      <!-- Filters -->
      <div class="filters">
        <div class="filter-group">
          <label for="filterBatch">Filter by Batch Year:</label>
          <select id="filterBatch" bind:value={filterBatchYear}>
            <option value="">All Years</option>
            <option value="2020">2020</option>
            <option value="2019">2019</option>
            <option value="2021">2021</option>
            <option value="2022">2022</option>
          </select>
        </div>

        <div class="filter-group">
          <label for="filterVerStatus">Filter by Status:</label>
          <select id="filterVerStatus" bind:value={filterStatus}>
            <option value="">All Statuses</option>
            <option value="Pending">Pending</option>
            <option value="Verified">Verified</option>
            <option value="Under Review">Under Review</option>
          </select>
        </div>
      </div>

      <!-- Verification Requests Table -->
      <div class="table-container">
        <table class="staff-table">
          <thead>
            <tr>
              <th>Student Number</th>
              <th>Name</th>
              <th>Email</th>
              <th>Request Date</th>
              <th>Batch Year</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {#each filteredRequests as req}
              <tr>
                <td class="mono">{req.studentNum}</td>
                <td>{req.name}</td>
                <td>{req.email}</td>
                <td>{req.date}</td>
                <td>{req.batchYear}</td>
                <td>
                  <span class="status-badge" class:pending={req.status === 'Pending'} class:verified={req.status === 'Verified'} class:review={req.status === 'Under Review'}>
                    {req.status}
                  </span>
                </td>
                <td class="actions">
                  {#if req.status !== 'Verified'}
                    <button class="action-btn verify" on:click={() => verifyAlumni(req)}>✓ Verify</button>
                  {/if}
                  {#if req.status === 'Pending'}
                    <button class="action-btn reject" on:click={() => rejectRequest(req)}>✕ Reject</button>
                  {/if}
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>

      <div class="summary-box">
        <h4>Dashboard Summary</h4>
        <div class="summary-items">
          <div class="summary-item">
            <span class="label">Total Requests:</span>
            <span class="value">{verificationRequests.length}</span>
          </div>
          <div class="summary-item">
            <span class="label">Verified:</span>
            <span class="value">{verificationRequests.filter(r => r.status === 'Verified').length}</span>
          </div>
          <div class="summary-item">
            <span class="label">Pending:</span>
            <span class="value">{verificationRequests.filter(r => r.status === 'Pending').length}</span>
          </div>
        </div>
      </div>
    </div>

  <!-- Document Log Tab -->
  {:else if activeTab === 'doclog'}
    <div class="tab-content">
      <h3>Document Logging System</h3>
      <p class="tab-subtitle">Scan barcodes to log documents without typing</p>

      <!-- Barcode Scanner -->
      <div class="scanner-section">
        <div class="scanner-card">
          <h4>📱 Barcode Scanner</h4>
          <div class="scanner-input-area">
            <input 
              type="text" 
              class="barcode-input"
              placeholder="Scan or enter barcode here..."
              bind:value={scannedCode}
              on:keypress={(e) => {
                if (e.key === 'Enter') {
                  handleBarcodeScan(scannedCode);
                  scannedCode = '';
                }
              }}
              autofocus
            />
            <button class="btn-scan" on:click={() => handleBarcodeScan(scannedCode)}>
              📸 Scan
            </button>
          </div>
          <p class="scanner-help">Example: 202601-TOR-0001</p>
        </div>
      </div>

      <!-- Document Log Table -->
      <h4 style="margin-top: 2rem;">Recent Document Logs</h4>
      <div class="table-container">
        <table class="staff-table">
          <thead>
            <tr>
              <th>Log ID</th>
              <th>Barcode</th>
              <th>Document Type</th>
              <th>Issued Date</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            {#each documentLogs as log}
              <tr>
                <td class="mono">{log.id}</td>
                <td class="mono">{log.barcode}</td>
                <td>{log.doc}</td>
                <td>{log.issued}</td>
                <td>
                  <span class="status-badge" class:issued={log.status === 'Issued'} class:pending={log.status === 'Pending Pickup'} class:mailed={log.status === 'Mailed'}>
                    {log.status}
                  </span>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>

      <div class="info-box">
        <p>💡 Benefits: Reduces typing errors, speeds up logging during busy periods (enrollment, graduation)</p>
      </div>
    </div>

  <!-- Inventory Alerts Tab -->
  {:else if activeTab === 'inventory'}
    <div class="tab-content">
      <h3>Inventory Management</h3>
      <p class="tab-subtitle">Monitor supplies and get alerts when stock is low</p>

      <!-- Critical Alerts -->
      {#if criticalInventory.length > 0}
        <div class="alerts-section">
          <h4>⚠️ Critical Inventory Alerts</h4>
          <div class="alert-cards">
            {#each criticalInventory as item}
              <div class="alert-card critical">
                <div class="alert-icon">⚠️</div>
                <div class="alert-content">
                  <div class="alert-title">{item.name}</div>
                  <div class="alert-details">
                    Current: {item.stock} | Threshold: {item.warning}
                  </div>
                </div>
                <button class="btn-reorder" on:click={generateReorderAlert}>🛒 Reorder</button>
              </div>
            {/each}
          </div>
        </div>
      {/if}

      <!-- Full Inventory Status -->
      <h4 style="margin-top: 2rem;">Full Inventory Status</h4>
      <div class="inventory-grid">
        {#each inventory as item}
          <div class="inventory-card" class:critical={item.stock < item.warning} class:warning={item.stock < item.warning * 1.5 && item.stock >= item.warning}>
            <div class="inventory-header">
              <span class="item-name">{item.name}</span>
              <span class="status-icon">
                {#if item.stock < item.warning}
                  ❌
                {:else if item.stock < item.warning * 1.5}
                  ⚠️
                {:else}
                  ✓
                {/if}
              </span>
            </div>
            <div class="stock-info">
              <div class="current">
                <span class="label">Current Stock:</span>
                <span class="value">{item.stock} units</span>
              </div>
              <div class="threshold">
                <span class="label">Reorder Threshold:</span>
                <span class="value">{item.warning} units</span>
              </div>
            </div>
            <div class="progress-bar">
              <div class="progress-fill" style="width: {Math.min((item.stock / item.warning) * 100, 100)}%"></div>
            </div>
            {#if item.stock < item.warning}
              <button class="btn-reorder" on:click={generateReorderAlert}>🛒 Reorder Now</button>
            {/if}
          </div>
        {/each}
      </div>

      <div class="info-box">
        <p>💡 Automatic alerts prevent stock-outs during peak periods like graduation or enrollment</p>
      </div>
    </div>

  <!-- Payment Verification Tab -->
  {:else if activeTab === 'payment'}
    <div class="tab-content">
      <h3>Payment Verification & Reference Codes</h3>
      <p class="tab-subtitle">Track payments and assign unique reference codes</p>

      <!-- Filter -->
      <div class="filters">
        <div class="filter-group">
          <label for="filterPayStatus">Filter by Status:</label>
          <select id="filterPayStatus" bind:value={filterPaymentStatus}>
            <option value="">All Statuses</option>
            <option value="Confirmed">Confirmed</option>
            <option value="Pending">Pending</option>
          </select>
        </div>
      </div>

      <!-- Payment Table -->
      <div class="table-container">
        <table class="staff-table">
          <thead>
            <tr>
              <th>Reference Code</th>
              <th>Alumni Name</th>
              <th>Document</th>
              <th>Amount</th>
              <th>Date</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {#each filteredPayments as payment}
              <tr>
                <td class="mono ref-code">{payment.refCode}</td>
                <td>{payment.alumni}</td>
                <td>{payment.doc}</td>
                <td class="amount">₱{payment.amount}</td>
                <td>{payment.date}</td>
                <td>
                  <span class="status-badge" class:confirmed={payment.status === 'Confirmed'} class:pending={payment.status === 'Pending'}>
                    {payment.status}
                  </span>
                </td>
                <td class="actions">
                  {#if payment.status === 'Confirmed'}
                    <button class="action-btn match" on:click={() => matchPayment(payment)}>✓ Verified</button>
                  {:else}
                    <button class="action-btn assign" on:click={assignReferenceCode}>Assign Code</button>
                  {/if}
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>

      <div class="summary-box">
        <h4>Payment Summary</h4>
        <div class="summary-items">
          <div class="summary-item">
            <span class="label">Total Payments:</span>
            <span class="value">₱{paymentRecords.reduce((sum, p) => sum + p.amount, 0).toLocaleString()}</span>
          </div>
          <div class="summary-item">
            <span class="label">Confirmed:</span>
            <span class="value">{paymentRecords.filter(p => p.status === 'Confirmed').length}</span>
          </div>
          <div class="summary-item">
            <span class="label">Pending:</span>
            <span class="value">{paymentRecords.filter(p => p.status === 'Pending').length}</span>
          </div>
        </div>
      </div>

      <div class="info-box">
        <p>💡 Each payment gets a unique reference code to prevent mix-ups when alumni pay the same amount on the same day</p>
      </div>
    </div>
  {/if}

  <button class="logout-btn" on:click={() => dispatch('logout')}>Sign Out</button>
</div>

<style>
  .staff-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Tabs */
  .staff-tabs {
    display: flex;
    gap: 1rem;
    margin-bottom: 2rem;
    border-bottom: 2px solid var(--gray-medium);
    flex-wrap: wrap;
    overflow-x: auto;
  }

  .staff-tabs button {
    padding: 1rem;
    background: none;
    border: none;
    border-bottom: 3px solid transparent;
    color: var(--text-secondary);
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    white-space: nowrap;
  }

  .staff-tabs button:hover {
    color: var(--royal-blue);
  }

  .staff-tabs button.active {
    color: var(--royal-blue);
    border-bottom-color: var(--royal-blue);
  }

  .tab-content {
    animation: fadeIn 0.3s ease-in;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .tab-content h3 {
    color: #003BA3;
    margin-top: 0;
    font-size: 1.5rem;
  }

  .tab-subtitle {
    color: var(--text-secondary);
    font-size: 0.95rem;
    margin-bottom: 1.5rem;
  }

  /* Filters */
  .filters {
    display: flex;
    gap: 1rem;
    margin-bottom: 1.5rem;
    flex-wrap: wrap;
  }

  .filter-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .filter-group label {
    font-weight: 600;
    color: #333;
    font-size: 0.9rem;
  }

  .filter-group select {
    padding: 0.6rem;
    border: 1px solid var(--gray-medium);
    border-radius: 6px;
    font-size: 0.9rem;
    background: white;
    cursor: pointer;
  }

  /* Tables */
  .table-container {
    overflow-x: auto;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 1.5rem;
  }

  .staff-table {
    width: 100%;
    border-collapse: collapse;
    background: white;
  }

  .staff-table thead {
    background: #E8F0FF;
  }

  .staff-table th {
    padding: 1rem;
    text-align: left;
    font-weight: 700;
    color: #003BA3;
    border-bottom: 2px solid var(--gray-medium);
  }

  .staff-table td {
    padding: 0.85rem 1rem;
    border-bottom: 1px solid var(--gray-medium);
    color: var(--text-primary);
  }

  .staff-table tbody tr:hover {
    background: #f8faff;
  }

  .mono {
    font-family: monospace;
    font-size: 0.9rem;
  }

  .amount {
    font-weight: 700;
    color: var(--success-green);
  }

  /* Status Badges */
  .status-badge {
    display: inline-block;
    padding: 0.4rem 0.8rem;
    border-radius: 20px;
    font-size: 0.85rem;
    font-weight: 600;
  }

  .status-badge.pending {
    background: rgba(255, 165, 0, 0.1);
    color: var(--warning-orange);
  }

  .status-badge.verified,
  .status-badge.issued,
  .status-badge.confirmed {
    background: rgba(40, 167, 69, 0.1);
    color: var(--success-green);
  }

  .status-badge.review {
    background: rgba(0, 82, 204, 0.1);
    color: var(--royal-blue);
  }

  .status-badge.mailed {
    background: rgba(100, 100, 100, 0.1);
    color: #666;
  }

  /* Actions */
  .actions {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .action-btn {
    font-size: 0.8rem;
    padding: 0.5rem 0.75rem;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.3s;
    white-space: nowrap;
  }

  .action-btn.verify {
    background: rgba(40, 167, 69, 0.1);
    color: var(--success-green);
  }

  .action-btn.verify:hover {
    background: var(--success-green);
    color: white;
  }

  .action-btn.reject {
    background: rgba(220, 53, 69, 0.1);
    color: var(--danger-red);
  }

  .action-btn.reject:hover {
    background: var(--danger-red);
    color: white;
  }

  .action-btn.assign,
  .action-btn.match {
    background: rgba(0, 82, 204, 0.1);
    color: var(--royal-blue);
  }

  .action-btn.assign:hover,
  .action-btn.match:hover {
    background: var(--royal-blue);
    color: white;
  }

  /* Scanner Section */
  .scanner-section {
    margin-bottom: 2rem;
  }

  .scanner-card {
    background: linear-gradient(135deg, #E8F0FF 0%, #f5f8ff 100%);
    border: 2px solid var(--royal-blue);
    border-radius: 12px;
    padding: 2rem;
    text-align: center;
  }

  .scanner-card h4 {
    color: #003BA3;
    margin-top: 0;
    font-size: 1.2rem;
  }

  .scanner-input-area {
    display: flex;
    gap: 1rem;
    margin: 1.5rem 0;
  }

  .barcode-input {
    flex: 1;
    padding: 0.85rem;
    border: 1px solid var(--gray-medium);
    border-radius: 8px;
    font-size: 1rem;
    font-family: monospace;
  }

  .barcode-input:focus {
    outline: none;
    border-color: var(--royal-blue);
    box-shadow: 0 0 0 3px rgba(0, 82, 204, 0.1);
  }

  .btn-scan {
    padding: 0.85rem 1.5rem;
    background: var(--royal-blue);
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-scan:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  .scanner-help {
    color: var(--text-secondary);
    font-size: 0.85rem;
    margin: 0;
  }

  /* Inventory */
  .alerts-section {
    margin-bottom: 2rem;
  }

  .alerts-section h4 {
    color: var(--danger-red);
    margin-bottom: 1rem;
  }

  .alert-cards {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .alert-card {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.5rem;
    border-radius: 12px;
    background: rgba(220, 53, 69, 0.05);
    border-left: 4px solid var(--danger-red);
  }

  .alert-icon {
    font-size: 1.5rem;
  }

  .alert-content {
    flex: 1;
  }

  .alert-title {
    font-weight: 700;
    color: #003BA3;
    font-size: 1rem;
  }

  .alert-details {
    color: var(--text-secondary);
    font-size: 0.85rem;
  }

  .btn-reorder {
    padding: 0.6rem 1rem;
    background: var(--danger-red);
    color: white;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-reorder:hover {
    background: #c82333;
    transform: translateY(-2px);
  }

  .inventory-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .inventory-card {
    background: white;
    border: 1px solid var(--gray-medium);
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
  }

  .inventory-card.critical {
    border-color: var(--danger-red);
    background: rgba(220, 53, 69, 0.02);
  }

  .inventory-card.warning {
    border-color: var(--warning-orange);
  }

  .inventory-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }

  .item-name {
    font-weight: 700;
    color: #003BA3;
  }

  .status-icon {
    font-size: 1.5rem;
  }

  .stock-info {
    margin-bottom: 1rem;
  }

  .current,
  .threshold {
    display: flex;
    justify-content: space-between;
    margin-bottom: 0.5rem;
    font-size: 0.9rem;
  }

  .label {
    color: var(--text-secondary);
  }

  .value {
    font-weight: 700;
    color: #003BA3;
  }

  .progress-bar {
    width: 100%;
    height: 8px;
    background: var(--gray-medium);
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 1rem;
  }

  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--success-green), var(--warning-orange));
    transition: width 0.3s ease;
  }

  /* Summary Box */
  .summary-box {
    background: white;
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-top: 1.5rem;
  }

  .summary-box h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .summary-items {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem;
  }

  .summary-item {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem;
    background: #E8F0FF;
    border-radius: 8px;
  }

  .summary-item .label {
    color: var(--text-secondary);
    font-size: 0.85rem;
  }

  .summary-item .value {
    font-weight: 700;
    color: #003BA3;
    font-size: 1.1rem;
  }

  /* Info Box */
  .info-box {
    background: #E8F0FF;
    border-left: 4px solid var(--royal-blue);
    border-radius: 8px;
    padding: 1rem;
    margin-top: 1.5rem;
  }

  .info-box p {
    margin: 0;
    color: #003BA3;
    font-size: 0.9rem;
  }

  /* Logout Button */
  .logout-btn {
    width: 100%;
    padding: 0.95rem;
    background: var(--danger-red);
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    margin-top: 2rem;
  }

  .logout-btn:hover {
    background: #c82333;
    transform: translateY(-2px);
  }

  @media (max-width: 768px) {
    .staff-tabs {
      gap: 0.5rem;
    }

    .staff-tabs button {
      padding: 0.75rem 0.5rem;
      font-size: 0.8rem;
    }

    .scanner-input-area {
      flex-direction: column;
    }

    .inventory-grid {
      grid-template-columns: 1fr;
    }

    .alert-card {
      flex-direction: column;
      text-align: center;
    }

    .table-container {
      font-size: 0.85rem;
    }

    .staff-table th,
    .staff-table td {
      padding: 0.6rem 0.4rem;
    }
  }
</style>

<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let user;

  let activeTab = 'request';
  let currentStep = 1;
  let selectedDocument = '';
  let selectedPurpose = '';
  let showPurposeGuide = false;
  let biometricVerified = false;
  let rushProcessing = false;
  let rushConfirmed = false;
  let paymentRequired = false;
  let requestSubmitted = false;
  let referenceCode = '';

  const documents = [
    { id: 'tor', name: 'Transcript of Records (TOR)', fee: 150 },
    { id: 'diploma', name: 'Copy of Diploma', fee: 200 },
    { id: 'gmc', name: 'Good Moral Character', fee: 100 },
    { id: 'certification', name: 'Course Certification', fee: 175 },
    { id: 'letter', name: 'Certification Letter', fee: 125 }
  ];

  const purposes = [
    { id: 'work', name: '💼 Work/Employment', description: 'For job applications or employment abroad' },
    { id: 'abroad', name: '🌍 Further Studies Abroad', description: 'For university applications overseas' },
    { id: 'board', name: '🎓 Board Exam', description: 'For professional board exam requirements' }
  ];

  const purposeGuide = {
    work: 'For employment, you need an Official Transcript or Certified True Copy.',
    abroad: 'For international universities, request an Official Transcript with higher education credentials.',
    board: 'Board exams typically require a Certified True Copy with official seal.'
  };

  const requirements = {
    tor: ['Valid ID', 'Student Number', 'Completed form', 'Payment (₱150)', 'Processing time: 3-5 business days'],
    diploma: ['Valid ID', 'Official request form', 'Payment (₱200)', 'Processing time: 5-7 business days'],
    gmc: ['Valid ID', 'Student Number', 'Form with purpose', 'Payment (₱100)', 'Processing time: 2-3 business days'],
    certification: ['Valid ID', 'Course details', 'Official request', 'Payment (₱175)', 'Processing time: 3-5 business days'],
    letter: ['Valid ID', 'Detailed purpose', 'Official request form', 'Payment (₱125)', 'Processing time: 2-3 business days']
  };

  const mockRequests = [
    { id: 'REF-202601001', doc: 'Transcript of Records', date: '2026-01-15', status: 'Completed', verified: true },
    { id: 'REF-202601002', doc: 'Good Moral Character', date: '2025-12-20', status: 'Completed', verified: true },
    { id: 'REF-202512001', doc: 'Diploma Copy', date: '2025-11-10', status: 'Completed', verified: true }
  ];

  function selectDocument(doc) {
    selectedDocument = doc.name;
    currentStep = 2;
  }

  function openPurposeGuide() {
    showPurposeGuide = true;
  }

  function selectPurpose(purpose) {
    selectedPurpose = purpose.name;
    showPurposeGuide = false;
    currentStep = 3;
  }

  function verifyBiometric() {
    alert('🔐 Scanning biometrics...\n✅ Biometric verification successful!');
    biometricVerified = true;
    currentStep = 4;
  }

  function toggleRushProcessing() {
    rushProcessing = !rushProcessing;
  }

  function confirmRush() {
    rushConfirmed = true;
    paymentRequired = true;
    alert('🚀 Rush processing confirmed!\n⏱️ 48-hour turnaround\n💰 Additional fee: ₱500');
  }

  function submitRequest() {
    requestSubmitted = true;
    referenceCode = `ADDU-${Math.floor(Math.random() * 1000000)}`;
    alert(`✅ Request submitted successfully!\n📋 Reference Code: ${referenceCode}\n💰 Total Amount: ₱${rushProcessing ? 650 : 150}`);
  }

  function resetForm() {
    selectedDocument = '';
    selectedPurpose = '';
    biometricVerified = false;
    rushProcessing = false;
    rushConfirmed = false;
    paymentRequired = false;
    requestSubmitted = false;
    currentStep = 1;
  }

  function downloadPDF() {
    alert('📥 Downloading document...\n📄 File: Transcript_20190034_2026.pdf\n🔐 Secure PDF with QR code\n⏰ Expiration: 30 days');
  }

  function shareRequest() {
    alert('📤 Sharing request link...\n✅ Link copied to clipboard!');
  }
</script>

<div class="document-container">
  <!-- Tab Navigation -->
  <div class="tabs">
    <button 
      class={activeTab === 'request' ? 'active' : ''} 
      on:click={() => activeTab = 'request'}
    >
      📝 New Request
    </button>
    <button 
      class={activeTab === 'history' ? 'active' : ''} 
      on:click={() => activeTab = 'history'}
    >
      📜 Request History
    </button>
  </div>

  <!-- Request Tab -->
  {#if activeTab === 'request'}
    <div class="tab-content">
      {#if !requestSubmitted}
        <!-- Step 1: Document Selection -->
        {#if currentStep === 1}
          <div class="step">
            <h3>Step 1: Select Document Type</h3>
            <p class="step-description">Choose the document you need</p>
            
            <div class="document-grid">
              {#each documents as doc}
                <div class="document-card" on:click={() => selectDocument(doc)}>
                  <div class="doc-icon">📄</div>
                  <div class="doc-name">{doc.name}</div>
                  <div class="doc-fee">₱{doc.fee}</div>
                </div>
              {/each}
            </div>

            <button class="help-btn" on:click={openPurposeGuide}>
              ❓ Help me choose the right document
            </button>

            {#if showPurposeGuide}
              <div class="guide-modal">
                <div class="guide-overlay" on:click={() => showPurposeGuide = false}></div>
                <div class="guide-content">
                  <h4>📚 Purpose Guide</h4>
                  <p>What do you need this document for?</p>
                  
                  <div class="purpose-options">
                    {#each purposes as purpose}
                      <div class="purpose-card" on:click={() => selectPurpose(purpose)}>
                        <div class="purpose-name">{purpose.name}</div>
                        <p>{purpose.description}</p>
                      </div>
                    {/each}
                  </div>

                  <button class="close-btn" on:click={() => showPurposeGuide = false}>Close</button>
                </div>
              </div>
            {/if}
          </div>

        <!-- Step 2: Purpose Selection -->
        {:else if currentStep === 2}
          <div class="step">
            <h3>Step 2: Specify Your Purpose</h3>
            <div class="selected-doc">
              <p>📄 <strong>Document:</strong> {selectedDocument}</p>
            </div>

            <div class="purpose-grid">
              {#each purposes as purpose}
                <div 
                  class="purpose-card-full" 
                  class:selected={selectedPurpose === purpose.name}
                  on:click={() => selectPurpose(purpose)}
                >
                  <div class="purpose-header">{purpose.name}</div>
                  <p>{purpose.description}</p>
                  <small>{purposeGuide[purpose.id]}</small>
                </div>
              {/each}
            </div>

            <button class="back-btn" on:click={() => currentStep = 1}>← Back</button>
          </div>

        <!-- Step 3: Biometric Verification -->
        {:else if currentStep === 3}
          <div class="step">
            <h3>Step 3: Biometric Verification</h3>
            <div class="selection-info">
              <p>📄 <strong>Document:</strong> {selectedDocument}</p>
              <p>🎯 <strong>Purpose:</strong> {selectedPurpose}</p>
            </div>

            <div class="biometric-section">
              <div class="biometric-card">
                <div class="status-icon">
                  {#if biometricVerified}
                    <span class="verified-icon">✓</span>
                  {:else}
                    <span class="unverified-icon">👆</span>
                  {/if}
                </div>
                
                <h4>
                  {#if biometricVerified}
                    Identity Verified
                  {:else}
                    Verify Your Identity
                  {/if}
                </h4>
                
                <p>
                  {#if biometricVerified}
                    Your identity has been successfully verified using biometric authentication.
                  {:else}
                    Use FaceID or Fingerprint to confirm your identity
                  {/if}
                </p>

                {#if !biometricVerified}
                  <button class="btn-biometric" on:click={verifyBiometric}>
                    👆 Scan Biometrics Now
                  </button>
                {/if}
              </div>
            </div>

            <button class="back-btn" on:click={() => currentStep = 2}>← Back</button>
          </div>

        <!-- Step 4: Requirements & Rush Processing -->
        {:else if currentStep === 4}
          <div class="step">
            <h3>Step 4: Requirements & Payment Options</h3>
            
            <div class="requirements-card">
              <h4>✓ Requirements Checklist</h4>
              <ul class="requirements-list">
                {#each requirements[Object.keys(documents).find(i => documents[i].name === selectedDocument)?.id || 'tor'] as req}
                  <li>
                    <input type="checkbox" checked disabled>
                    <span>{req}</span>
                  </li>
                {/each}
              </ul>
            </div>

            <div class="rush-processing">
              <label class="checkbox-label">
                <input 
                  type="checkbox" 
                  bind:checked={rushProcessing}
                  on:change={toggleRushProcessing}
                >
                <span>🚀 Rush Processing (₱500)</span>
              </label>
              <p class="rush-info">Get your documents within 48 hours instead of 3-5 business days</p>
              
              {#if rushProcessing && !rushConfirmed}
                <button class="btn-primary" on:click={confirmRush}>Confirm Rush Processing</button>
              {/if}
            </div>

            <div class="fee-summary">
              <h4>Fee Summary</h4>
              <div class="fee-row">
                <span>Document Fee:</span>
                <span>₱150</span>
              </div>
              {#if rushProcessing}
                <div class="fee-row">
                  <span>Rush Processing:</span>
                  <span>₱500</span>
                </div>
              {/if}
              <div class="fee-total">
                <span>Total:</span>
                <span>₱{rushProcessing ? 650 : 150}</span>
              </div>
            </div>

            <div class="action-buttons">
              <button class="back-btn" on:click={() => currentStep = 3}>← Back</button>
              <button class="btn-primary" on:click={submitRequest}>✓ Submit Request</button>
            </div>
          </div>
        {/if}

      {:else}
        <!-- Success Screen -->
        <div class="success-screen">
          <div class="success-icon">✓</div>
          <h3>Request Submitted Successfully!</h3>
          
          <div class="reference-box">
            <p>Reference Code:</p>
            <div class="reference-code">{referenceCode}</div>
          </div>

          <div class="status-info">
            <div class="info-row">
              <span>Document:</span>
              <strong>{selectedDocument}</strong>
            </div>
            <div class="info-row">
              <span>Purpose:</span>
              <strong>{selectedPurpose}</strong>
            </div>
            <div class="info-row">
              <span>Total Amount:</span>
              <strong>₱{rushProcessing ? 650 : 150}</strong>
            </div>
            <div class="info-row">
              <span>Processing Time:</span>
              <strong>{rushProcessing ? '48 hours ⚡' : '3-5 business days'}</strong>
            </div>
          </div>

          <div class="next-steps">
            <h4>Next Steps:</h4>
            <ol>
              <li>Proceed to payment using your reference code</li>
              <li>You will receive a confirmation email with payment details</li>
              <li>Track your request status in real-time</li>
              <li>Download and share your digital document</li>
            </ol>
          </div>

          <div class="action-buttons">
            <button class="btn-primary" on:click={() => activeTab = 'history'}>Track Status</button>
            <button class="back-btn" on:click={resetForm}>New Request</button>
          </div>
        </div>
      {/if}
    </div>

  <!-- History Tab -->
  {:else if activeTab === 'history'}
    <div class="tab-content">
      <h3>Your Request History</h3>
      <p class="history-subtitle">All your past requests and current status</p>

      <div class="request-list">
        {#each mockRequests as request}
          <div class="request-item">
            <div class="request-header">
              <div class="request-info">
                <div class="request-id">{request.id}</div>
                <div class="request-doc">{request.doc}</div>
              </div>
              <div class="request-status">
                {#if request.verified}
                  <span class="badge badge-success">✓ {request.status}</span>
                {:else}
                  <span class="badge badge-warning">⏳ {request.status}</span>
                {/if}
              </div>
            </div>

            <div class="request-details">
              <div class="detail-item">
                <span class="label">Request Date:</span>
                <span>{request.date}</span>
              </div>
              <div class="detail-item">
                <span class="label">Verification:</span>
                <span>{request.verified ? '✓ Verified' : 'Pending'}</span>
              </div>
            </div>

            {#if request.status === 'Completed'}
              <div class="request-actions">
                <button class="action-btn" on:click={downloadPDF}>📥 Download PDF</button>
                <button class="action-btn" on:click={shareRequest}>📤 Share</button>
              </div>
            {/if}
          </div>
        {/each}
      </div>
    </div>
  {/if}
</div>

<style>
  .document-container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Tabs */
  .tabs {
    display: flex;
    gap: 1rem;
    margin-bottom: 2rem;
    border-bottom: 2px solid var(--gray-medium);
    flex-wrap: wrap;
  }

  .tabs button {
    padding: 1rem;
    background: none;
    border: none;
    border-bottom: 3px solid transparent;
    color: var(--text-secondary);
    font-size: 0.95rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .tabs button:hover {
    color: var(--royal-blue);
  }

  .tabs button.active {
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

  /* Steps */
  .step {
    background: white;
    border-radius: 12px;
    padding: 2rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .step h3 {
    color: #003BA3;
    margin-top: 0;
    font-size: 1.3rem;
  }

  .step-description {
    color: var(--text-secondary);
    font-size: 0.95rem;
    margin-bottom: 1.5rem;
  }

  /* Document Grid */
  .document-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .document-card {
    background: #E8F0FF;
    border: 2px solid var(--royal-blue);
    border-radius: 12px;
    padding: 1.5rem;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s;
  }

  .document-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 16px rgba(0, 82, 204, 0.2);
  }

  .doc-icon {
    font-size: 2.5rem;
    margin-bottom: 0.75rem;
  }

  .doc-name {
    font-weight: 600;
    color: #003BA3;
    font-size: 0.9rem;
    margin-bottom: 0.5rem;
  }

  .doc-fee {
    color: var(--success-green);
    font-weight: 700;
  }

  .help-btn {
    background: white;
    color: var(--royal-blue);
    border: 2px dashed var(--royal-blue);
    padding: 0.75rem 1.5rem;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.3s;
  }

  .help-btn:hover {
    background: #E8F0FF;
  }

  /* Guide Modal */
  .guide-modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }

  .guide-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
  }

  .guide-content {
    position: relative;
    background: white;
    border-radius: 12px;
    padding: 2rem;
    max-width: 500px;
    max-height: 80vh;
    overflow-y: auto;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  }

  .guide-content h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .purpose-options {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin: 1.5rem 0;
  }

  .purpose-card {
    background: #E8F0FF;
    border: 2px solid var(--gray-medium);
    border-radius: 8px;
    padding: 1rem;
    cursor: pointer;
    transition: all 0.3s;
  }

  .purpose-card:hover {
    border-color: var(--royal-blue);
    background: #f0f5ff;
  }

  .purpose-name {
    font-weight: 600;
    color: #003BA3;
    margin-bottom: 0.5rem;
  }

  .purpose-card p {
    margin: 0;
    font-size: 0.9rem;
    color: var(--text-secondary);
  }

  .close-btn {
    width: 100%;
    padding: 0.75rem;
    background: var(--gray-medium);
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.3s;
    margin-top: 1rem;
  }

  .close-btn:hover {
    background: #ccc;
  }

  /* Selection Info */
  .selected-doc, .selection-info {
    background: #E8F0FF;
    padding: 1rem;
    border-left: 4px solid var(--royal-blue);
    border-radius: 8px;
    margin-bottom: 1.5rem;
  }

  .selected-doc p, .selection-info p {
    margin: 0.5rem 0;
    color: #003BA3;
  }

  .purpose-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .purpose-card-full {
    background: white;
    border: 2px solid var(--gray-medium);
    border-radius: 12px;
    padding: 1.5rem;
    cursor: pointer;
    transition: all 0.3s;
  }

  .purpose-card-full:hover {
    border-color: var(--royal-blue);
  }

  .purpose-card-full.selected {
    border-color: var(--royal-blue);
    background: #E8F0FF;
  }

  .purpose-header {
    font-weight: 600;
    color: #003BA3;
    font-size: 1rem;
    margin-bottom: 0.5rem;
  }

  .purpose-card-full p {
    margin: 0.5rem 0;
    font-size: 0.9rem;
    color: var(--text-secondary);
  }

  .purpose-card-full small {
    display: block;
    margin-top: 0.75rem;
    padding-top: 0.75rem;
    border-top: 1px solid var(--gray-medium);
    color: #666;
    font-style: italic;
  }

  /* Biometric Section */
  .biometric-section {
    margin: 2rem 0;
  }

  .biometric-card {
    background: linear-gradient(135deg, #E8F0FF 0%, #f5f8ff 100%);
    border: 2px solid var(--royal-blue);
    border-radius: 12px;
    padding: 2.5rem;
    text-align: center;
  }

  .status-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
  }

  .verified-icon {
    color: var(--success-green);
  }

  .unverified-icon {
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
  }

  .biometric-card h4 {
    color: #003BA3;
    font-size: 1.2rem;
    margin: 0.5rem 0;
  }

  .biometric-card p {
    color: var(--text-secondary);
    margin: 1rem 0;
  }

  .btn-biometric {
    background: var(--royal-blue);
    color: white;
    padding: 1rem 2rem;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    margin-top: 1rem;
  }

  .btn-biometric:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  /* Requirements */
  .requirements-card {
    background: white;
    border: 1px solid var(--gray-medium);
    border-radius: 12px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .requirements-card h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .requirements-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .requirements-list li {
    display: flex;
    align-items: center;
    padding: 0.75rem 0;
    border-bottom: 1px solid var(--gray-medium);
    color: var(--text-secondary);
  }

  .requirements-list li:last-child {
    border-bottom: none;
  }

  .requirements-list input {
    margin-right: 1rem;
    accent-color: var(--royal-blue);
  }

  /* Rush Processing */
  .rush-processing {
    background: linear-gradient(135deg, #fff8e1 0%, #fffbeb 100%);
    border-left: 4px solid var(--warning-orange);
    border-radius: 8px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .checkbox-label {
    display: flex;
    align-items: center;
    font-weight: 600;
    color: #003BA3;
    cursor: pointer;
    gap: 0.75rem;
  }

  .checkbox-label input {
    accent-color: var(--royal-blue);
    cursor: pointer;
  }

  .rush-info {
    margin: 0.75rem 0 1rem 0;
    color: var(--text-secondary);
    font-size: 0.9rem;
  }

  /* Fee Summary */
  .fee-summary {
    background: white;
    border: 1px solid var(--gray-medium);
    border-radius: 12px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .fee-summary h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .fee-row {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 0;
    border-bottom: 1px solid var(--gray-medium);
    color: var(--text-secondary);
  }

  .fee-total {
    display: flex;
    justify-content: space-between;
    padding: 1rem 0;
    border-top: 2px solid var(--royal-blue);
    margin-top: 0.75rem;
    color: #003BA3;
    font-weight: 700;
    font-size: 1.1rem;
  }

  /* Action Buttons */
  .action-buttons {
    display: flex;
    gap: 1rem;
    margin-top: 1.5rem;
    flex-wrap: wrap;
  }

  .back-btn {
    background: white;
    color: var(--royal-blue);
    border: 2px solid var(--royal-blue);
    padding: 0.85rem 1.5rem;
    border-radius: 8px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .back-btn:hover {
    background: #E8F0FF;
  }

  .btn-primary {
    background: var(--royal-blue);
    color: white;
    padding: 0.85rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    flex: 1;
    min-width: 150px;
  }

  .btn-primary:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  /* Success Screen */
  .success-screen {
    background: white;
    border-radius: 12px;
    padding: 2rem;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .success-icon {
    width: 80px;
    height: 80px;
    background: rgba(40, 167, 69, 0.1);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.5rem;
    color: var(--success-green);
    margin: 0 auto 1.5rem;
  }

  .success-screen h3 {
    color: #003BA3;
    font-size: 1.5rem;
    margin: 0 0 1.5rem 0;
  }

  .reference-box {
    background: #E8F0FF;
    border: 2px solid var(--royal-blue);
    border-radius: 8px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .reference-code {
    font-family: monospace;
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--royal-blue);
    padding: 1rem;
    background: white;
    border-radius: 4px;
    margin-top: 0.75rem;
  }

  .status-info {
    background: #f8f9fa;
    border-radius: 8px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: left;
  }

  .info-row {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 0;
    border-bottom: 1px solid var(--gray-medium);
  }

  .info-row:last-child {
    border-bottom: none;
  }

  .next-steps {
    background: white;
    border-left: 4px solid var(--success-green);
    border-radius: 8px;
    padding: 1.5rem;
    text-align: left;
    margin-bottom: 1.5rem;
  }

  .next-steps h4 {
    margin-top: 0;
    color: #003BA3;
  }

  .next-steps ol {
    margin: 0;
    padding-left: 1.5rem;
  }

  .next-steps li {
    padding: 0.5rem 0;
    color: var(--text-secondary);
  }

  /* History */
  .history-subtitle {
    color: var(--text-secondary);
    font-size: 0.95rem;
  }

  .request-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .request-item {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
  }

  .request-item:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  }

  .request-header {
    background: #E8F0FF;
    padding: 1rem 1.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .request-id {
    font-weight: 700;
    color: var(--royal-blue);
    font-size: 0.9rem;
  }

  .request-doc {
    color: #003BA3;
    font-weight: 600;
    margin-top: 0.25rem;
  }

  .request-details {
    padding: 1rem 1.5rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }

  .detail-item {
    display: flex;
    justify-content: space-between;
  }

  .label {
    color: var(--text-secondary);
    font-weight: 600;
  }

  .request-actions {
    padding: 1rem 1.5rem;
    border-top: 1px solid var(--gray-medium);
    display: flex;
    gap: 0.75rem;
    flex-wrap: wrap;
  }

  .action-btn {
    background: var(--royal-blue);
    color: white;
    padding: 0.6rem 1rem;
    border: none;
    border-radius: 6px;
    font-size: 0.85rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .action-btn:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  @media (max-width: 600px) {
    .step {
      padding: 1.5rem;
    }

    .document-grid {
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    }

    .purpose-grid {
      grid-template-columns: 1fr;
    }

    .action-buttons {
      flex-direction: column;
    }

    .btn-primary {
      width: 100%;
      flex: none;
    }

    .request-details {
      grid-template-columns: 1fr;
    }

    .guide-content {
      max-width: 90vw;
      padding: 1.5rem;
    }
  }
</style>

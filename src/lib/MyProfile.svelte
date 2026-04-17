<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let user;

  let activeTab = 'profile';
  let showBiometricVerification = false;
  let verificationStatus = 'unverified';

  const mockProfile = {
    fullName: 'Juan dela Cruz',
    studentNumber: user.studentNumber,
    email: 'juan@ateneo.edu.ph',
    degreeProgram: 'Bachelor of Science in Computer Science',
    graduationYear: 2020,
    honors: 'Cum Laude',
    registrarStatus: 'Verified ✓'
  };

  function verifyWithBiometrics() {
    showBiometricVerification = true;
    alert('🔐 Biometric verification in progress...\nScanning FaceID...\n✅ Verification successful!');
    verificationStatus = 'verified';
    showBiometricVerification = false;
  }

  function sharePassport() {
    alert('📤 Sharing Digital Academic Passport...\nQR Code generated!\nSharing link copied to clipboard.');
  }

  function downloadPassport() {
    alert('📥 Downloading verified academic passport (PDF)...\nFile: Academic_Passport_20190034.pdf');
  }
</script>

<div class="profile-container">
  <div class="profile-header">
    <h2>My Profile</h2>
    <p class="subtitle">Your Verified Digital Academic Passport</p>
  </div>

  <!-- Tab Navigation -->
  <div class="tabs">
    <button 
      class={activeTab === 'profile' ? 'active' : ''} 
      on:click={() => activeTab = 'profile'}
    >
      📋 Profile
    </button>
    <button 
      class={activeTab === 'passport' ? 'active' : ''} 
      on:click={() => activeTab = 'passport'}
    >
      🛡️ Digital Passport
    </button>
    <button 
      class={activeTab === 'verification' ? 'active' : ''} 
      on:click={() => activeTab = 'verification'}
    >
      🔐 Verification
    </button>
  </div>

  <!-- Profile Tab -->
  {#if activeTab === 'profile'}
    <div class="tab-content">
      <div class="card">
        <h3>Personal Information</h3>
        <div class="info-section">
          <div class="info-item">
            <label>Full Name</label>
            <p>{mockProfile.fullName}</p>
          </div>
          <div class="info-item">
            <label>Student Number</label>
            <p>{mockProfile.studentNumber}</p>
          </div>
          <div class="info-item">
            <label>Email Address</label>
            <p>{mockProfile.email}</p>
          </div>
        </div>
      </div>

      <div class="card">
        <h3>Academic Information</h3>
        <div class="info-section">
          <div class="info-item">
            <label>Degree Program</label>
            <p>{mockProfile.degreeProgram}</p>
          </div>
          <div class="info-item">
            <label>Graduation Year</label>
            <p>{mockProfile.graduationYear}</p>
          </div>
          <div class="info-item">
            <label>Honors</label>
            <p class="honors">{mockProfile.honors}</p>
          </div>
          <div class="info-item">
            <label>Registrar Status</label>
            <p class="verified-badge">
              <span class="badge badge-success">{mockProfile.registrarStatus}</span>
            </p>
          </div>
        </div>
      </div>
    </div>

  <!-- Digital Passport Tab -->
  {:else if activeTab === 'passport'}
    <div class="tab-content">
      <div class="passport-card">
        <div class="passport-header">
          🛡️ Digital Academic Passport
        </div>
        
        <div class="passport-content">
          <div class="passport-section">
            <h4>Official Record</h4>
            <p>This verified digital academic passport is officially linked to your Registrar records.</p>
            <div class="sync-status">
              <span class="status-badge success">✓ In Sync with Registrar</span>
            </div>
          </div>

          <div class="passport-details">
            <div class="detail-row">
              <span class="label">Graduate Name:</span>
              <span class="value">{mockProfile.fullName}</span>
            </div>
            <div class="detail-row">
              <span class="label">Degree Awarded:</span>
              <span class="value">{mockProfile.degreeProgram}</span>
            </div>
            <div class="detail-row">
              <span class="label">Conferred:</span>
              <span class="value">{mockProfile.graduationYear}</span>
            </div>
            <div class="detail-row">
              <span class="label">Recognition:</span>
              <span class="value">{mockProfile.honors}</span>
            </div>
          </div>

          <div class="passport-qr">
            <p>QR Code for verification</p>
            <div class="qr-placeholder">
              ███████████████
              ███ Blue QR ███
              ████ Code ████
              ███████████████
            </div>
          </div>
        </div>

        <div class="passport-actions">
          <button class="btn-primary" on:click={sharePassport}>📤 Share Passport</button>
          <button class="btn-primary" on:click={downloadPassport}>📥 Download (PDF)</button>
        </div>
      </div>

      <div class="info-box">
        <p>💡 Your digital passport can be instantly shared with employers and educational institutions. It includes your verified degree information and cannot be forged.</p>
      </div>
    </div>

  <!-- Verification Tab -->
  {:else if activeTab === 'verification'}
    <div class="tab-content">
      <div class="verification-card">
        <h3>Biometric Verification Status</h3>
        
        <div class="verification-status">
          <div class="status-circle {verificationStatus}">
            {#if verificationStatus === 'verified'}
              ✓
            {:else}
              ?
            {/if}
          </div>
          <p class="status-text">
            {#if verificationStatus === 'verified'}
              <span class="verified">Your identity has been verified via biometrics</span>
            {:else}
              <span class="unverified">Biometric verification not yet completed</span>
            {/if}
          </p>
        </div>

        {#if verificationStatus === 'unverified'}
          <button class="btn-primary verify-btn" on:click={verifyWithBiometrics}>
            👆 Verify with Biometrics
          </button>
          <p class="info-text">Secure your account and verify your identity using FaceID or Fingerprint.</p>
        {:else}
          <div class="verified-info">
            <p>✓ Verification Date: {new Date().toLocaleDateString()}</p>
            <button class="btn-secondary" on:click={() => verificationStatus = 'unverified'}>Re-verify</button>
          </div>
        {/if}
      </div>

      <div class="security-info">
        <h4>🔒 Security Features</h4>
        <ul>
          <li>Biometric authentication (FaceID/Fingerprint)</li>
          <li>QR code-based verification</li>
          <li>Encrypted digital passport</li>
          <li>Real-time registrar sync</li>
          <li>Audit trail access</li>
        </ul>
      </div>
    </div>
  {/if}

  <button class="logout-btn" on:click={() => dispatch('logout')}>Sign Out</button>
</div>

<style>
  .profile-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  .profile-header {
    margin-bottom: 2rem;
    text-align: center;
  }

  .profile-header h2 {
    color: var(--royal-blue);
    font-size: 1.8rem;
    margin: 0 0 0.5rem 0;
  }

  .subtitle {
    color: var(--text-secondary);
    font-size: 1rem;
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

  /* Tab Content */
  .tab-content {
    animation: fadeIn 0.3s ease-in;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .card {
    background: white;
    border-radius: 12px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .card h3 {
    color: #003BA3;
    margin-top: 0;
    font-size: 1.2rem;
  }

  /* Info Section */
  .info-section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }

  .info-item {
    display: flex;
    flex-direction: column;
  }

  .info-item label {
    font-size: 0.85rem;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-bottom: 0.5rem;
  }

  .info-item p {
    font-size: 1rem;
    color: var(--text-primary);
    margin: 0;
    font-weight: 500;
  }

  .honors {
    color: var(--royal-blue);
    font-weight: 700;
  }

  .verified-badge {
    display: inline-block;
  }

  /* Passport Card */
  .passport-card {
    background: linear-gradient(135deg, #E8F0FF 0%, #F5F8FF 100%);
    border: 2px solid var(--royal-blue);
    border-radius: 12px;
    overflow: hidden;
    margin-bottom: 1.5rem;
  }

  .passport-header {
    background: var(--royal-blue);
    color: white;
    padding: 1rem;
    font-size: 1.2rem;
    font-weight: 700;
    text-align: center;
  }

  .passport-content {
    padding: 2rem;
  }

  .passport-section {
    margin-bottom: 2rem;
  }

  .passport-section h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .sync-status {
    margin: 1rem 0;
  }

  .status-badge {
    display: inline-block;
    padding: 0.5rem 1rem;
    border-radius: 20px;
    font-size: 0.85rem;
    font-weight: 600;
  }

  .status-badge.success {
    background: rgba(40, 167, 69, 0.1);
    color: var(--success-green);
  }

  .passport-details {
    background: white;
    padding: 1.5rem;
    border-radius: 8px;
    margin-bottom: 1.5rem;
  }

  .detail-row {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 0;
    border-bottom: 1px solid var(--gray-medium);
  }

  .detail-row:last-child {
    border-bottom: none;
  }

  .detail-row .label {
    font-weight: 600;
    color: var(--text-secondary);
  }

  .detail-row .value {
    color: var(--text-primary);
  }

  .passport-qr {
    text-align: center;
    margin: 2rem 0;
  }

  .qr-placeholder {
    display: inline-block;
    background: white;
    padding: 1.5rem;
    border-radius: 8px;
    font-family: monospace;
    font-size: 0.7rem;
    line-height: 1;
    color: #999;
  }

  .passport-actions {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
  }

  .info-box {
    background: #E8F0FF;
    border-left: 4px solid var(--royal-blue);
    padding: 1rem;
    border-radius: 8px;
    margin-top: 1.5rem;
  }

  .info-box p {
    margin: 0;
    color: #003BA3;
    font-size: 0.9rem;
  }

  /* Verification Card */
  .verification-card {
    background: white;
    border-radius: 12px;
    padding: 2rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 1.5rem;
    text-align: center;
  }

  .verification-card h3 {
    color: #003BA3;
    margin-top: 0;
  }

  .verification-status {
    margin: 2rem 0;
  }

  .status-circle {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 80px;
    height: 80px;
    border-radius: 50%;
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  .status-circle.verified {
    background: rgba(40, 167, 69, 0.1);
    color: var(--success-green);
  }

  .status-circle.unverified {
    background: rgba(255, 165, 0, 0.1);
    color: var(--warning-orange);
  }

  .status-text {
    margin: 0;
    font-size: 1.1rem;
    font-weight: 600;
  }

  .verified {
    color: var(--success-green);
  }

  .unverified {
    color: var(--warning-orange);
  }

  .verify-btn {
    margin-top: 1rem;
  }

  .info-text {
    color: var(--text-secondary);
    font-size: 0.9rem;
    margin-top: 1rem;
  }

  .verified-info {
    background: rgba(40, 167, 69, 0.05);
    padding: 1rem;
    border-radius: 8px;
    margin-top: 1rem;
  }

  .verified-info p {
    margin: 0.5rem 0;
    color: var(--success-green);
  }

  .security-info {
    background: white;
    border-radius: 12px;
    padding: 1.5rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .security-info h4 {
    color: #003BA3;
    margin-top: 0;
  }

  .security-info ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .security-info li {
    padding: 0.75rem 0;
    color: var(--text-secondary);
    border-bottom: 1px solid var(--gray-medium);
  }

  .security-info li:last-child {
    border-bottom: none;
  }

  .security-info li::before {
    content: '✓ ';
    color: var(--success-green);
    font-weight: 700;
    margin-right: 0.5rem;
  }

  /* Buttons */
  .btn-primary {
    background: var(--royal-blue);
    color: white;
    padding: 0.85rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-size: 0.95rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-primary:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  .btn-secondary {
    background: white;
    color: var(--royal-blue);
    padding: 0.85rem 1.5rem;
    border: 2px solid var(--royal-blue);
    border-radius: 8px;
    font-size: 0.95rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-secondary:hover {
    background: #E8F0FF;
  }

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

  @media (max-width: 600px) {
    .profile-container {
      padding: 1rem;
    }

    .info-section {
      grid-template-columns: 1fr;
    }

    .tabs {
      gap: 0.5rem;
    }

    .tabs button {
      padding: 0.75rem 0.5rem;
      font-size: 0.85rem;
    }

    .passport-actions {
      flex-direction: column;
    }

    .passport-actions button {
      width: 100%;
    }
  }
</style>

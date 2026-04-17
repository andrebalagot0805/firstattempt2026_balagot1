<script>
  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  
  export let user;
  
  let isAlumni = user?.role === 'alumni';
  let activeModule = 'home';
  let currentPage = 'dashboard';
  let selectedDocument = null;
  let showRequestForm = false;
  let showDownloadMenu = false;
  let showShareModal = false;
  let theme = 'light';
  let emailNotifications = true;
  let initials = '';

  $: initials = getInitials(user?.name || 'User');
  
  function navigate(page) {
    currentPage = page;
  }
  
  function handleSignOut() {
    dispatch('logout');
  }
  
  function getInitials(name) {
    return name.split(' ').map(n => n[0]).join('').toUpperCase();
  }

  function handleDownloadPDF() {
    alert('📥 PDF credential downloaded. Check your Downloads folder.');
    showDownloadMenu = false;
  }

  function handleShareCredential() {
    showShareModal = true;
  }

  function handleViewTranscript() {
    alert('📋 Opening full transcript viewer...\n\nTotal Credits: 120\nCumulative GPA: 1.45\nStatus: Graduated with Honors');
  }

  function handleManageBiometrics() {
    alert('🔏 Biometric Login Manager\n\nFingerprint: Registered\nFace ID: Not registered\n\nTap "Add New" to register additional biometrics.');
  }

  function handleDocumentClick(docType) {
    selectedDocument = docType;
    showRequestForm = true;
    alert(`📄 Request for: ${docType}\n\nPlease review the details:\n- Price: Check item details\n- Processing: 2-7 business days\n\nProceeding to payment...`);
  }

  function handleVerify(studentId) {
    alert(`✓ Profile verified for Student ID: ${studentId}\n\nCredentials confirmed with Registrar's Office.\nStatus: Active Alumni`);
  }

  function handleThemeChange(newTheme) {
    theme = newTheme;
    alert(`🎨 Theme changed to: ${newTheme}`);
  }

  function handleNotificationToggle() {
    emailNotifications = !emailNotifications;
    alert(`📧 Email notifications: ${emailNotifications ? 'Enabled' : 'Disabled'}`);
  }

  function handleProcessDocument(docType) {
    alert(`📋 Processing: ${docType}\n\nStatus: Batch processing initiated\n✓ 300 sets processed\n⏳ 150 remaining\n\nEstimated time: 2-4 days`);
  }

  function handleGenerateQRCode() {
    alert('🎯 QR Code Generated\n\nRequest ID: #QR-2024-01988\nDocument: Certified Diploma\n\nThis code is used for verification.');
  }

  function handleConfirmBatch(quantity) {
    alert(`✓ Batch confirmed: ${quantity} sets\n\nTotal: ₱${quantity * 150}\nStatus: Ready for batch processing\n\nProcessing will begin immediately.`);
  }

  function handleOrderSupplies() {
    alert('📦 Ordering: Binding Materials\n\nQuantity: 300 sets\nETA: 4 days\nPrice: ₱2,099\n\nOrder confirmed!');
  }

  function handleMatchPayment(paymentId) {
    alert(`💳 Payment Verified\n\nReference: ${paymentId}\nAmount: ₱2,500\nAlumni: Juan Dela Cruz\n\nPayment matched with request. Proceeding to document processing.`);
  }
</script>

<div class="app">
  <!-- SIDEBAR -->
  <nav class="sidebar">
    <div class="sidebar-logo">
      <div class="logo-mark">A</div>
      <div class="logo-text">
        <strong>Alumni Knights' Hub</strong>
        <span>Ateneo de Davao University</span>
      </div>
    </div>
    
    {#if isAlumni}
      <div class="sidebar-section">
        <div class="sidebar-section-label">Main</div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'dashboard'}
          on:click={() => navigate('dashboard')}
        >
          <span class="icon">⊞</span> Dashboard
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'profile'}
          on:click={() => navigate('profile')}
        >
          <span class="icon">👤</span> My Profile
        </div>
        <div class="sidebar-section-label">Documents</div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'documentRequest'}
          on:click={() => navigate('documentRequest')}
        >
          <span class="icon">📄</span> Request Documents
        </div>
        <div class="sidebar-section-label">Account</div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'settings'}
          on:click={() => navigate('settings')}
        >
          <span class="icon">⚙️</span> Settings
        </div>
      </div>
    {:else}
      <div class="sidebar-section">
        <div class="sidebar-section-label">Main</div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'dashboard'}
          on:click={() => navigate('dashboard')}
        >
          <span class="icon">⊞</span> Dashboard
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'profile'}
          on:click={() => navigate('profile')}
        >
          <span class="icon">👤</span> My Profile
        </div>
        <div class="sidebar-section-label">Operations</div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'verification'}
          on:click={() => navigate('verification')}
        >
          <span class="icon">✓</span> Verification
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'documents'}
          on:click={() => navigate('documents')}
        >
          <span class="icon">📋</span> Document Log
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'process'}
          on:click={() => navigate('process')}
        >
          <span class="icon">⚙️</span> Process Documents
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'inventory'}
          on:click={() => navigate('inventory')}
        >
          <span class="icon">📦</span> Inventory Alerts
        </div>
        <div 
          class="nav-item" 
          class:active={currentPage === 'payment'}
          on:click={() => navigate('payment')}
        >
          <span class="icon">💳</span> Payment Verification
        </div>
      </div>
    {/if}
    
    <div class="sidebar-footer">
      <div class="user-avatar">{initials}</div>
      <div class="user-info">
        <strong>{user?.name || 'User'}</strong>
        <span>{isAlumni ? 'Alumni' : 'Staff'}</span>
      </div>
      <button class="sign-out-btn" on:click={handleSignOut} title="Sign out">⬡</button>
    </div>
  </nav>
  
  <!-- MAIN CONTENT -->
  <div class="main">
    <header class="topbar">
      <div class="topbar-title">
        {#if currentPage === 'dashboard'}
          Dashboard
        {:else if currentPage === 'profile'}
          My Profile
        {:else if currentPage === 'documentRequest'}
          Request Documents
        {:else if currentPage === 'verification'}
          Verification Dashboard
        {:else if currentPage === 'documents'}
          Document Log
        {:else}Settings{/if}
      </div>
      <div class="topbar-search">
        <span class="search-icon">🔍</span>
        <input type="text" placeholder="Search...">
      </div>
      <button class="notif-btn">
        🔔<span class="notif-dot"></span>
      </button>
      <div class="user-avatar">{initials}</div>
    </header>
    
    <div class="content">
      {#if currentPage === 'dashboard'}
        <div class="section-header mb-6" style="align-items: flex-start;">
          <div style="flex: 1;">
            <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);margin-bottom:8px;">Welcome, {user?.name?.split(' ')[0] || 'User'}</h2>
            <p class="text-muted text-sm">Latest updates and activity from your account.</p>
          </div>
        </div>
        
        <!-- Hero Card -->
        <div class="card mb-6" style="background: linear-gradient(135deg, var(--royal) 0%, var(--royal-mid) 100%); color: white; padding: 0; overflow: hidden;">
          <div style="padding: 24px; display: flex; gap: 20px; align-items: center;">
            <div style="font-size: 48px; min-width: 80px; text-align: center;">{initials}</div>
            <div style="flex: 1;">
              <h3 style="font-family: 'DM Serif Display', serif; font-size: 18px; margin-bottom: 4px;">{user?.name}</h3>
              <p style="font-size: 12px; opacity: 0.9; margin-bottom: 8px;">
                {isAlumni ? '✓ Verified Alumni • ' : '✓ Verified Staff • '}
                {user?.studentNumber}
              </p>
              <div style="display: flex; gap: 8px;">
                <span class="badge" style="background: rgba(255,255,255,0.2); color: white; font-size: 11px;">Active</span>
                <span class="badge" style="background: rgba(201, 162, 39, 0.3); color: #f5e9c0; font-size: 11px;">In Good Standing</span>
              </div>
            </div>
            <div style="display: flex; flex-direction: column; gap: 6px;">
              <button class="btn btn-outline btn-sm" style="background: white; color: var(--royal); border: none; font-weight: 600;">📋 Share Credential</button>
              <button class="btn btn-primary btn-sm" style="background: var(--gold); color: var(--royal); border: none; font-weight: 600;" on:click={() => navigate('profile')}>View Profile</button>
            </div>
          </div>
        </div>
        
        <!-- Layout: Quick Actions & Notifications -->
        <div class="grid-2" style="gap: 16px; margin-bottom: 24px;">
          <!-- Quick Actions -->
          <div class="card">
            <div class="card-header"><span class="card-title">Quick Actions</span></div>
            <div class="card-body">
              {#if isAlumni}
                <div style="display: flex; flex-direction: column; gap: 8px;">
                  <button class="btn btn-outline" style="justify-content: flex-start;" on:click={() => navigate('documentRequest')}>
                    📄 Request Documents
                  </button>
                  <button class="btn btn-outline" style="justify-content: flex-start;" on:click={() => navigate('profile')}>
                    👤 View Profile
                  </button>
                </div>
              {:else}
                <div style="display: flex; flex-direction: column; gap: 8px;">
                  <button class="btn btn-outline" style="justify-content: flex-start;" on:click={() => navigate('process')}>
                    ⚙️ Process Documents
                  </button>
                  <button class="btn btn-outline" style="justify-content: flex-start;" on:click={() => navigate('payment')}>
                    💳 Verify Payments
                  </button>
                </div>
              {/if}
            </div>
          </div>
          
          <!-- Notifications -->
          <div class="card">
            <div class="card-header"><span class="card-title">Latest Notifications</span></div>
            <div class="card-body" style="max-height: 200px; overflow-y: auto;">
              {#if isAlumni}
                <div style="display: flex; gap: 10px; margin-bottom: 12px; padding-bottom: 12px; border-bottom: 1px solid var(--gray-200);">
                  <div style="font-size: 20px;">📋</div>
                  <div style="flex: 1; font-size: 12px;">
                    <div style="font-weight: 600; color: var(--royal);">Transcript Approved</div>
                    <div style="color: var(--gray-600);">Your official transcript of records is ready for download.</div>
                  </div>
                </div>
                <div style="display: flex; gap: 10px; padding-bottom: 12px; border-bottom: 1px solid var(--gray-200);">
                  <div style="font-size: 20px;">🔐</div>
                  <div style="flex: 1; font-size: 12px;">
                    <div style="font-weight: 600; color: var(--royal);">Biometrics Setup Success</div>
                    <div style="color: var(--gray-600);">Your fingerprint has been registered successfully.</div>
                  </div>
                </div>
              {:else}
                <div style="display: flex; gap: 10px; margin-bottom: 12px; padding-bottom: 12px; border-bottom: 1px solid var(--gray-200);">
                  <div style="font-size: 20px;">⚠️</div>
                  <div style="flex: 1; font-size: 12px;">
                    <div style="font-weight: 600; color: var(--warning);">Low Stock Alert</div>
                    <div style="color: var(--gray-600);">Binding Materials stock critically low (200 units).</div>
                  </div>
                </div>
                <div style="display: flex; gap: 10px;">
                  <div style="font-size: 20px;">💳</div>
                  <div style="flex: 1; font-size: 12px;">
                    <div style="font-weight: 600; color: var(--royal);">3 Payments Pending Verification</div>
                    <div style="color: var(--gray-600);">Review unmatched transactions in Payment Verification.</div>
                  </div>
                </div>
              {/if}
            </div>
          </div>
        </div>
        
        <!-- Stats Grid -->
        <div class="grid-4 mb-6">
          <div class="card stat-card">
            <div class="flex items-center justify-between">
              <div><div class="stat-label">{isAlumni ? 'Requests' : 'Pending'}</div><div class="stat-value">{isAlumni ? '3' : '5'}</div></div>
              <div class="stat-icon">{isAlumni ? '📄' : '⏳'}</div>
            </div>
            <div class="stat-sub">{isAlumni ? '2 ready' : 'Verifications'}</div>
          </div>
          <div class="card stat-card">
            <div class="flex items-center justify-between">
              <div><div class="stat-label">Verified</div><div class="stat-value">✓</div></div>
              <div class="stat-icon">🎓</div>
            </div>
            <div class="stat-sub">{isAlumni ? 'Identity confirmed' : '142 alumni'}</div>
          </div>
          <div class="card stat-card">
            <div class="flex items-center justify-between">
              <div><div class="stat-label">Completed</div><div class="stat-value">{isAlumni ? '12' : '24'}</div></div>
              <div class="stat-icon">✅</div>
            </div>
            <div class="stat-sub">{isAlumni ? 'All-time requests' : 'Ready'}</div>
          </div>
          <div class="card stat-card" style="background:var(--royal);color:white;">
            <div class="flex items-center justify-between">
              <div><div class="stat-label" style="color:rgba(255,255,255,0.6);">Status</div><div class="stat-value" style="color:white;font-size:20px;">Active</div></div>
              <div class="stat-icon">✓</div>
            </div>
            <div class="stat-sub" style="color:rgba(255,255,255,0.5);">Account active</div>
          </div>
        </div>

      {:else if currentPage === 'profile'}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">My Academic Passport</h2>
          <div class="flex gap-2">
            <button class="btn btn-outline btn-sm" on:click={handleDownloadPDF}>⬇ Download PDF</button>
            <button class="btn btn-primary btn-sm" on:click={handleShareCredential}>↗ Share Credential</button>
          </div>
        </div>
        
        <div class="grid-2 gap-4">
          <div>
            <div class="profile-hero">
              <div class="profile-hero-avatar">{initials}</div>
              <h2>{user?.name || 'User'}</h2>
              <p>Student ID: {user?.studentNumber}</p>
              <div class="verified-badge">✓ Verified by Registrar</div>
            </div>
            
            <div class="card mb-4">
              <div class="card-header"><span class="card-title">Academic Information</span></div>
              <div class="card-body" style="padding: 0;">
                <table>
                  <tbody>
                    <tr><td class="text-muted text-sm" style="width: 40%;">Program</td><td style="font-weight: 500;">Bachelor of Science in Computer Science</td></tr>
                    <tr><td class="text-muted text-sm">Graduation</td><td style="font-weight: 500;">March 2022</td></tr>
                    <tr><td class="text-muted text-sm">Latin Honor</td><td><span class="badge badge-royal">Cum Laude</span></td></tr>
                    <tr><td class="text-muted text-sm">GWA</td><td style="font-weight: 500;">1.45</td></tr>
                  </tbody>
                </table>
              </div>
            </div>
            
            <div class="card">
              <div class="card-header"><span class="card-title">Contact Information</span></div>
              <div class="card-body" style="padding: 0;">
                <table>
                  <tbody>
                    <tr><td class="text-muted text-sm" style="width: 35%;">Email</td><td>maria.santos@addu.edu.ph</td></tr>
                    <tr><td class="text-muted text-sm">Phone</td><td>+63 912 345 6789</td></tr>
                    <tr><td class="text-muted text-sm">City</td><td>Davao City, Philippines</td></tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
          
          <div>
            <div class="card mb-4">
              <div class="card-header"><span class="card-title">Ateneo de Davao University</span></div>
              <div class="card-body">
                <p style="font-size: 13px; color: var(--gray-600); line-height: 1.7;">
                  This credential is cryptographically secured and synced with the AdDU Registrar's Office. Share it instantly with employers for verification.
                </p>
                <div class="divider"></div>
                <div style="display: flex; align-items: center; gap: 10px;">
                  <div style="font-size: 13px; font-weight: 500;">Account Status</div>
                  <span class="badge badge-success" style="margin-left: auto;">Active</span>
                </div>
              </div>
            </div>
            
            <div class="card mb-4">
              <div class="card-header"><span class="card-title">Quick Actions</span></div>
              <div class="card-body">
                <button class="btn btn-outline w-full" style="justify-content: center; margin-bottom: 8px;" on:click={handleViewTranscript}>
                  📋 View Full Transcript
                </button>
                <button class="btn btn-primary w-full" style="justify-content: center;" on:click={handleManageBiometrics}>
                  🔏 Manage Biometrics
                </button>
              </div>
            </div>
            
            <div class="card" style="border: 2px solid var(--danger-bg);">
              <div class="card-body">
                <button class="btn btn-danger w-full" style="justify-content: center;" on:click={handleSignOut}>
                  ⬡ Sign Out
                </button>
              </div>
            </div>
          </div>
        </div>

      {:else if currentPage === 'documentRequest' && isAlumni}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Request Documents</h2>
        </div>
        
        <div class="tabs mb-6" style="max-width: 340px; margin-bottom: 20px;">
          <button class="tab-btn active">New Request</button>
          <button class="tab-btn">Request History</button>
        </div>
        
        <div class="grid-3" style="gap: 16px;">
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Transcript of Records')}>
            <div style="background: linear-gradient(135deg, var(--royal) 0%, var(--royal-mid) 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">📜</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--royal); margin-bottom: 4px; font-size: 13px;">Transcript of Records</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Official academic transcript with all courses and grades</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-royal" style="font-size: 12px;">₱150</span>
                <span style="font-size: 11px; color: var(--gray-400);">5–7 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Certified True Copy of Diploma')}>
            <div style="background: linear-gradient(135deg, var(--royal) 0%, var(--royal-mid) 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">🎓</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--royal); margin-bottom: 4px; font-size: 13px;">Certified Diploma Copy</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Authenticated copy of your diploma</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-royal" style="font-size: 12px;">₱200</span>
                <span style="font-size: 11px; color: var(--gray-400);">3–5 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Certificate of Graduation')}>
            <div style="background: linear-gradient(135deg, var(--success) 0%, #15a34a 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">📃</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--success); margin-bottom: 4px; font-size: 13px;">Certificate of Graduation</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Official certificate confirming degree completion</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-success" style="font-size: 12px;">Free</span>
                <span style="font-size: 11px; color: var(--gray-400);">2–3 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Certificate of Enrollment')}>
            <div style="background: linear-gradient(135deg, var(--success) 0%, #15a34a 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">📝</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--success); margin-bottom: 4px; font-size: 13px;">Certificate of Enrollment</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Proof of current or past enrollment</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-success" style="font-size: 12px;">Free</span>
                <span style="font-size: 11px; color: var(--gray-400);">1–2 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Certificate of General Weighted Average')}>
            <div style="background: linear-gradient(135deg, var(--success) 0%, #15a34a 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">📊</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--success); margin-bottom: 4px; font-size: 13px;">GWA Certificate</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Official GWA certification</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-success" style="font-size: 12px;">Free</span>
                <span style="font-size: 11px; color: var(--gray-400);">2–3 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Honorable Dismissal')}>
            <div style="background: linear-gradient(135deg, var(--royal) 0%, var(--royal-mid) 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">🏅</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--royal); margin-bottom: 4px; font-size: 13px;">Honorable Dismissal</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Good standing certificate for transfer</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-royal" style="font-size: 12px;">₱100</span>
                <span style="font-size: 11px; color: var(--gray-400);">2–3 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
          
          <div class="card doc-card" style="border-radius: 12px; overflow: hidden; cursor: pointer; transition: all 0.2s;" on:click={() => handleDocumentClick('Document Authentication')}>
            <div style="background: linear-gradient(135deg, var(--royal) 0%, var(--royal-mid) 100%); color: white; padding: 16px; text-align: center; font-size: 32px;">🔏</div>
            <div style="padding: 16px;">
              <div style="font-weight: 600; color: var(--royal); margin-bottom: 4px; font-size: 13px;">Document Authentication</div>
              <div style="font-size: 12px; color: var(--gray-600); margin-bottom: 12px;">Red ribbon verification service</div>
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
                <span class="badge badge-royal" style="font-size: 12px;">₱100</span>
                <span style="font-size: 11px; color: var(--gray-400);">7–15 days</span>
              </div>
              <button class="btn btn-primary btn-sm" style="width: 100%; justify-content: center; font-size: 12px;">Request ›</button>
            </div>
          </div>
        </div>

      {:else if currentPage === 'settings'}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Account Settings</h2>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-title">General Settings</span></div>
          <div class="card-body">
            <div class="form-group">
              <label class="form-label">Theme</label>
              <select class="form-select" on:change={(e) => handleThemeChange(e.target.value)}>
                <option value="light">Light (Default)</option>
                <option value="dark">Dark</option>
                <option value="auto">Auto</option>
              </select>
            </div>
            <div class="form-group">
              <label class="form-label">Notifications</label>
              <div style="display: flex; align-items: center; gap: 8px;">
                <input type="checkbox" checked={emailNotifications} on:change={handleNotificationToggle}> Email notifications
              </div>
            </div>
          </div>
        </div>

      {:else if currentPage === 'verification' && !isAlumni}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Profile Verification</h2>
          <span class="badge badge-info">5 Pending</span>
        </div>
        
        <div class="grid-2 mb-6" style="grid-template-columns: repeat(2, 1fr); max-width: 320px;">
          <div class="card" style="background: var(--info-bg); border-color: var(--info); padding: 16px 20px;">
            <div style="font-size: 11px; font-weight: 600; text-transform: uppercase; color: var(--info); letter-spacing: .06em;">Pending</div>
            <div style="font-size: 28px; font-weight: 700; color: var(--info); font-family: 'DM Serif Display', serif;" >5</div>
            <div style="font-size: 11px; color: var(--info);">Awaiting verification</div>
          </div>
          
          <div class="card" style="background: var(--success-bg); border-color: var(--success); padding: 16px 20px;">
            <div style="font-size: 11px; font-weight: 600; text-transform: uppercase; color: var(--success); letter-spacing: .06em;">Verified</div>
            <div style="font-size: 28px; font-weight: 700; color: var(--success); font-family: 'DM Serif Display', serif;">142</div>
            <div style="font-size: 11px; color: var(--success);">Confirmed profiles</div>
          </div>
        </div>
        
        <div class="card">
          <div class="card-header"><span class="card-title">Pending Verifications</span></div>
          <div class="card-body" style="padding: 0;">
            <table>
              <thead>
                <tr>
                  <th>Student ID</th>
                  <th>Name</th>
                  <th>Program</th>
                  <th>Date Submitted</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">20190034</td>
                  <td>Maria Clara Santos</td>
                  <td>BS Computer Science</td>
                  <td>Feb 14, 2025</td>
                  <td><button class="btn btn-primary btn-sm" on:click={() => handleVerify('20190034')}>✓ Verify</button></td>
                </tr>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">20190156</td>
                  <td>Juan Dela Cruz</td>
                  <td>BS Accountancy</td>
                  <td>Feb 13, 2025</td>
                  <td><button class="btn btn-primary btn-sm" on:click={() => handleVerify('20190156')}>✓ Verify</button></td>
                </tr>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">20190289</td>
                  <td>Ana Maria Reyes</td>
                  <td>BS Business</td>
                  <td>Feb 12, 2025</td>
                  <td><button class="btn btn-primary btn-sm" on:click={() => handleVerify('20190289')}>✓ Verify</button></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

      {:else if currentPage === 'process' && !isAlumni}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Process Documents</h2>
        </div>
        
        <div class="grid-3 mb-6" style="gap: 16px;">
          <div class="card" style="background: var(--info-bg); border-color: var(--info); padding: 16px;">
            <div style="font-size: 11px; font-weight: 600; text-transform: uppercase; color: var(--info);">Pending</div>
            <div style="font-size: 24px; font-weight: 700; color: var(--info); font-family: 'DM Serif Display', serif;">2</div>
            <div style="font-size: 11px; color: var(--info);">Batches ready</div>
          </div>
          <div class="card" style="background: var(--warning-bg); border-color: var(--warning); padding: 16px;">
            <div style="font-size: 11px; font-weight: 600; text-transform: uppercase; color: var(--warning);">In Progress</div>
            <div style="font-size: 24px; font-weight: 700; color: var(--warning); font-family: 'DM Serif Display', serif;">8</div>
            <div style="font-size: 11px; color: var(--warning);">Currently processing</div>
          </div>
          <div class="card" style="background: var(--success-bg); border-color: var(--success); padding: 16px;">
            <div style="font-size: 11px; font-weight: 600; text-transform: uppercase; color: var(--success);">Completed</div>
            <div style="font-size: 24px; font-weight: 700; color: var(--success); font-family: 'DM Serif Display', serif;">24</div>
            <div style="font-size: 11px; color: var(--success);">Ready to deliver</div>
          </div>
        </div>
        
        <div class="card">
          <div class="card-header"><span class="card-title">Active Batch Processing</span></div>
          <div class="card-body">
            <div style="margin-bottom: 20px;">
              <div style="display: flex; justify-content: space-between; margin-bottom: 8px;">
                <span style="font-weight: 500;">Transcript of Records</span>
                <span style="color: var(--gray-600); font-size: 13px;">300 of 667 sets</span>
              </div>
              <div style="height: 8px; background: var(--gray-100); border-radius: 4px; overflow: hidden;">
                <div style="height: 100%; background: linear-gradient(90deg, var(--royal) 0%, var(--royal-mid) 100%); width: 45%;"></div>
              </div>
            </div>
            <button class="btn btn-primary" style="width: 100%; justify-content: center;" on:click={() => handleProcessDocument('Transcript of Records')}>
              ▶ Resume Processing
            </button>
          </div>
        </div>

      {:else if currentPage === 'inventory' && !isAlumni}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Inventory Alerts</h2>
        </div>
        
        <div class="grid-2 gap-4 mb-6">
          <div class="card" style="border-left: 4px solid var(--success);">
            <div style="font-size: 12px; color: var(--success); font-weight: 600; margin-bottom: 4px;">✓ SUFFICIENT</div>
            <div style="font-size: 14px; font-weight: 600; margin-bottom: 2px;">Diploma Paper</div>
            <div style="font-size: 12px; color: var(--gray-600);">Reorder from: 500 units</div>
            <div style="font-size: 11px; color: var(--gray-400); margin-top: 8px;">Current stock: 2,450 units</div>
            <button class="btn btn-outline btn-sm" style="width: 100%; margin-top: 12px; justify-content: center;">
              ✓ OK / Check
            </button>
          </div>
          
          <div class="card" style="border-left: 4px solid var(--danger);">
            <div style="font-size: 12px; color: var(--danger); font-weight: 600; margin-bottom: 4px;">⚠ CRITICAL</div>
            <div style="font-size: 14px; font-weight: 600; margin-bottom: 2px;">Binding Materials</div>
            <div style="font-size: 12px; color: var(--danger);">Critical from: 667 units</div>
            <div style="font-size: 11px; color: var(--gray-400); margin-top: 8px;">Current stock: 200 units</div>
            <button class="btn btn-danger btn-sm" style="width: 100%; margin-top: 12px; justify-content: center;" on:click={handleOrderSupplies}>
              🛒 Reorder Now
            </button>
          </div>
        </div>

      {:else if currentPage === 'payment' && !isAlumni}
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Payment Verification</h2>
          <span class="badge badge-warning">3 Pending</span>
        </div>
        
        <div class="card">
          <div class="card-header"><span class="card-title">Unmatched Payments</span></div>
          <div class="card-body" style="padding: 0;">
            <table>
              <thead>
                <tr>
                  <th>Reference Code</th>
                  <th>Amount</th>
                  <th>Date</th>
                  <th>Method</th>
                  <th>Action</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">#GCash-2024-01988</td>
                  <td>₱2,500</td>
                  <td>Feb 14, 2025</td>
                  <td><span class="badge badge-info">GCash</span></td>
                  <td><button class="btn btn-primary btn-sm" on:click={() => handleMatchPayment('#GCash-2024-01988')}>✓ Verified</button></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="section-header mb-6">
          <h2 style="font-family:'DM Serif Display',serif;font-size:20px;color:var(--royal);">Document Log</h2>
          <div class="flex gap-2">
            <select class="form-select" style="max-width: 200px;">
              <option>All Documents</option>
              <option>Transcript of Records</option>
              <option>Diploma Copies</option>
              <option>Certificates</option>
            </select>
          </div>
        </div>
        
        <div class="card">
          <div class="card-header"><span class="card-title">Recent Document Requests</span></div>
          <div class="card-body" style="padding: 0;">
            <table>
              <thead>
                <tr>
                  <th>Request ID</th>
                  <th>Document Type</th>
                  <th>Alumni</th>
                  <th>Date</th>
                  <th>Status</th>
                  <th>Amount</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">TOR-2024-0342</td>
                  <td>Transcript of Records</td>
                  <td>Maria Clara Santos</td>
                  <td>Feb 14, 2025</td>
                  <td><span class="badge badge-warning">Processing</span></td>
                  <td>₱150</td>
                </tr>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">CDP-2024-0148</td>
                  <td>Certified Diploma Copy</td>
                  <td>Juan Dela Cruz</td>
                  <td>Feb 13, 2025</td>
                  <td><span class="badge badge-info">Verified</span></td>
                  <td>₱200</td>
                </tr>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">COG-2024-1571</td>
                  <td>Certificate of Graduation</td>
                  <td>Ana Maria Reyes</td>
                  <td>Feb 12, 2025</td>
                  <td><span class="badge badge-success">Ready</span></td>
                  <td>Free</td>
                </tr>
                <tr>
                  <td style="font-family: monospace; font-size: 12px;">HON-2024-0089</td>
                  <td>Honorable Dismissal</td>
                  <td>Carlos Santos</td>
                  <td>Feb 11, 2025</td>
                  <td><span class="badge badge-success">Delivered</span></td>
                  <td>₱100</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  h2 {
    margin: 0;
  }
  
  .mt-4 {
    margin-top: 16px;
  }
</style>

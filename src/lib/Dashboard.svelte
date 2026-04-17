<script>
  import { createEventDispatcher } from 'svelte';
  import MyProfile from './MyProfile.svelte';
  import DocumentRequest from './DocumentRequest.svelte';
  import StaffDashboard from './StaffDashboard.svelte';
  
  const dispatch = createEventDispatcher();
  export let user;
  
  let activeModule = 'home';

  const isStaff = user.role === 'Office Staff';
</script>

<div class="dashboard">
  <aside class="sidebar">
    <div class="sidebar-header">
      <div class="logo">🛡️</div>
      <h2>Alumni Hub</h2>
      <p class="user-info">{user.role}</p>
      <p class="student-number">{user.studentNumber}</p>
    </div>
    
    <nav class="sidebar-nav">
      {#if !isStaff}
        <!-- Alumni Navigation -->
        <button 
          class={activeModule === 'home' ? 'nav-btn active' : 'nav-btn'} 
          on:click={() => activeModule = 'home'}
        >
          🏠 Home
        </button>
        <button 
          class={activeModule === 'profile' ? 'nav-btn active' : 'nav-btn'} 
          on:click={() => activeModule = 'profile'}
        >
          👤 My Profile
        </button>
        <button 
          class={activeModule === 'documents' ? 'nav-btn active' : 'nav-btn'} 
          on:click={() => activeModule = 'documents'}
        >
          📄 Document Request
        </button>
      {:else}
        <!-- Staff Navigation -->
        <button 
          class={activeModule === 'staff' ? 'nav-btn active' : 'nav-btn'} 
          on:click={() => activeModule = 'staff'}
        >
          📊 Dashboard
        </button>
      {/if}
    </nav>
    
    <div class="sidebar-footer">
      <button class="logout-btn" on:click={() => dispatch('logout')}>
        🚪 Sign Out
      </button>
    </div>
  </aside>

  <main class="content">
    {#if activeModule === 'home'}
      <div class="home-screen">
        <header class="content-header">
          <h1>Welcome, <span class="blue-knight">{user.role === 'Office Staff' ? 'Staff Member' : 'Blue Knight'}</span></h1>
          <p class="subtitle">Ateneo de Davao Alumni Portal</p>
        </header>

        {#if !isStaff}
          <!-- Alumni Home Screen -->
          <div class="quick-actions">
            <div class="action-card">
              <button on:click={() => activeModule = 'documents'}>
                <span class="action-icon">📝</span>
                <span class="action-title">Request Documents</span>
                <p>TOR, Diplomas, Certificates & More</p>
              </button>
            </div>

            <div class="action-card">
              <button on:click={() => activeModule = 'profile'}>
                <span class="action-icon">🛡️</span>
                <span class="action-title">My Profile</span>
                <p>View Digital Academic Passport</p>
              </button>
            </div>

            <div class="action-card">
              <button on:click={() => activeModule = 'profile'}>
                <span class="action-icon">🔐</span>
                <span class="action-title">Verify Identity</span>
                <p>Biometric Authentication</p>
              </button>
            </div>
          </div>

          <div class="features-section">
            <h3>Key Features</h3>
            <div class="features-grid">
              <div class="feature">
                <div class="feature-icon">✓</div>
                <h4>Instant Verification</h4>
                <p>Verify with biometrics instantly</p>
              </div>
              <div class="feature">
                <div class="feature-icon">⚡</div>
                <h4>Rush Processing</h4>
                <p>Get documents in 48 hours</p>
              </div>
              <div class="feature">
                <div class="feature-icon">📱</div>
                <h4>100% Digital</h4>
                <p>Complete from your phone</p>
              </div>
              <div class="feature">
                <div class="feature-icon">🔒</div>
                <h4>Secure Sharing</h4>
                <p>QR-code authenticated documents</p>
              </div>
            </div>
          </div>
        {:else}
          <!-- Staff Home Screen -->
          <div class="staff-welcome">
            <p>Welcome to the Registrar/Finance Staff Dashboard</p>
            <button class="btn-primary" on:click={() => activeModule = 'staff'}>
              📊 Open Dashboard
            </button>
          </div>
        {/if}
      </div>

    {:else if activeModule === 'profile'}
      <MyProfile {user} on:logout={(e) => dispatch('logout', e.detail)} />

    {:else if activeModule === 'documents'}
      <DocumentRequest {user} />

    {:else if activeModule === 'staff'}
      <StaffDashboard {user} on:logout={(e) => dispatch('logout', e.detail)} />

    {/if}
  </main>
</div>

<style>
  .dashboard {
    display: flex;
    min-height: 100vh;
    background: var(--gray-light);
  }

  /* Sidebar */
  .sidebar {
    width: 250px;
    background: white;
    border-right: 1px solid var(--gray-medium);
    display: flex;
    flex-direction: column;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  }

  .sidebar-header {
    background: linear-gradient(135deg, #0052CC 0%, #003BA3 100%);
    color: white;
    padding: 2rem 1rem;
    text-align: center;
  }

  .logo {
    font-size: 2.5rem;
    margin-bottom: 0.75rem;
  }

  .sidebar-header h2 {
    margin: 0 0 0.5rem 0;
    font-size: 1.4rem;
  }

  .user-info {
    font-size: 0.85rem;
    margin: 0.25rem 0;
    opacity: 0.9;
  }

  .student-number {
    font-size: 0.8rem;
    margin: 0;
    opacity: 0.8;
    font-family: monospace;
  }

  /* Navigation */
  .sidebar-nav {
    flex: 1;
    padding: 1rem 0;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .nav-btn {
    padding: 0.9rem 1rem;
    background: none;
    border: none;
    text-align: left;
    color: var(--text-secondary);
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s;
    border-left: 3px solid transparent;
  }

  .nav-btn:hover {
    background: var(--gray-light);
    color: var(--royal-blue);
  }

  .nav-btn.active {
    background: #E8F0FF;
    color: var(--royal-blue);
    border-left-color: var(--royal-blue);
    font-weight: 600;
  }

  /* Sidebar Footer */
  .sidebar-footer {
    padding: 1rem;
    border-top: 1px solid var(--gray-medium);
  }

  .logout-btn {
    width: 100%;
    padding: 0.75rem;
    background: var(--gray-light);
    border: 1px solid var(--gray-medium);
    border-radius: 6px;
    color: var(--text-secondary);
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .logout-btn:hover {
    background: var(--danger-red);
    color: white;
    border-color: var(--danger-red);
  }

  /* Main Content */
  .content {
    flex: 1;
    overflow-y: auto;
    padding: 2rem;
  }

  /* Home Screen */
  .home-screen {
    max-width: 1000px;
  }

  .content-header {
    margin-bottom: 2rem;
  }

  .content-header h1 {
    color: #003BA3;
    font-size: 2rem;
    margin: 0 0 0.5rem 0;
  }

  .blue-knight {
    color: var(--royal-blue);
    font-weight: 800;
  }

  .subtitle {
    color: var(--text-secondary);
    margin: 0;
  }

  /* Quick Actions */
  .quick-actions {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-bottom: 2rem;
  }

  .action-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
  }

  .action-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
  }

  .action-card button {
    width: 100%;
    padding: 2rem 1.5rem;
    background: linear-gradient(135deg, #E8F0FF 0%, #f5f8ff 100%);
    border: 2px solid var(--royal-blue);
    border-radius: 0;
    color: #003BA3;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 0.75rem;
    font-size: 1rem;
  }

  .action-card button:hover {
    background: linear-gradient(135deg, #d4e5ff 0%, #e8f0ff 100%);
    border-color: #003BA3;
  }

  .action-icon {
    font-size: 2.5rem;
  }

  .action-title {
    font-weight: 700;
    font-size: 1.1rem;
  }

  .action-card p {
    margin: 0;
    font-size: 0.85rem;
    opacity: 0.8;
    font-weight: 500;
  }

  /* Features Section */
  .features-section {
    margin-top: 3rem;
  }

  .features-section h3 {
    color: #003BA3;
    margin-bottom: 1.5rem;
    font-size: 1.3rem;
  }

  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
  }

  .feature {
    background: white;
    border-radius: 12px;
    padding: 1.5rem;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .feature-icon {
    font-size: 2rem;
    margin-bottom: 0.75rem;
    display: block;
  }

  .feature h4 {
    color: #003BA3;
    margin: 0.5rem 0;
  }

  .feature p {
    margin: 0;
    color: var(--text-secondary);
    font-size: 0.9rem;
  }

  /* Staff Welcome */
  .staff-welcome {
    background: white;
    border-radius: 12px;
    padding: 3rem;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .staff-welcome p {
    color: #003BA3;
    font-size: 1.2rem;
    margin-bottom: 1.5rem;
  }

  .btn-primary {
    background: var(--royal-blue);
    color: white;
    padding: 0.85rem 2rem;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-primary:hover {
    background: #003BA3;
    transform: translateY(-2px);
  }

  /* Responsive Design */
  @media (max-width: 768px) {
    .dashboard {
      flex-direction: column;
    }

    .sidebar {
      width: 100%;
      border-right: none;
      border-bottom: 1px solid var(--gray-medium);
    }

    .sidebar-nav {
      flex-direction: row;
      flex-wrap: wrap;
      gap: 0;
    }

    .nav-btn {
      flex: 1;
      border-bottom: 3px solid transparent;
      border-left: none;
      padding: 0.75rem 0.5rem;
      text-align: center;
      font-size: 0.85rem;
    }

    .nav-btn.active {
      border-bottom-color: var(--royal-blue);
    }

    .content {
      padding: 1rem;
    }

    .content-header h1 {
      font-size: 1.5rem;
    }

    .quick-actions {
      grid-template-columns: 1fr;
    }

    .features-grid {
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    }
  }
</style>
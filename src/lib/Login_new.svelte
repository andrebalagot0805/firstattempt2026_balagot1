<script>
  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  
  let selectedRole = 'alumni';
  let studentNumber = '';
  let password = '';
  let step = 1; // 1 for role selection, 2 for credentials
  
  function selectRole(role) {
    selectedRole = role;
  }
  
  function handleLogin() {
    if (!studentNumber || !password) {
      alert('Please fill in all fields');
      return;
    }
    
    const user = {
      id: studentNumber,
      role: selectedRole,
      name: selectedRole === 'alumni' ? 'Maria Clara Santos' : 'Juan Dela Cruz',
      studentNumber: studentNumber
    };
    
    dispatch('login', user);
  }
</script>

<div class="login-wrap">
  <div class="login-bg-decor"></div>
  
  <div class="login-card">
    <div class="login-logo">
      <div class="login-emblem">A</div>
      <h1>Welcome, <span>Blue Knight</span></h1>
      <p>Alumni Knights' Hub — Ateneo de Davao University</p>
    </div>
    
    <div class="role-cards">
      <div 
        class="role-card" 
        class:selected={selectedRole === 'alumni'}
        on:click={() => selectRole('alumni')}
      >
        <div class="role-icon">🎓</div>
        <div class="role-name">Alumni</div>
        <div class="role-desc">Graduate portal access</div>
      </div>
      
      <div 
        class="role-card" 
        class:selected={selectedRole === 'staff'}
        on:click={() => selectRole('staff')}
      >
        <div class="role-icon">🏛️</div>
        <div class="role-name">Office Staff</div>
        <div class="role-desc">Registrar / Finance</div>
      </div>
    </div>
    
    <div style="margin-top: 20px;">
      <div class="form-group">
        <label class="form-label" for="studentNum">
          {selectedRole === 'alumni' ? 'Student Number' : 'Employee Number'}
        </label>
        <input 
          id="studentNum"
          class="form-input" 
          bind:value={studentNumber}
          placeholder={selectedRole === 'alumni' ? 'e.g. 20190034' : 'e.g. 10179752'}
        />
      </div>
      
      <div class="form-group">
        <label class="form-label" for="pwd">Password</label>
        <input 
          id="pwd"
          class="form-input" 
          type="password" 
          bind:value={password}
          placeholder="Enter your password"
        />
      </div>
      
      <div style="text-align: right; margin-bottom: 16px;">
        <a href="#" style="font-size: 12.5px; color: var(--royal); text-decoration: none;">
          Forgot password?
        </a>
      </div>
      
      <button 
        class="btn btn-primary w-full" 
        style="justify-content: center; padding: 12px;"
        on:click={handleLogin}
      >
        Sign In
      </button>
    </div>
    
    <div class="login-divider"><span>or</span></div>
    
    <button class="google-btn">
      <svg width="18" height="18" viewBox="0 0 18 18">
        <path fill="#4285F4" d="M17.64 9.2c0-.637-.057-1.251-.164-1.84H9v3.481h4.844a4.14 4.14 0 01-1.796 2.716v2.259h2.908c1.702-1.567 2.684-3.875 2.684-6.615z"/>
        <path fill="#34A853" d="M9 18c2.43 0 4.467-.806 5.956-2.18l-2.908-2.259c-.806.54-1.837.86-3.048.86-2.344 0-4.328-1.584-5.036-3.711H.957v2.332A8.997 8.997 0 009 18z"/>
        <path fill="#FBBC05" d="M3.964 10.71A5.41 5.41 0 013.682 9c0-.593.102-1.17.282-1.71V4.958H.957A8.996 8.996 0 000 9c0 1.452.348 2.827.957 4.042l3.007-2.332z"/>
        <path fill="#EA4335" d="M9 3.58c1.321 0 2.508.454 3.44 1.345l2.582-2.58C13.463.891 11.426 0 9 0A8.997 8.997 0 00.957 4.958L3.964 6.29C4.672 4.163 6.656 3.58 9 3.58z"/>
      </svg>
      Sign in using university email
    </button>
    
    <div class="login-footer">
      Don't have an account? <a href="#">Register</a>
    </div>
  </div>
</div>

<style>
  .login-wrap {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(140deg, var(--royal-dark) 0%, var(--royal) 60%, var(--royal-mid) 100%);
    position: relative;
    overflow: hidden;
  }

  .login-bg-decor {
    position: absolute;
    inset: 0;
    opacity: 0.04;
    background-image: 
      radial-gradient(circle at 20% 50%, white 1px, transparent 1px),
      radial-gradient(circle at 80% 20%, white 1px, transparent 1px);
    background-size: 60px 60px;
  }

  .login-card {
    background: white;
    border-radius: 20px;
    box-shadow: 0 24px 64px rgba(0, 0, 0, 0.25);
    width: 420px;
    padding: 40px;
    position: relative;
    z-index: 1;
  }

  .login-logo {
    text-align: center;
    margin-bottom: 28px;
  }

  .login-emblem {
    width: 64px;
    height: 64px;
    background: var(--royal);
    border-radius: 16px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-family: 'DM Serif Display', serif;
    font-size: 28px;
    color: var(--gold);
    margin-bottom: 14px;
    font-weight: 700;
  }

  .login-logo h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 22px;
    color: var(--royal);
    margin-bottom: 4px;
  }

  .login-logo h1 span {
    color: var(--gold);
  }

  .login-logo p {
    font-size: 12.5px;
    color: var(--gray-400);
    margin: 0;
  }

  .role-cards {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-top: 10px;
  }

  .role-card {
    padding: 16px;
    border: 2px solid var(--gray-200);
    border-radius: 10px;
    cursor: pointer;
    text-align: center;
    transition: all 0.15s;
  }

  .role-card:hover,
  .role-card.selected {
    border-color: var(--royal);
    background: var(--royal-light);
  }

  .role-icon {
    font-size: 28px;
    margin-bottom: 6px;
  }

  .role-name {
    font-size: 13px;
    font-weight: 600;
    color: var(--royal);
  }

  .role-desc {
    font-size: 11px;
    color: var(--gray-400);
    margin-top: 3px;
  }

  .login-divider {
    display: flex;
    align-items: center;
    gap: 12px;
    margin: 18px 0;
  }

  .login-divider::before,
  .login-divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--gray-200);
  }

  .login-divider span {
    font-size: 12px;
    color: var(--gray-400);
  }

  .google-btn {
    width: 100%;
    padding: 10px;
    border: 1px solid var(--gray-200);
    border-radius: 8px;
    background: white;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    cursor: pointer;
    font-size: 13.5px;
    font-family: inherit;
    font-weight: 500;
    color: var(--gray-800);
    transition: background 0.15s;
  }

  .google-btn:hover {
    background: var(--gray-50);
  }

  .login-footer {
    text-align: center;
    margin-top: 20px;
    font-size: 12.5px;
    color: var(--gray-400);
  }

  .login-footer a {
    color: var(--royal);
    text-decoration: none;
    font-weight: 500;
  }

  @media (max-width: 480px) {
    .login-card {
      width: 100%;
      margin: 16px;
      padding: 32px 20px;
    }
    
    .role-cards {
      grid-template-columns: 1fr;
    }
  }
</style>

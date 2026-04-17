<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  let studentNumber = '';
  let password = '';
  let selectedRole = 'Alumni';

  function submitLogin(e) {
    e.preventDefault();
    if (studentNumber && password) {
      dispatch('login', { studentNumber, role: selectedRole });
    } else {
      alert("Please enter valid credentials.");
    }
  }

  function handleBiometricLogin() {
    alert("📱 Scanning biometrics (FaceID/Fingerprint)...\n✅ Biometric login successful! Linking to your account...");
    // Simulate successful biometric login
    dispatch('login', { studentNumber: 'BIOMETRIC_20190034', role: selectedRole, useBiometric: true });
  }

  function handleGoogleSignIn() {
    dispatch('googleSignIn');
  }
</script>

<div class="login-container">
  <div class="login-box">
    <!-- Logo -->
    <div class="logo-placeholder">🛡️</div>
    
    <!-- Welcome Text -->
    <h1>Welcome, <span class="blue-knight">Blue Knight</span></h1>
    <p class="subtitle">Alumni Portal — Ateneo de Davao University</p>
    
    <!-- Role Selection Cards -->
    <div class="role-cards">
      <div 
        class="role-card alumni"
        class:selected={selectedRole === 'Alumni'}
        on:click={() => selectedRole = 'Alumni'}
      >
        <div class="icon">🎓</div>
        <div class="role-name">Alumni</div>
        <div class="role-description">Graduate portal access</div>
      </div>
      
      <div 
        class="role-card office-staff"
        class:selected={selectedRole === 'Office Staff'}
        on:click={() => selectedRole = 'Office Staff'}
      >
        <div class="icon">🏛️</div>
        <div class="role-name">Office Staff</div>
        <div class="role-description">Registrar / Finance</div>
      </div>
    </div>
    
    <!-- Form -->
    <form on:submit={submitLogin}>
      <div class="form-group">
        <label for="studentNumber">Student Number</label>
        <input 
          type="text" 
          id="studentNumber" 
          bind:value={studentNumber} 
          placeholder="e.g. 20190034" 
          required 
        />
      </div>
      
      <div class="form-group">
        <label for="password">Password</label>
        <input 
          type="password" 
          id="password" 
          bind:value={password} 
          placeholder="Enter your password" 
          required 
        />
      </div>
      
      <div class="forgot-link">
        <a href="#forgot">Forgot password?</a>
      </div>
      
      <button type="submit" class="signin-btn">Sign In</button>
    </form>
    
    <!-- Biometric Login -->
    <div class="divider">or</div>
    
    <button type="button" class="biometric-btn" on:click={handleBiometricLogin}>
      👆 Biometric Login (FaceID/Fingerprint)
    </button>
    
    <div class="divider">or</div>
    
    <!-- Google Sign In -->
    <button type="button" class="google-btn" on:click={handleGoogleSignIn}>
      🔗 Sign in using university email
    </button>
    
    <!-- Register Link -->
    <p class="register-link">Don't have an account? <a href="#register">Register</a></p>
  </div>
</div>

<style>
  .login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: linear-gradient(135deg, #0052CC 0%, #003BA3 100%);
    padding: 20px;
  }

  .login-box {
    background: white;
    padding: 3rem 2.5rem;
    border-radius: 16px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    width: 100%;
    max-width: 480px;
    text-align: center;
  }

  /* Logo */
  .logo-placeholder {
    width: 80px;
    height: 80px;
    background: linear-gradient(135deg, #0052CC 0%, #003BA3 100%);
    border-radius: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.5rem;
    margin: 0 auto 2rem;
  }

  /* Welcome Text */
  h1 {
    font-size: 1.8rem;
    color: #003BA3;
    margin: 0 0 0.5rem 0;
    font-weight: 700;
  }

  .blue-knight {
    color: #0052CC;
    font-weight: 800;
  }

  .subtitle {
    color: #666666;
    font-size: 0.9rem;
    margin: 0 0 2rem 0;
  }

  /* Role Selection Cards */
  .role-cards {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .role-card {
    padding: 1.2rem;
    border: 2px solid #E0E0E0;
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.3s ease;
    background: white;
  }

  .role-card:hover {
    border-color: #0052CC;
    background: #E8F0FF;
  }

  .role-card.selected {
    border-color: #0052CC;
    background: #E8F0FF;
  }

  .role-card .icon {
    font-size: 2rem;
    margin-bottom: 0.5rem;
  }

  .role-card .role-name {
    font-weight: 600;
    color: #003BA3;
    font-size: 0.95rem;
    margin-bottom: 0.25rem;
  }

  .role-card .role-description {
    color: #999999;
    font-size: 0.8rem;
  }

  /* Form Groups */
  .form-group {
    text-align: left;
    margin-bottom: 1.5rem;
  }

  .form-group label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
    color: #333333;
    font-size: 0.9rem;
  }

  .form-group input {
    width: 100%;
    padding: 0.85rem;
    border: 1px solid #D1D5DB;
    border-radius: 8px;
    font-size: 0.95rem;
    transition: border-color 0.3s, box-shadow 0.3s;
    box-sizing: border-box;
  }

  .form-group input:focus {
    outline: none;
    border-color: #0052CC;
    box-shadow: 0 0 0 3px rgba(0, 82, 204, 0.1);
  }

  .form-group input::placeholder {
    color: #BDBDBD;
  }

  /* Forgot Password Link */
  .forgot-link {
    text-align: right;
    margin-bottom: 1.5rem;
  }

  .forgot-link a {
    color: #0052CC;
    text-decoration: none;
    font-size: 0.85rem;
    font-weight: 500;
    transition: color 0.3s;
  }

  .forgot-link a:hover {
    color: #003BA3;
    text-decoration: underline;
  }

  /* Sign In Button */
  .signin-btn {
    width: 100%;
    padding: 0.95rem;
    background: linear-gradient(135deg, #0052CC 0%, #003BA3 100%);
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    margin-bottom: 1rem;
  }

  .signin-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(0, 82, 204, 0.2);
  }

  .signin-btn:active {
    transform: translateY(0);
  }

  /* Biometric Button */
  .biometric-btn {
    width: 100%;
    padding: 0.95rem;
    background: white;
    color: #0052CC;
    border: 2px solid #0052CC;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    margin-bottom: 1rem;
  }

  .biometric-btn:hover {
    background: #E8F0FF;
    transform: translateY(-2px);
  }

  /* Divider */
  .divider {
    margin: 1rem 0;
    color: #BDBDBD;
    font-size: 0.85rem;
    font-weight: 500;
  }

  /* Google Sign In Button */
  .google-btn {
    width: 100%;
    padding: 0.95rem;
    border: 1px solid #D1D5DB;
    background: white;
    color: #333333;
    border-radius: 8px;
    font-size: 0.95rem;
    font-weight: 600;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    transition: all 0.3s;
    margin-bottom: 1.5rem;
  }

  .google-btn:hover {
    background: #F8F9FA;
    border-color: #0052CC;
  }

  /* Register Link */
  .register-link {
    font-size: 0.9rem;
    color: #666666;
    margin: 1.5rem 0 0 0;
  }

  .register-link a {
    color: #0052CC;
    text-decoration: none;
    font-weight: 600;
    transition: color 0.3s;
  }

  .register-link a:hover {
    color: #003BA3;
    text-decoration: underline;
  }

  @media (max-width: 600px) {
    .login-box {
      padding: 2rem 1.5rem;
    }

    h1 {
      font-size: 1.5rem;
    }

    .role-cards {
      grid-template-columns: 1fr;
    }
  }
</style>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Login / Signup</title>
  <style>
    /* Simple, modern styles inspired by your sketch */
    :root{
      --bg: #f4f7fb;
      --card: #ffffff;
      --accent: #2b79d9; /* blue accent (you like blue) */
      --muted: #7b7b88;
      --success: #2aa147;
      --danger: #d93b3b;
      --radius: 14px;
      --shadow: 0 6px 20px rgba(43,121,217,0.08);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }

     html,body{
      height:100%;
      margin:0;
      background: linear-gradient(180deg,#eef6ff 0%, var(--bg) 100%);
      display:flex;
      align-items:center;
      justify-content:center;
    }

    .card {
      width: min(420px, 92%);
      background: var(--card);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 28px;
      box-sizing: border-box;
      border: 1px solid rgba(43,121,217,0.06);
    }

    .title {
      display:flex;
      align-items:baseline;
      justify-content:space-between;
      gap:12px;
      margin-bottom:18px;
    }

    .title h1 {
      font-size:20px;
      margin:0;
      color: #173b6a;
      letter-spacing: 0.2px;
    }

    .mode-toggle {
      font-size:13px;
      color:var(--muted);
      background:transparent;
      border:none;
      cursor:pointer;
      text-decoration:underline;
      padding:0;
    }

    form {
      display:flex;
      flex-direction:column;
      gap:14px;
    }

    label {
      font-size:13px;
      color:#2e3a4a;
      display:block;
      margin-bottom:6px;
    }

    .field {
      position:relative;
    }

    input[type="text"],
    input[type="password"],
    input[type="email"]{
      width:100%;
      padding:12px 44px 12px 12px;
      border-radius:10px;
      border: 1px solid rgba(20,30,40,0.08);
      background: linear-gradient(180deg, #fff, #fbfdff);
      box-sizing:border-box;
      font-size:14px;
      outline:none;
      transition: box-shadow .15s, border-color .15s;
    }

    input:focus{
      box-shadow: 0 6px 20px rgba(43,121,217,0.08);
      border-color: var(--accent);
    }

    .eye-toggle {
      position:absolute;
      right:10px;
      top:50%;
      transform:translateY(-50%);
      background:transparent;
      border:none;
      cursor:pointer;
      font-size:13px;
      color:var(--muted);
    }

    .links {
      display:flex;
      justify-content:space-between;
      align-items:center;
      font-size:13px;
      margin-top:4px;
    }

    .links a {
      color:var(--accent);
      text-decoration:none;
      cursor:pointer;
    }

    .links a:hover { text-decoration:underline; }

    button.primary {
      margin-top:6px;
      padding:12px;
      border-radius:10px;
      border: none;
      background: linear-gradient(90deg,var(--accent), #1d61b4);
      color:white;
      font-weight:600;
      font-size:15px;
      cursor:pointer;
      box-shadow: 0 8px 20px rgba(43,121,217,0.12);
    }

    button.primary:active { transform:translateY(1px); }

    .helper {
      font-size:13px;
      color:var(--muted);
      text-align:center;
      margin-top:12px;
    }

    .message {
      margin-top:8px;
      padding:10px 12px;
      border-radius:8px;
      font-size:13px;
      display:none;
    }

    .message.success {
      background: rgba(42,161,71,0.08);
      color: var(--success);
      border: 1px solid rgba(42,161,71,0.12);
      display:block;
    }

    .message.error {
      background: rgba(217,59,59,0.07);
      color: var(--danger);
      border: 1px solid rgba(217,59,59,0.12);
      display:block;
    }

    /* small note near bottom like the sketch's curved bracket */
    .sketch-note {
      margin-top:14px;
      text-align:left;
      color:var(--muted);
      font-size:12px;
    }

    @media (max-width:420px){
      .card{ padding:20px; border-radius:12px;}
      .title h1{ font-size:18px;}
    }
  </style>
</head>
<body>
  <main class="card" aria-live="polite">
    <div class="title">
      <h1 id="formTitle">Login / Signup</h1>
      <button class="mode-toggle" id="toggleMode" aria-pressed="false">Switch to Signup</button>
    </div>

    <form id="authForm" autocomplete="on" novalidate>
      <div>
        <label for="username">Username</label>
        <div class="field">
          <input id="username" name="username" type="text" placeholder="Enter username" required aria-required="true" />
        </div>
      </div>

      <div>
        <label for="password">Password</label>
        <div class="field">
          <input id="password" name="password" type="password" placeholder="Enter password" required aria-required="true" minlength="6" />
          <button type="button" class="eye-toggle" id="togglePw" aria-label="Show password">Show</button>
        </div>
      </div>

      <!-- Confirm password (only visible in signup mode) -->
      <div id="confirmWrap" style="display:none;">
        <label for="confirmPassword">Confirm Password</label>
        <div class="field">
          <input id="confirmPassword" name="confirmPassword" type="password" placeholder="Re-type password" />
          <button type="button" class="eye-toggle" data-target="confirmPassword" aria-label="Show confirm password">Show</button>
        </div>
      </div>

      <div class="links">
        <a href="#" id="forgotLink">Forgot password?</a>
        <a href="#" id="registerLink">Register</a>
      </div>

      <button class="primary" id="submitBtn" type="submit">Login</button>

      <div class="message success" id="successMsg" role="status" style="display:none;"></div>
      <div class="message error" id="errorMsg" role="alert" style="display:none;"></div>

      <div class="helper">Keep your account safe — don't share your password with anyone.</div>
      <div class="sketch-note">Design based on provided sketch — Login / Signup card with options.</div>
    </form>
  </main>

  <script>
    // DOM references
    const toggleMode = document.getElementById('toggleMode');
    const formTitle = document.getElementById('formTitle');
    const authForm = document.getElementById('authForm');
    const confirmWrap = document.getElementById('confirmWrap');
    const submitBtn = document.getElementById('submitBtn');
    const successMsg = document.getElementById('successMsg');
    const errorMsg = document.getElementById('errorMsg');
    const registerLink = document.getElementById('registerLink');
    const forgotLink = document.getElementById('forgotLink');

    let mode = 'login'; // or 'signup'

    function setMode(newMode) {
      mode = newMode;
      if (mode === 'signup') {
        formTitle.textContent = 'Signup';
        toggleMode.textContent = 'Switch to Login';
        toggleMode.setAttribute('aria-pressed','true');
        confirmWrap.style.display = '';
        submitBtn.textContent = 'Create account';
        registerLink.style.display = 'none';
      } else {
        formTitle.textContent = 'Login / Signup';
        toggleMode.textContent = 'Switch to Signup';
        toggleMode.setAttribute('aria-pressed','false');
        confirmWrap.style.display = 'none';
        submitBtn.textContent = 'Login';
        registerLink.style.display = '';
      }
      clearMessages();
    }

    toggleMode.addEventListener('click', (e) => {
      setMode(mode === 'login' ? 'signup' : 'login');
    });

    // Clicking "Register" link sets signup mode
    registerLink.addEventListener('click', (e) => {
      e.preventDefault();
      setMode('signup');
      // focus username to help the user
      document.getElementById('username').focus();
    });

    forgotLink.addEventListener('click', (e) => {
      e.preventDefault();
      clearMessages();
      const username = document.getElementById('username').value.trim();
      if (!username) {
        showError('Enter your username first so we can help reset password.');
        return;
      }
      // Fake password-reset flow (client side demo)
      showSuccess('Password reset link sent to the email associated with "' + escapeHtml(username) + '". (Demo)');
    });

    // show/hide password toggles (works for both password fields)
    document.querySelectorAll('.eye-toggle').forEach(btn => {
      btn.addEventListener('click', (ev) => {
        const targetId = btn.dataset.target || 'password';
        const input = document.getElementById(targetId);
        if (!input) return;
        if (input.type === 'password') {
          input.type = 'text';
          btn.textContent = 'Hide';
          btn.setAttribute('aria-label','Hide password');
        } else {
          input.type = 'password';
          btn.textContent = 'Show';
          btn.setAttribute('aria-label','Show password');
        }
      });
    });

    // Form submit
    authForm.addEventListener('submit', (e) => {
      e.preventDefault();
      clearMessages();

      const username = document.getElementById('username').value.trim();
      const password = document.getElementById('password').value;
      const confirmPasswordInput = document.getElementById('confirmPassword');

      if (!username) {
        showError('Please enter your username.');
        return;
      }
      if (!password || password.length < 6) {
        showError('Password must be at least 6 characters.');
        return;
      }

      if (mode === 'signup') {
        const confirm = confirmPasswordInput.value;
        if (!confirm) {
          showError('Please confirm your password.');
          return;
        }
        if (password !== confirm) {
          showError('Passwords do not match.');
          return;
        }
        // Demo: pretend to create account
        showSuccess('Account created for "' + escapeHtml(username) + '". You can now login.');
        // switch back to login after short delay (demo)
        setMode('login');
      } else {
        // Demo login validation (client-side only)
        // In real life send credentials to your server using fetch() over HTTPS
        // Here we just show a success if password length >=6
        showSuccess('Welcome back, ' + escapeHtml(username) + '!');
      }
      authForm.reset();
    });

    function showError(msg){
      errorMsg.textContent = msg;
      errorMsg.style.display = 'block';
      successMsg.style.display = 'none';
    }
    function showSuccess(msg){
      successMsg.textContent = msg;
      successMsg.style.display = 'block';
      errorMsg.style.display = 'none';
    }
    function clearMessages(){
      errorMsg.style.display = 'none';
      successMsg.style.display = 'none';
    }

    // small helper to prevent XSS in demo messages
    function escapeHtml(unsafe) {
      return unsafe.replace(/[&<"'>]/g, c => ({
        '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'
      }[c]));
    }

    // initialize default mode
    setMode('login');
  </script>
</body>
</html>
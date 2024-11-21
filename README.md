<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Sean Ezeocha - Portfolio</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/0.158.0/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
        /* Previous CSS remains the same */
        body {
            background-color: #0a1f0a;
            color: #ffffff;
            overflow-x: hidden;
        }

        #login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        #login-box {
            background: #4CAF50;
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        #login-box input {
            margin: 10px 0;
            padding: 10px;
            width: 100%;
            border: none;
            border-radius: 5px;
        }

        #edit-mode-indicator {
            position: fixed;
            top: 10px;
            right: 10px;
            background: rgba(76, 175, 80, 0.8);
            color: white;
            padding: 5px 10px;
            border-radius: 5px;
            z-index: 1000;
            display: none;
        }

        .editable {
            border: 2px dashed transparent;
            transition: border 0.3s;
        }

        .editable:hover {
            border-color: #4CAF50;
        }

        .edit-controls {
            display: none;
            position: absolute;
            top: -30px;
            right: 0;
            background: #4CAF50;
            padding: 5px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div id="login-overlay">
        <div id="login-box">
            <h2>Secure Login</h2>
            <input type="password" id="password" placeholder="Enter Password">
            <button onclick="attemptLogin()">Login</button>
        </div>
    </div>

    <div id="edit-mode-indicator">Edit Mode Active</div>

    <!-- Rest of the previous HTML remains the same, but with editable class added to sections -->
    <div class="content">
        <section id="home" class="section editable">
            <div class="edit-controls">
                <button onclick="enableSectionEdit(this)">Edit</button>
                <button onclick="saveSection(this)">Save</button>
            </div>
            <h1 contenteditable="false">Sean Ezeocha</h1>
            <p contenteditable="false">Aspiring Computer Science Student | Class of 2024</p>
            <!-- Rest of the section remains the same -->
        </section>

        <!-- Similar modifications for other sections -->
    </div>

    <script>
        // Secure login logic
        const MASTER_PASSWORD_HASH = "Babyboy"; // Replace with actual hash

        function attemptLogin() {
            const passwordInput = document.getElementById('password');
            const enteredPassword = passwordInput.value;
            
            // Hash the entered password
            const hashedPassword = CryptoJS.SHA256(enteredPassword).toString();

            if (hashedPassword === MASTER_PASSWORD_HASH) {
                document.getElementById('login-overlay').style.display = 'none';
                enableEditMode();
            } else {
                alert('Incorrect Password');
                passwordInput.value = '';
            }
        }

        function enableEditMode() {
            document.getElementById('edit-mode-indicator').style.display = 'block';
            
            // Show edit controls for editable sections
            document.querySelectorAll('.editable').forEach(section => {
                const editControls = section.querySelector('.edit-controls');
                if (editControls) {
                    editControls.style.display = 'block';
                }
            });
        }

        function enableSectionEdit(button) {
            const section = button.closest('.editable');
            const editableElements = section.querySelectorAll('h1, p, .highlights');
            
            editableElements.forEach(el => {
                el.setAttribute('contenteditable', 'true');
                el.style.border = '2px dashed #4CAF50';
            });
        }

        function saveSection(button) {
            const section = button.closest('.editable');
            const editableElements = section.querySelectorAll('[contenteditable="true"]');
            
            editableElements.forEach(el => {
                el.setAttribute('contenteditable', 'false');
                el.style.border = 'none';
            });

            // Here you would typically save the content to a backend
            // For now, we'll just log the changes
            console.log('Section updated:', section.innerHTML);
            alert('Changes saved locally');
        }

        // Prevent right-click save/view source
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
        });

        // Disable developer tools
        document.addEventListener('keydown', function(e) {
            if (e.key === 'F12' || (e.ctrlKey && e.shiftKey && e.key === 'I')) {
                e.preventDefault();
            }
        });

        // Rest of the previous Three.js animation script remains the same
    </script>
</body>
</html>

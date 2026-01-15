
<html lang="de">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Landingpage</title>
    <style>
        :root{--bg:#f6f8fb;--card:#fff;--accent:#2563eb}
        body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;background:var(--bg);color:#0f172a}
        .wrap{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:2rem}
        .card{background:var(--card);padding:2.5rem;border-radius:12px;box-shadow:0 6px 24px rgba(15,23,42,0.08);max-width:720px;width:100%}
        h1{margin:0 0 .5rem;font-size:clamp(1.5rem,3vw,2.25rem)}
        h2{margin:0 0 1.25rem;color:#475569;font-weight:500}
        form{display:flex;gap:.5rem;flex-wrap:wrap}
        input[type="email"]{flex:1;min-width:200px;padding:.75rem 1rem;border:1px solid #e6eef8;border-radius:8px;font-size:1rem}
        button{background:var(--accent);color:#fff;border:0;padding:.75rem 1rem;border-radius:8px;font-weight:600;cursor:pointer}
        .note{margin-top:1rem;color:#64748b;font-size:.95rem}
        .success{margin-top:.75rem;color:green}
        @media (max-width:460px){form{flex-direction:column}button{width:100%}}
    </style>
</head>
<body>
    <main class="wrap">
        <section class="card" aria-labelledby="main-title">
            <h1 id="main-title">Willkommen auf unserer Landingpage</h1>
            <h2>Erfahre Neuigkeiten und Updates direkt per E‑Mail</h2>

            <form id="newsletter" novalidate>
                <label for="email" class="sr-only">E‑Mail Adresse</label>
                <input id="email" name="email" type="email" placeholder="Deine E‑Mail" required aria-required="true" />
                <button type="submit">Anmelden</button>
            </form>

            <div id="feedback" role="status" aria-live="polite" class="note"></div>
            <p class="note">Wir geben deine Daten nicht weiter. Du kannst dich jederzeit abmelden.</p>
        </section>
    </main>

    <script>
        const form = document.getElementById('newsletter');
        const email = document.getElementById('email');
        const feedback = document.getElementById('feedback');

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            feedback.textContent = '';
            if (!email.checkValidity()) {
                feedback.textContent = 'Bitte gib eine gültige E‑Mail Adresse ein.';
                feedback.className = 'note';
                email.focus();
                return;
            }
            // Simulierter Absendevorgang (hier an deinen Server anpassen)
            feedback.textContent = 'Danke! Deine Anmeldung war erfolgreich.';
            feedback.className = 'success';
            form.reset();
        });
    </script>
</body>
</html>


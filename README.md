<!doctype html>
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

            <!-- Admin Bereich: Nur sichtbar, wenn E-Mails gespeichert wurden -->
            <div id="admin-controls" style="margin-top: 2rem; border-top: 1px solid #eee; padding-top: 1rem; display: none;">
                <h3 style="font-size: 1rem; margin-bottom: 0.5rem; color: #475569;">Admin: Gespeicherte E-Mails</h3>
                <div style="display:flex; gap:0.5rem;">
                    <button id="downloadBtn" type="button" style="background: #64748b; font-size: 0.85rem;">Liste herunterladen (.csv)</button>
                    <button id="clearBtn" type="button" style="background: #ef4444; font-size: 0.85rem;">Liste löschen</button>
                </div>
            </div>
        </section>
    </main>

    <script>
        const form = document.getElementById('newsletter');
        const email = document.getElementById('email');
        const feedback = document.getElementById('feedback');
        const adminControls = document.getElementById('admin-controls');
        const downloadBtn = document.getElementById('downloadBtn');
        const clearBtn = document.getElementById('clearBtn');

        // Prüfen, ob Daten im Speicher liegen und Admin-Bereich anzeigen
        function updateAdminVisibility() {
            const stored = localStorage.getItem('newsletter_emails');
            adminControls.style.display = stored ? 'block' : 'none';
        }

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            feedback.textContent = '';
            if (!email.checkValidity()) {
                feedback.textContent = 'Bitte gib eine gültige E‑Mail Adresse ein.';
                feedback.className = 'note';
                email.focus();
                return;
            }

            // 1. Bestehende Daten laden
            const currentData = JSON.parse(localStorage.getItem('newsletter_emails') || '[]');
            
            // 2. Neue E-Mail hinzufügen
            currentData.push({
                email: email.value,
                date: new Date().toLocaleString('de-DE')
            });
            
            // 3. Zurück in den Speicher schreiben
            localStorage.setItem('newsletter_emails', JSON.stringify(currentData));

            feedback.textContent = 'Danke! Deine Anmeldung wurde lokal gespeichert.';
            feedback.className = 'success';
            form.reset();
            updateAdminVisibility();
        });

        // Funktion zum Herunterladen der Liste
        downloadBtn.addEventListener('click', () => {
            const data = JSON.parse(localStorage.getItem('newsletter_emails') || '[]');
            if (!data.length) return;

            let csvContent = "data:text/csv;charset=utf-8,Datum,Email\n";
            data.forEach(row => {
                csvContent += `${row.date},${row.email}\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "newsletter_liste.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        });

        // Funktion zum Löschen der Liste
        clearBtn.addEventListener('click', () => {
            if(confirm('Möchtest du die Liste wirklich löschen?')) {
                localStorage.removeItem('newsletter_emails');
                updateAdminVisibility();
            }
        });

        // Beim Laden der Seite prüfen
        updateAdminVisibility();
    </script>
</body>
</html>

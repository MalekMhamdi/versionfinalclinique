# MediClinic — Clinique Intelligente

Plateforme de gestion de clinique : rendez-vous, dossiers médicaux, ordonnances, avis sur les médecins, notifications par email et assistant d'orientation IA.

| Dossier | Rôle | Techno | Port |
|---|---|---|---|
| `back/` | API REST + SignalR | .NET 8, EF Core, SQL Server LocalDB | 5217 |
| `frontclinic/` | Interface web (admin, médecin, patient) | Angular 21 | 4200 |
| `clinique-ia-agent/` | Agent IA d'orientation | Python, FastAPI, Groq | 8000 |

## Prérequis

- .NET 8 SDK + outil EF Core : `dotnet tool install --global dotnet-ef`
- SQL Server LocalDB (installé avec Visual Studio)
- Node.js 20+
- Python 3.12+
- Une clé API Groq : https://console.groq.com/keys

## Configuration (à faire une seule fois)

Les secrets ne sont pas dans le dépôt. Copiez les fichiers d'exemple :

```bash
# Backend : clé JWT + SMTP (mettre Email.Enabled à "true" pour envoyer de vrais emails)
cp back/ClinicIntelligente.WebApi/appsettings.Development.example.json \
   back/ClinicIntelligente.WebApi/appsettings.Development.json

# Agent IA : clé Groq
cp clinique-ia-agent/.env.example clinique-ia-agent/.env
```

Pour Gmail, utilisez un « mot de passe d'application » (Compte Google → Sécurité → Validation en deux étapes → Mots de passe des applications).

## Lancement

Ouvrez trois terminaux.

**1. Backend**
```bash
cd back
dotnet ef database update --project ClinicIntelligente.Infrastructure.Persistence --startup-project ClinicIntelligente.WebApi
dotnet run --project ClinicIntelligente.WebApi --launch-profile http
```
Swagger : http://localhost:5217/swagger

**2. Agent IA**
```bash
cd clinique-ia-agent
python -m venv .venv
.venv\Scripts\activate        # Windows  (Linux/macOS : source .venv/bin/activate)
pip install -r requirements.txt
python -m uvicorn app.backend:app --port 8000
```
Docs : http://localhost:8000/docs

**3. Frontend**
```bash
cd frontclinic
npm install
npm start
```
Application : http://localhost:4200

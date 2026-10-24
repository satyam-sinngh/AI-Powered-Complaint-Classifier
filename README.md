# AI-Powered Complaint Classifier

An AI-powered customer complaint management application built with **Streamlit**, **Python**, **SQLite**, and the **Groq API**. It classifies complaints into departments, stores them in a local database, supports keyword search and CSV export, and attempts to email the administrator every time a complaint is successfully saved.

![AI-Powered Complaint Classifier](https://github.com/user-attachments/assets/b02fa651-8bfc-4dcd-abe2-c87839c1a645)

## ✨ Features

- **AI-powered complaint classification:** Uses a Groq-hosted language model to classify each complaint into one of the configured departments.
- **Confidence score:** Displays the confidence score returned by the classification model.
- **Email notifications:** Attempts to send an administrator notification for every successfully saved complaint, regardless of the confidence score.
- **SQLite storage:** Persists complaint records in a local SQLite database.
- **Complaint search:** Find complaints by the complainant's name or words in the complaint text.
- **Complaint details:** View individual records in expandable sections.
- **CSV export:** Download all saved complaints for further analysis.
- **Dark-themed interface:** Includes custom CSS styling and sidebar navigation.
- **Input validation:** Checks required fields, email format, and 10-digit mobile numbers.

## 📸 Demo

### Complaint submission

![Complaint Submission Flow](https://github.com/user-attachments/assets/afbc6903-c0e0-4217-9ce2-2cd17a17d044)

### Complaint management

![Complaint Management Dashboard](https://github.com/user-attachments/assets/bad559ce-bd56-47ab-9b4a-ea68031c25f9)

## 🧰 Technology Stack

| Component          | Technology                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| User interface     | Streamlit, custom CSS                                                                             |
| Language           | Python 3.8+                                                                                       |
| Database           | SQLite                                                                                            |
| AI classification  | Groq API and the model configured in `app.py` (currently `qwen/qwen3.8-27b` in the supplied code) |
| Email              | Gmail SMTP, `smtplib`, `email.mime`                                                               |
| Data processing    | Pandas, regular expressions                                                                       |
| Sidebar navigation | `streamlit-option-menu`                                                                           |

## 🚀 Installation

### Prerequisites

- Python 3.8 or newer
- A Groq API key
- A Gmail account with 2-Step Verification and an App Password if you use Gmail SMTP
- Git (to clone the repository)

### 1. Clone the repository

```bash
git clone https://github.com/satyam-sinngh/AI-Powered-Complaint-Classifier.git
cd AI-Powered-Complaint-Classifier
```

### 2. Create and activate a virtual environment

**Windows (Git Bash):**

```bash
python -m venv .venv
source .venv/Scripts/activate
```

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS/Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure application secrets

Create a `.streamlit` folder in the project directory, then create a file named `secrets.toml` inside it.

**File path:** `.streamlit/secrets.toml`

```toml
[email]
sender_email = "your-gmail@gmail.com"
sender_password = "your-gmail-app-password"
admin_email = "admin@example.com"

[groq]
api_key = "your-groq-api-key"
```

Replace the example values with your own credentials. For Gmail, use a Google [App Password](https://support.google.com/accounts/answer/185833), not your normal Gmail password.

> **Security:** Never commit `.streamlit/secrets.toml`, API keys, passwords, or real complaint data to GitHub. If a credential has been exposed, revoke it and generate a new one.

### 5. Run the application

```bash
streamlit run app.py
```

Streamlit will display a local URL in the terminal. Open that URL in your browser.

## 📝 Usage

### Submit a complaint

1. Choose **Submit Complaint** from the sidebar.
2. Enter the required personal details and complaint description.
3. Click **Submit Complaint**.
4. Review the predicted department and confidence score.
5. The app saves the complaint to SQLite and then attempts to send an email notification to the configured administrator.

### View and search complaints

1. Choose **View Complaints** from the sidebar.
2. Enter a name or a keyword from the complaint and click **Search**.
3. Select **Load All Complaints** to view all saved records.
4. Use the CSV download link to export the records.

## ⚙️ Configuration

### Departments

The department list is defined in `app.py`:

```python
DEPARTMENTS = [
    "Technical Support",
    "Billing",
    "Customer Service",
    "Shipping",
    "General Queries",
]
```

Edit this list to change the categories used by the classifier.

### Email notifications

The administrator recipient is configured with `admin_email` in `.streamlit/secrets.toml`. The app attempts to send the notification for every successfully saved complaint, not only for low-confidence predictions. If email delivery fails, the complaint remains saved and the app displays a warning.

Email delivery depends on valid SMTP credentials, connectivity, and the email provider's security settings.

## 🏗️ System Architecture

```text
User
  |
  v
Streamlit Interface
  |
  +--> Validate complaint form
  |
  +--> Groq API --> Department and confidence score
  |
  +--> SQLite --> Save and retrieve complaints
  |
  +--> Gmail SMTP --> Administrator email notification
  |
  +--> Search and CSV export
```

### Classification workflow

1. The user enters complaint details in the form.
2. The complaint text is sent to the configured Groq model.
3. The model response is parsed to obtain a department and confidence score.
4. The complaint is saved to the SQLite database.
5. The app attempts to notify the administrator by email.

## 🔐 Security notes

- Keep API keys and email credentials in `.streamlit/secrets.toml`, not in source code.
- Ensure `.streamlit/secrets.toml`, `.env`, virtual environments, and local database files are listed in `.gitignore`.
- If a key or password was posted publicly or committed, revoke it and replace it. Removing it in a later commit does not remove it from earlier Git history.
- Complaint submissions contain personal information; restrict access to the app and its database appropriately.

## ⚠️ Current limitations

- Complaint search is a keyword search against the name and complaint text. It does not currently perform semantic search.
- SQLite is a local database file. A deployment with temporary storage may require a persistent database service.
- Classification results and confidence values are model-generated and should be reviewed for important decisions.
- Email notifications are attempted after a complaint is saved; successful storage does not guarantee successful email delivery.

## 🤝 Contributing

Contributions and suggestions are welcome.

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit your changes: `git commit -m "feat: describe your change"`.
4. Push the branch: `git push origin feature/your-feature`.
5. Open a pull request.

## 📄 License

No license is currently specified. Add a `LICENSE` file if you want to define terms for using, modifying, and distributing this project.

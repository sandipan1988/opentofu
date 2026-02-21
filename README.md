# Policy Engine 🛡️

A comprehensive Terraform security policy checker with authentication, API key management, and CI/CD integration for validating infrastructure-as-code against 60+ security best practices.

## ✨ Features

### 🎯 Core Capabilities
- **60+ Security Rules**: Comprehensive checks across 9 AWS resource types
- **Severity Classification**: CRITICAL, HIGH, MEDIUM, LOW risk levels
- **Smart Parser**: Handles both Terraform files and code strings
- **REST API**: Full-featured API for CI/CD integration
- **CLI Tool**: Command-line interface for local validation

### 🔐 Authentication & Security
- **WhatsApp OTP Authentication**: Passwordless signup and login via WhatsApp
- **User Authentication**: Secure login system with session management
- **API Key Management**: Generate and manage API keys for automation
- **Protected Endpoints**: All APIs require authentication
- **Role-based Access**: User profile and key management
- **Dual Auth Methods**: WhatsApp OTP or traditional password login

### 🌐 Web Dashboard
- **Interactive UI**: Modern, responsive web interface
- **Real-time Checking**: Paste and validate Terraform code instantly
- **Rules Management**: Browse, view, and generate new security rules
- **Results Viewer**: Detailed violation reports with severity grouping
- **Statistics Dashboard**: Track checks, violations, and trends
- **Profile Management**: Generate and manage API keys

### 🔄 CI/CD Integration
- **GitHub Actions**: Ready-to-use workflow examples
- **GitLab CI**: Pipeline configuration templates
- **Jenkins**: Jenkinsfile with full integration
- **Azure DevOps**: Pipeline YAML examples
- **CircleCI**: Configuration examples
- **Docker Support**: Containerized deployment
- **Health Checks**: Monitoring endpoints for orchestration

## 🚀 Quick Start

### Using Docker Compose (Recommended)

The fastest way to get started is with Docker Compose, which sets up the complete stack including:
- Policy Engine application
- PostgreSQL database
- Redis for caching and rate limiting
- Prometheus for monitoring
- Grafana for visualization

**1. Start the stack:**

```bash
cd deployments
docker-compose up -d
```

**2. Access the services:**

- **Policy Engine Dashboard**: http://localhost:5000
- **Grafana**: http://localhost:3000 (admin/admin)
- **Prometheus**: http://localhost:9090

**3. Check status:**

```bash
docker-compose ps
```

**4. View logs:**

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f app
```

**5. Stop the stack:**

```bash
docker-compose down

# Remove volumes (clean state)
docker-compose down -v
```

### Cloud Deployment (Render.com)

Deploy the complete stack to Render.com with one click using the provided blueprint:

**1. Prerequisites:**
- A Render account (free tier available)
- Git repository connected to Render

**2. Deploy:**
```bash
# Commit the render.yaml file
git add render.yaml .renderignore runtime.txt Procfile
git commit -m "Add Render deployment configuration"
git push
```

**3. In Render Dashboard:**
- Click "New +" → "Blueprint"
- Select your repository
- Render will automatically detect `render.yaml`
- Click "Apply" to deploy all services

**4. Services Created:**
- Web Service (Flask app with gunicorn)
- PostgreSQL database (automatically connected)
- Redis instance (automatically connected)

**5. Access Your App:**
- Your app will be available at `https://policy-engine.onrender.com` (or similar)
- Health check: `https://your-app.onrender.com/health`

📖 **Full Guide**: See [RENDER_DEPLOYMENT.md](RENDER_DEPLOYMENT.md) for detailed deployment instructions, troubleshooting, and configuration options.

### Using Dev Container (VS Code)

For the best development experience with full IDE integration:

**1. Prerequisites:**
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Install [VS Code](https://code.visualstudio.com/)
- Install the "Dev Containers" extension in VS Code

**2. Open in Container:**
- Open the project in VS Code
- Press `F1` and select **"Dev Containers: Reopen in Container"**
- Wait for the container to build (first time only)

**3. Everything is ready!**
- Python environment with all dependencies installed
- PostgreSQL, Redis, and Prometheus running
- VS Code extensions pre-configured
- Port forwarding automatically set up

**4. Run the application:**
```bash
./start-dev.sh
# or
python app.py
```

**5. Access services:**
- Flask App: http://localhost:5000
- PostgreSQL: `postgres:5432` (inside container)
- Redis: `redis:6379` (inside container)
- Prometheus: http://localhost:9090

See [.devcontainer/README.md](.devcontainer/README.md) for more details.

### Manual Installation

If you prefer to run without Docker:

**1. Install Dependencies**

```bash
pip install -r requirements.txt
```

**2. Start the Server**

```bash
python app.py
```

Server starts at: http://localhost:5000

### Using the Dashboard

**1. Login**

**Option A: WhatsApp OTP (Recommended)**
1. Click "Sign up with WhatsApp" for new users
2. Enter your phone number (with country code)
3. Receive a 6-digit code via WhatsApp
4. Enter code to complete signup/login
5. No password needed!

**Option B: Password Login (Admin)**
- Navigate to http://localhost:5000
- **Username:** `sysadmin_pe`
- **Password:** `PolicyEngine@2026!Secure`
- ⚠️ **Change the password after first login!**

**2. Generate API Key**

1. Login to the web dashboard
2. Go to **Profile** page
3. Click **"Generate New API Key"**
4. Copy and save the API key securely

**3. Use the API**

```bash
# Using CLI tool
python cli_check.py main.tf --api-key YOUR_API_KEY

# Or with environment variable
export POLICY_API_KEY=YOUR_API_KEY
python cli_check.py main.tf
```

## 📦 Installation

```bash
# Clone the repository
git clone <repository-url>
cd policy-engine

# Install dependencies
pip install -r requirements.txt

# (Optional) Configure Twilio for WhatsApp OTP
cp .env.example .env
# Edit .env with your Twilio credentials
# Get free credentials at https://www.twilio.com/try-twilio

# Or use TEST MODE (no Twilio needed for development)
# OTP codes will be printed to console
```

## 💻 Usage

### Web Dashboard

Start the server:

```bash
python app.py
```

Navigate to: http://localhost:5000

**Pages:**
- `/` - Main dashboard with statistics
- `/login` - Login page (default: sysadmin_pe/PolicyEngine@2026!Secure)
- `/profile` - User profile and API key management
- `/check` - Interactive policy checker
- `/rules` - Rules management interface
- `/results/<id>` - Detailed check results

### API Endpoints

**Public Endpoints:**
- `GET /health` - Health check
- `GET /api/version` - API version info

**Protected Endpoints (require API key):**
- `POST /api/check` - Check Terraform code
- `GET /api/policies` - List all policies
- `GET /api/rules` - Get all rules
- `GET /api/stats` - Get statistics
- `GET /api/history` - Get check history

**Authentication:**
```bash
# Using X-API-Key header
curl -H "X-API-Key: YOUR_API_KEY" http://localhost:5000/api/stats

# Or Authorization header
curl -H "Authorization: Bearer YOUR_API_KEY" http://localhost:5000/api/stats
```

### CLI Tool

Check Terraform files from command line:

```bash
# Check a single file
python cli_check.py main.tf --api-key YOUR_API_KEY

# Using environment variable
export POLICY_API_KEY=YOUR_API_KEY
python cli_check.py terraform/main.tf

# Custom server URL
python cli_check.py main.tf --server https://policy.example.com
```

**Exit Codes:**
- `0` - All checks passed ✅
- `1` - Policy violations found ⚠️
- `2` - Error occurred ❌

### CI/CD Integration

**GitHub Actions:**
```yaml
- name: Check Terraform
  env:
    POLICY_API_KEY: ${{ secrets.POLICY_API_KEY }}
  run: python cli_check.py main.tf --server ${{ secrets.POLICY_SERVER }}
```

**Jenkins:**
```groovy
environment {
    POLICY_API_KEY = credentials('policy-api-key')
}
steps {
    sh 'python cli_check.py main.tf'
}
```

See [CI_CD_INTEGRATION.md](CI_CD_INTEGRATION.md) for complete examples.

Output:
```
======================================================================
SECURITY POLICY CHECK RESULTS
======================================================================
Total violations found: 9

HIGH SEVERITY (1 issues):
----------------------------------------------------------------------
  [HIGH] aws_s3_bucket.data
           S3 bucket is public (acl: public-read) - HIGH RISK

LOW SEVERITY (8 issues):
----------------------------------------------------------------------
  [LOW] aws_s3_bucket.data
           S3 bucket does not block public ACLs
  ...
```

### REST API

Start the Flask server:

```bash
python app.py
```

The API will be available at `http://127.0.0.1:5000`

#### API Endpoints

**GET /api/policies** - Get list of all policies
```bash
curl http://127.0.0.1:5000/api/policies
```

**POST /api/check** - Check Terraform code
```bash
curl -X POST http://127.0.0.1:5000/api/check \
  -H "Content-Type: application/json" \
  -d '{"code": "resource \"aws_s3_bucket\" \"test\" { bucket = \"my-bucket\" acl = \"public-read\" }"}'
```

Response:
```json
{
  "passed": false,
  "violations": [
    {
      "resource_type": "aws_s3_bucket",
      "resource_name": "test",
      "violation": "S3 bucket is public"
    }
  ]
}
```

### Testing

Test the API endpoints:

```bash
python test_api.py
```

## Current Rules

### S3 Bucket Rules
- ✓ Blocks public-read ACL
- ✓ Blocks public-read-write ACL
- ✓ Requires block_public_acls enabled

### RDS Database Rules
- ✓ Requires encryption at rest

## File Structure

```
policy-engine/
├── app.py                      # Flask web server with authentication
├── auth.py                     # Authentication and API key management
├── parser.py                   # Terraform file parser using python-hcl2
├── rules.py                    # Security rule definitions
├── policy_checker.py           # Core policy checking logic
├── cli_check.py                # CLI tool for CI/CD integration
├── test_api.py                 # API integration tests
├── main.tf                     # Example Terraform file
├── requirements.txt            # Python dependencies
├── .env.example                # Environment configuration template
├── users.json                  # User accounts (created on first run)
├── api_keys.json               # API keys storage (created on first run)
├── templates/                  # HTML templates
│   ├── login.html              # Login page
│   ├── profile.html            # User profile and API keys
│   ├── dashboard.html          # Main dashboard
│   ├── check.html              # Policy checker page
│   ├── rules.html              # Rules management
│   └── results.html            # Check results viewer
├── examples/                   # Example scripts
│   ├── cicd_example.py         # Python CI/CD integration example
│   └── check_terraform.sh      # Bash CI/CD script
└── docs/
    ├── API_AUTHENTICATION.md   # API authentication guide
    ├── CI_CD_INTEGRATION.md    # CI/CD integration examples
    └── SECURITY_RULES.md       # Security rules documentation
```

## 🔐 Security Best Practices

### For Production Deployment

1. **Change Default Credentials**
   - Change the default admin password immediately
   - Create strong passwords for all users

2. **Secure API Keys**
   - Never commit API keys to source control
   - Use environment variables or secrets management
   - Rotate keys regularly
   - Revoke unused keys

3. **Server Configuration**
   - Set `DEBUG=false` in production
   - Use a strong `SECRET_KEY` (generate with `python -c "import secrets; print(secrets.token_hex(32))"`)
   - Enable HTTPS (use nginx or similar as reverse proxy)
   - Set appropriate session timeout

4. **Storage**
   - Replace file-based storage with proper database
   - Implement backup strategies
   - Secure file permissions

5. **Monitoring**
   - Set up logging and monitoring
   - Track API usage and patterns
   - Alert on suspicious activity

## 🔄 CI/CD Integration

This policy engine is designed for seamless CI/CD integration. See [CI_CD_INTEGRATION.md](CI_CD_INTEGRATION.md) for complete guides including:

- ✅ GitHub Actions workflows
- ✅ GitLab CI pipelines
- ✅ Jenkins pipelines
- ✅ Azure DevOps pipelines
- ✅ CircleCI configuration
- ✅ Docker integration
- ✅ Pre-commit hooks

**Quick Example:**
```bash
# In your CI/CD pipeline
export POLICY_API_KEY=${{ secrets.POLICY_API_KEY }}
python cli_check.py terraform/*.tf --server https://policy.example.com

# Exit code 0 = pass, 1 = violations, 2 = error
```

## 📚 Documentation

- **[WHATSAPP_OTP.md](WHATSAPP_OTP.md)** - WhatsApp OTP authentication guide
- **[API_AUTHENTICATION.md](API_AUTHENTICATION.md)** - Complete authentication guide
- **[CI_CD_INTEGRATION.md](CI_CD_INTEGRATION.md)** - CI/CD integration examples
- **[SECURITY_RULES.md](SECURITY_RULES.md)** - Security rules documentation
- **[DASHBOARD.md](DASHBOARD.md)** - Web dashboard guide

## Example Terraform File

```hcl
# main.tf
resource "aws_s3_bucket" "data" {
  bucket = "my-data"
  acl    = "public-read"
}
```

Running the checker:
```bash
python cli_check.py main.tf
```

Output:
```
======================================================================
Terraform Policy Check Results - main.tf
======================================================================

Resources checked: 1
Violations found: 2
Status: ❌ FAILED

Violations by severity:
  HIGH: 2

======================================================================
Detailed Violations:
======================================================================

1. [HIGH] aws_s3_bucket.data
   S3 bucket is public

2. [HIGH] aws_s3_bucket.data
   S3 bucket does not block public ACLs

======================================================================
```

## Adding New Rules

To add new security rules, edit `rules.py`:

```python
def check_your_rule(resource):
    """Check for your condition"""
    if not resource.get('your_property'):
        return 'Your violation message'
    return None

# Add to the appropriate rules list
S3_RULES.append(check_your_rule)
```

## 🧪 Testing

```bash
# Run API tests
python test_api.py

# Run comprehensive tests
python test_comprehensive.py

# Test with pytest
pytest
```

## Dependencies

- **Flask** - Web framework and API server
- **bcrypt** - Password hashing
- **python-hcl2** - HCL2 parser for Terraform
- **requests** - HTTP library
- **python-dotenv** - Environment configuration

See `requirements.txt` for complete list.

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add tests for new features
4. Submit a pull request

## 📝 License

MIT

---

## 🆘 Troubleshooting

### "Invalid API key" error
- Verify the API key is correct
- Check the key hasn't been revoked in Profile page
- Ensure you're using the correct header (`X-API-Key`)

### "Cannot connect to server"
- Verify the server is running: `curl http://localhost:5000/health`
- Check firewall settings
- Confirm the correct server URL

### Exit code 2 in CI/CD
- Check server connectivity
- Verify API key is set correctly
- Review error messages in logs

For more help, see [API_AUTHENTICATION.md](API_AUTHENTICATION.md#troubleshooting)

---

**Made with ❤️ for Infrastructure Security**

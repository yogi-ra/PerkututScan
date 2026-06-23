
# PerkututScan


This project, **PerkututScan**, is an automated Static Application Security Testing (SAST) tool that leverages Nuclei for vulnerability scanning, enhanced with Artificial Intelligence for more insightful analysis. Developed as a lightweight and portable Bash script, it seamlessly integrates into the development pipeline to detect potential security flaws before they reach production.

The system uses Nuclei with custom configurations for static analysis, Regex for pattern matching, and an AI model to provide detailed explanations and risk assessments for identified vulnerabilities.

-----

## Features

  * **AI-Enhanced Reporting**: Each finding is enriched with AI-generated explanations, risk context, mitigation advice, and full-match-string context (showing the exact line of code where the vulnerability was found), helping developers understand the core problem.
  * **Highly Portable**: As a Bash script, it runs on any Unix/Linux-based system without complex dependencies or configurations.
  * **Automated Vulnerability Categorization**: The tool automatically classifies vulnerabilities based on severity (Critical, High, Medium, Low, etc.) to help prioritize fixes.
  * **Git Integration**: Includes a Git pre-push hook that automatically scans your code, preventing you from pushing code with high or critical vulnerabilities.
  * **Security Scan Workflow**: Optionally run additional security tools (Trivy, Bandit, Safety, Pip-Audit, Semgrep) via the `-s/--security-scan` flag for comprehensive vulnerability assessment.

-----

## Required Dependencies

Before you begin, you need to install a few dependencies.

1.  **Nuclei**: The core scanning engine.
2.  **Rsync**: Filter which files should or shouldn't be scanned.
3.  **JQ**: A command-line JSON processor.
4.  **CURL**: A tool to transfer data from or to a server.
5.  **Ollama (Optional)**: If you want run the AI model locally.

**Optional dependencies for security-scan workflow** (enabled with `-s/--security-scan`):
- **Docker**: Required for Trivy and Semgrep scans
- **Bandit**: For Python security issues
- **Safety**: For dependency vulnerability checking
- **Pip-Audit**: For dependency vulnerability checking
- **Semgrep**: For security audit (requires Docker)

## Usage

### Basic Scan

To perform a scan, use the `perkutut-scan` script with the following flags:

  * `-m`: Specifies the scan mode (`dir` for a local directory or `git` for a repository).
  * `-t`: Defines the target directory or repository URL.
  * `-s, --security-scan`: Enable security-scan workflow (runs Trivy, Bandit, Safety, Pip-Audit, Semgrep).
  * `--tools LIST`: Comma-separated list of security tools to run (trivy,bandit,safety,pip-audit,semgrep).
  * `--ai`: Enables the AI analysis feature.
  * `--aih`: Sets a custom URL for the AI host (default: http://localhost:11434/v1).
  * `--aim`: Sets a custom AI model (default: gemma3:latest).
  * `--aib`: Sets how many findings are sent to AI in a batch (default: 5).
  * `--aik`: Sets the API key used to access the AI model (default: ollama).

**Example Command:**

```bash
./perkutut-scan -m dir -t /path/to/your/project --ai
```

After the scan, a detailed report named `sast-report.txt` will be generated in the target's root directory.

### Git Pre-Push Hook Setup

To automatically scan your code every time you push to your repository, you can use the interactive setup script.

1.  Make sure the `perkutut-scan` and `perkutut-scan-init` scripts are in the same directory.
2.  Navigate to your local Git repository's directory.
    ```
    cd /path/to/your/git/project
    ```
4.  Run the initialization script from within your repository's directory:
    ```bash
    /path/to/scripts/perkutut-scan-init
    ```
5.  Follow the interactive prompts to enable AI analysis and confirm the settings.

The script will install a pre-push hook in your repository. Now, every time you run `git push`, the hook will execute the SAST scan. If high or critical vulnerabilities are found, it will prompt you to confirm whether you still want to proceed with the push.

-----

## License

This project is licensed under the **MIT License**, as the core component, Nuclei, is distributed under the same license. See the LICENSE file for more details.

-----

## Contributing

This is an open-source project, and contributions are welcome\! Feel free to open an issue to report bugs or suggest improvements. We appreciate your help in making this tool better.

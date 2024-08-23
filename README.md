# Workflow to Create an AMI Using Packer 

## Description

This repository provides a complete workflow to create an Amazon Machine Image (AMI) using Packer, upload the Packer manifest to an S3 bucket, and configure email notifications for when the manifest is uploaded. The workflow leverages Infrastructure as Code (IaC) principles to automate these processes using GitHub Actions, Packer, and Terraform.

## Repository Structure

1. **GitHub Actions Workflow**: Automates the creation of an AMI, uploads the manifest to S3, and manages notifications.
2. **Packer Configuration**: Defines the Packer template for creating the AMI.
3. **Terraform Configuration**: Sets up the S3 bucket and SNS topic for notifications.
4. **Git Ignore File**: Specifies files and directories to be ignored in version control.

## Workflow Overview

### 1. GitHub Actions Workflow

The GitHub Actions workflow, defined in `.github/workflows/automation.yaml`, automates the following tasks:

- **Packer Build Job**:
  - Checks out the repository.
  - Sets up Packer and initializes, formats, and validates the Packer configuration.
  - Builds the AMI and moves the Packer manifest to an artifact directory.
  - Saves the manifest as an artifact.

- **Terraform Job**:
  - Downloads the Packer manifest artifact.
  - Sets up Terraform and runs initialization, formatting, planning, and applying Terraform configurations.

- **S3 Upload Job**:
  - Downloads the Packer manifest artifact.
  - Configures AWS credentials.
  - Uploads the manifest to the specified S3 bucket.

### 2. Packer Configuration 

The Packer configuration file `image.pkr.hcl` defines the AMI creation process:

- **Source Configuration**: Specifies the source AMI and region.
- **Build Configuration**: Defines the build steps, including package installations and updates.
- **Post-Processor**: Generates a Packer manifest file (`packer-manifest.json`).

### 3. Terraform Configuration

The Terraform configuration file `main.tf` sets up:

- **AWS Provider**: Configures the AWS provider and backend.
- **S3 Bucket and SNS Topic**:
  - Creates an SNS topic for notifications.
  - Defines a policy allowing S3 to publish to the SNS topic.
  - Subscribes an email address to the SNS topic.
  - Configures S3 bucket notifications to trigger the SNS topic on specific events.

### 4. Git Ignore File

The `.gitignore` file ensures that sensitive data and temporary files are not included in version control. It covers:

- **Terraform State Files**: Prevents tracking of `.tfstate` and backup files.
- **Sensitive Files**: Excludes files with sensitive data and secrets.
- **Temporary Files**: Ignores temporary files created by various tools.

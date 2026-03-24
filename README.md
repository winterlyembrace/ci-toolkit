# ci-toolkit

🛠️ CI-Toolkit: High-Performance CI/CD Runtime Images

This repository provides a collection of lightweight and multi-stage Docker images optimized for security (non-root) and speed (Alpine-based) designed as runtimes for CI/CD pipelines (GitLab CI, GitHub Actions, Jenkins).

By using pre-built, specialized images instead of installing dependencies during job execution, we reduce pipeline duration by 40–60% and ensure environment parity across all stages.


🚀 Key Features

    Multi-Stage Builds: Optimized layers to keep images under 100MB where possible.

    Security First: All images run as a non-root user (devops) to minimize the attack surface.

    Hardened Base: Built on Alpine Linux to reduce CVE exposure.

    Tool Ecosystem: Includes not just the core binaries (Terraform/Ansible), but also linting and security scanning tools (TFLint, TFSec, Ansible-lint).


🛡️ Security & Compliance

To ensure production-grade security, this repository implements:

    User Isolation: USER "user" is defined in all Dockerfiles.

    Automated Scanning: Images are scanned weekly for vulnerabilities using Trivy.

    Base Image Pinning: Strict versioning (e.g., python:3.12-alpine3.19) to prevent "latest-tag" drift.


🔄 Pipeline Automation

This project uses a Smart Build approach:

    Targeted Rebuilds: Only modified images are rebuilt using GitLab/GitHub rules:changes.

    Semantic Tagging: Images are tagged with both :latest and the specific :git-sha for rollback capabilities.


🛠️ Usage Examples

GitLab CI/CD
<pre>
deploy-infrastructure:
  stage: deploy
  image: ghcr.io/winterlyembrace/ci-toolkit/tf-runner:1.7.5
  script:
    - terraform init
    - terraform apply -auto-approve
</pre>

GitHub Actions
<pre>
jobs:
  lint:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/winterlyembrace/ci-toolkit/ansible-runner:2.15.9
    steps:
      - uses: actions/checkout@v4
      - run: ansible-lint site.yml
</pre>


📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

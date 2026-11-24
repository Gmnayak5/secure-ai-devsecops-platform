# MASTER PROMPT: 10-Hour Enterprise Jenkins & GitHub Curriculum (Session-by-Session Generator)

## Role & Persona
Act as a Principal DevOps Architect and Technical Instructor with 15+ years of enterprise CI/CD experience. You are designing and delivering a 10-hour hands-on **Jenkins + GitHub** training module for freshers and aspiring DevOps engineers. Your tone is clear, encouraging, practical, and industry-grounded.

## Curriculum Scope (10 Hours Total = 5 Sessions x 2 Hours Each)
Map the complete syllabus across five 2-hour sessions as follows:

* **Session 1 (Hours 1–2): CI Fundamentals, Jenkins Architecture & Setup**
  * Overview of Continuous Integration (CI) & Traditional vs. Continuous Integration
  * Jenkins Controller and Agent topology (modern terminology replacing master-slave)
  * Jenkins installation, initial setup, tool configuration (JDK, Maven, Git), and plugin management
  * *Lab:* Installing Jenkins, configuring global tools/plugins, and connecting a build agent

* **Session 2 (Hours 3–4): Modern Pipeline Configuration & Git Integration**
  * Freestyle jobs (brief historical comparison & why enterprises moved away)
  * Pipeline-as-Code (Declarative Pipelines): syntax, `agent`, `stages`, `steps`, `post` actions, `environment` variables
  * Version control integration: sourcing `Jenkinsfile` directly from GitHub repositories
  * Multibranch Pipelines: managing CI/CD across `feature/*`, `staging`, and `main`/`production` branches
  * *Lab:* Creating a Declarative Pipeline from GitHub & configuring a Multibranch Pipeline

* **Session 3 (Hours 5–6): Enterprise Agents, Secrets Management & Build Triggers**
  * Secrets and credentials management: `withCredentials`, secure credential binding, SSH keys, GitHub PATs/tokens, preventing console log exposure
  * Ephemeral build agents: provisioning on-demand build agents using Docker containers to isolate build environments
  * Build triggers & notifications: GitHub webhooks (push/PR events), Email notifications, and ChatOps alerts (Slack / Microsoft Teams)
  * *Lab:* Provisioning dynamic Docker build agents, configuring GitHub Webhooks, and sending Slack pipeline alerts

* **Session 4 (Hours 7–8): DevSecOps Quality Gates & Artifact Registry Integration**
  * Quality gates & static code analysis: integrating SonarQube (`withSonarQubeEnv`, `waitForQualityGate`)
  * Container image building & vulnerability scanning: scanning images with Trivy and failing builds on CRITICAL/HIGH CVEs
  * Container registry integration: authenticating and pushing verified artifacts to Docker Hub and Amazon ECR
  * *Lab:* Building a DevSecOps pipeline with Maven build, SonarQube Quality Gate, Docker build, Trivy scan, and ECR/Docker Hub push

* **Session 5 (Hours 9–10): Enterprise Architecture (Shared Libraries) & End-to-End Capstone**
  * Enterprise pipeline architecture: Jenkins Shared Libraries (`vars/`, `src/`, `resources/`) for DRY code reuse across microservices
  * End-to-End Production CI Capstone: GitHub Webhook trigger -> Maven build -> SonarQube quality gate -> Docker build -> Trivy scan -> Push to Amazon ECR -> Slack status alert
  * *Lab:* Writing a reusable Shared Library function and executing the full end-to-end enterprise pipeline

---

## Strict Output & Formatting Rules (Per Session)
Generate **ONLY ONE 2-hour session per response** so each session is comprehensive, deep, and ready to copy-paste directly into a GitHub `README.md` or session handbook.

Every 2-hour session output MUST follow this exact structure in clean **GitHub-Flavored Markdown**:

1. **Session Header & Time-Boxed Agenda (120 Minutes)**
   * Break down the 2 hours into clear teaching + lab blocks (e.g., 00–20 min, 20–50 min, etc.).
2. **Real-World Analogies & Neat ASCII Architecture Diagrams**
   * Explain core concepts using everyday analogies that freshers immediately grasp.
   * Include clean, well-aligned ASCII flow/architecture diagrams.
3. **Deep-Dive Concepts with Production Examples**
   * Provide complete, copy-pasteable, syntax-highlighted code snippets (`groovy`, `bash`, `dockerfile`, `yaml`) with line-by-line explanations.
4. **Industry Scenarios, Common Production Failures & DevOps Fixes**
   * Include a dedicated section detailing:
     * *How it is used in real enterprise teams*
     * *3–4 real-world production issues/errors freshers will encounter* (e.g., webhook payload failures, leaked secrets in logs, zombie Docker containers, quality gate timeouts)
     * *Step-by-step DevOps root-cause fixes and best practices*
5. **Guided Hands-On Lab (Classroom Practical)**
   * Step-by-step terminal and UI instructions matching the session's practical deliverables.
6. **Take-Home Mini-Project (Portfolio / LinkedIn Ready)**
   * A realistic mini-project with clear objectives, folder structure, acceptance criteria, and deliverables students can push to GitHub.
7. **Knowledge Check: 10 Multiple-Choice Questions (MCQs)**
   * Scenario-based and conceptual MCQs with an expandable `<details><summary>Click to view Answer Key & Explanations</summary>` block at the bottom.
8. **Deep-Dive Research & Homework Questions (10 Investigative Prompts)**
   * Open-ended questions requiring students to explore Jenkins documentation, inspect logs, test edge cases at home, and prepare for the next session.

---

## Execution Instruction
Confirm you understand the curriculum and immediately generate **Session 1 (Hours 1–2)** in full detail. Wait for my prompt before generating Session 2.

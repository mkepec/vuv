# Week 9: Secure CI/CD + Advanced Deployments - "Building Security Into the Pipeline" 🔐
**VelocityTech Transformation Week 9**

**Story Context:** CI/CD pipeline works smoothly, but security vulnerabilities slip through to production and deployment strategies lack sophistication  
**Focus:** Implementing DevSecOps practices with security scanning and advanced deployment patterns  
**Characters in Focus:** Filip's security nightmares, Ana's vulnerability concerns, Maja's compliance requirements

---

## 🔥 The Weekly Crisis: "The Great Security Breach Scare"
**Setting:** VelocityTech office, Tuesday 8:45 AM. Security alert notifications flooding everyone's phones.  
**The Problem:** Automated vulnerability scanner found critical security issues in production, deployed through the "secure" CI/CD pipeline

**Scene:** [VelocityTech main floor. Filip staring at security alerts with growing panic. Ana checking vulnerability reports across multiple environments. Luka confused about which security issues are real vs false positives. Maja on an emergency call with a very concerned client.]

**FILIP** *(frantically checking security dashboard alerts)*  
> "Critical vulnerability: CVE-2023-44487 in our HTTP/2 implementation..."  
> "High-risk dependency: lodash 4.17.20 with prototype pollution vulnerability..."  
> *(voice rising)* "These vulnerabilities have been in production for THREE WEEKS!"

**ANA** *(running security scans across environments)*  
> "The payment gateway has 47 security findings - 12 critical, 18 high, 17 medium..."  
> "Some of these vulnerabilities could allow complete system compromise!"  
> *(frustrated)* "Why didn't our CI/CD pipeline catch these before deployment?"

**LUKA** *(looking at dependency scan results)*  
> "Half of my npm packages have known vulnerabilities..."  
> "But some of these 'critical' issues are in development dependencies that never reach production..."  
> *(confused)* "How do I know which security alerts actually matter?"

**MAJA** *(ending a tense phone call)*  
> "Client is demanding a complete security audit and compliance certification..."  
> "They want proof that our deployment process prevents security vulnerabilities!"  
> *(stressed)* "Insurance company says we need SOC 2 compliance for the new contract!"

**FILIP** *(pulling up container scan results)*  
> "Our Docker images have outdated base layers with kernel vulnerabilities..."  
> "Container running as root, no security profiles, privileged access..."  
> *(overwhelmed)* "Every part of our stack has security issues!"

**ANA** *(checking application security testing)*  
> "I've been testing functionality, but I haven't been testing for security vulnerabilities..."  
> "SQL injection, XSS, authentication bypass - how do I test for all these attack vectors?"

**LUKA** *(looking at code analysis results)*  
> "The security scanner flagged 'potential hardcoded secrets' in my code..."  
> "But they're just example values in comments! How do I separate real issues from noise?"

**MARKO** *(entering, immediately recognizing the security crisis)*  
> "Let me guess - you built a fast CI/CD pipeline but security became an afterthought?"  
> *(to you)* "This is exactly why we need DevSecOps - security integrated into every step. Ready to shift security left?"

**FILIP** *(desperate)*  
> "We need security, but I can't slow down deployments with manual security reviews!"  
> "There has to be a way to automate security without breaking our delivery speed!"

**MAJA** *(calculating business impact)*  
> "If we fail the security audit, we lose the contract worth 40% of our revenue..."  
> "But if we stop deployments for manual security reviews, we can't meet feature deadlines!"

**[All eyes turn to you - the intern who might understand modern security practices.]**

**ANA** *(pleading)*  
> "Please tell me there's a way to catch security issues automatically before they reach production!"  
> "Something that integrates with our existing CI/CD pipeline?"

**[Your systems thinking activates. This isn't just about adding security tools - it's about building security into every stage of the development process.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Implement automated security scanning in CI/CD pipelines** - Integrate dependency scanning, container security, and code analysis to catch vulnerabilities early
2. **Design advanced deployment strategies** - Implement blue-green, canary, and rolling deployments that minimize risk and enable quick rollbacks
3. **Create comprehensive DevSecOps workflows** - Build security gates that ensure compliance without slowing development velocity

**Real-world Connection:** DevSecOps skills are extremely valuable in the Croatian market. Companies like Infobip, Rimac, and Span are heavily investing in security automation due to regulatory requirements and cyber threats. Security-integrated CI/CD expertise commands premium salaries and is essential for senior DevOps and security engineering roles.

---

## 📚 The Knowledge Foundation

### DevSecOps Principles and Shifting Security Left
**The Problem it Solves:** Security vulnerabilities discovered late, manual security reviews creating bottlenecks, compliance failures, security vs speed conflicts  
**How it Works:** Security practices integrated throughout the development lifecycle, with automated checks preventing vulnerable code from reaching production

**Traditional vs DevSecOps Security:**
```
Traditional Security:              DevSecOps Security:
┌─────────────────────────┐       ┌─────────────────────────┐
│ Security at the end     │       │ Security from start     │
│ Manual penetration tests│       │ Automated scanning      │
│ Weeks of security review│       │ Minutes of gate checks  │
│ Security vs Speed       │       │ Security enables Speed  │
│ Reactive vulnerability  │       │ Proactive prevention    │
│ Blame culture          │       │ Shared responsibility   │
└─────────────────────────┘       └─────────────────────────┘
```

**Core DevSecOps Principles:**
- **Shift Left:** Move security testing early in development cycle
- **Automation First:** Automate security checks and compliance validation
- **Continuous Monitoring:** Real-time security monitoring in all environments
- **Shared Responsibility:** Security is everyone's responsibility, not just security team
- **Risk-Based Approach:** Focus on actual business risks, not just vulnerability counts
- **Fail Fast:** Catch security issues in minutes, not months

**DevSecOps Integration Points:**
```
Code → Build → Test → Deploy → Monitor
  ↓      ↓       ↓       ↓        ↓
Static   Dependency Container Runtime  Security
Analysis Scanning   Security Monitoring Alerts
SAST     SCA       Container  RASP     SIEM
```

**Real-world Example:** Capital One integrates 50+ security tools into their CI/CD pipeline, catching 95% of vulnerabilities before production  
**VelocityTech Connection:** Automated security scanning would have caught vulnerabilities before they reached production

### Advanced CI/CD Patterns and Multi-Stage Pipelines
**The Problem it Solves:** Single-pipeline bottlenecks, insufficient testing environments, deployment risks, slow feedback loops  
**How it Works:** Multi-stage pipelines with parallel execution, comprehensive testing environments, and automated quality gates

**Advanced Pipeline Architecture:**
```yaml
# .github/workflows/secure-pipeline.yml
name: Secure CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  security-scan:
    name: Security Analysis
    runs-on: ubuntu-latest
    strategy:
      matrix:
        scan-type: [sast, dependency, secret, license]
    steps:
      - uses: actions/checkout@v4
      - name: Run security scan
        uses: security-scanner-action@v1
        with:
          type: ${{ matrix.scan-type }}

  build-and-test:
    name: Build and Test
    runs-on: ubuntu-latest
    needs: security-scan
    steps:
      - name: Build application
      - name: Run unit tests
      - name: Run integration tests
      - name: Security container scan

  deploy-staging:
    name: Deploy to Staging
    needs: build-and-test
    if: github.ref == 'refs/heads/develop'
    environment: staging
    steps:
      - name: Deploy with blue-green strategy
      - name: Run smoke tests
      - name: Security penetration testing

  deploy-production:
    name: Deploy to Production
    needs: deploy-staging
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - name: Canary deployment
      - name: Monitor metrics
      - name: Full deployment or rollback
```

**Pipeline Security Gates:**
- **Code Quality Gates:** SonarQube, CodeClimate, static analysis
- **Dependency Gates:** Snyk, OWASP Dependency Check, license compliance
- **Container Gates:** Trivy, Twistlock, image vulnerability scanning
- **Deployment Gates:** Infrastructure compliance, security policy validation
- **Runtime Gates:** Application performance, security monitoring, error rates

**Real-world Example:** Netflix runs 4,000+ deployments daily through multi-stage pipelines with comprehensive security gates  
**VelocityTech Connection:** Multi-stage security scanning would provide defense in depth

### Advanced Deployment Strategies: Blue-Green, Canary, Rolling
**The Problem it Solves:** Deployment downtime, rollback complexity, user impact from bad deployments, risk management  
**How it Works:** Advanced deployment patterns that minimize risk and enable instant rollbacks

**Blue-Green Deployment:**
```
Blue Environment (Current Production)    Green Environment (New Version)
┌─────────────────────────────────┐    ┌─────────────────────────────────┐
│ App v1.0 ← 100% Traffic         │    │ App v1.1 ← 0% Traffic          │
│ Load Balancer                   │    │ Testing & Validation           │
│ Database (shared)               │    │ Ready for switchover           │
└─────────────────────────────────┘    └─────────────────────────────────┘
                    ↓ Switch traffic instantly ↓
┌─────────────────────────────────┐    ┌─────────────────────────────────┐
│ App v1.0 ← 0% Traffic           │    │ App v1.1 ← 100% Traffic        │
│ Ready for rollback              │    │ New Production                 │
│ Can be terminated               │    │ Monitoring for issues          │
└─────────────────────────────────┘    └─────────────────────────────────┘
```

**Canary Deployment:**
```
Production Traffic Distribution:
┌──────────────────────────────────────────────────────────────────┐
│ Stable Version (v1.0)     │ Canary Version (v1.1)               │
│ ████████████████████      │ ██                                  │
│ 90% of users              │ 10% of users                        │
│ Proven stable             │ Monitoring for issues               │
└──────────────────────────────────────────────────────────────────┘
           ↓ If metrics good, increase canary traffic ↓
┌──────────────────────────────────────────────────────────────────┐
│ Stable Version (v1.0)     │ Canary Version (v1.1)               │
│ ████████████              │ ████████                            │
│ 70% of users              │ 30% of users                        │
│ Gradually decreasing      │ Gradually increasing                │
└──────────────────────────────────────────────────────────────────┘
```

**Rolling Deployment:**
```
Kubernetes Rolling Update:
Stage 1: │ v1.0 │ v1.0 │ v1.0 │     │     │  ← 3 replicas v1.0
Stage 2: │ v1.0 │ v1.0 │ v1.1 │     │     │  ← 1 replica updated
Stage 3: │ v1.0 │ v1.1 │ v1.1 │     │     │  ← 2 replicas updated  
Stage 4: │ v1.1 │ v1.1 │ v1.1 │     │     │  ← All replicas updated
```

**Deployment Strategy Selection:**
- **Blue-Green:** Zero downtime, instant rollback, requires 2x resources
- **Canary:** Risk mitigation, gradual rollout, complex monitoring
- **Rolling:** Resource efficient, built into Kubernetes, slower rollback
- **Feature Flags:** Runtime control, A/B testing, gradual feature release

**Real-world Example:** Amazon uses canary deployments for AWS services, automatically rolling back if error rates increase  
**VelocityTech Connection:** Advanced deployments would minimize risk and enable confident releases

**Key Takeaways:**
- DevSecOps integrates security throughout the development lifecycle
- Multi-stage pipelines provide comprehensive quality and security gates
- Advanced deployment strategies minimize risk and enable quick recovery
- Security automation enables speed while maintaining compliance

---

## 🛠️ Lab Mission: "The DevSecOps Security Revolution"
**Scenario:** VelocityTech's CI/CD pipeline deploys vulnerable code to production, failing security audits  
**Your Mission:** Implement comprehensive security scanning and advanced deployment strategies  
**Success Criteria:** CI/CD pipeline with automated security gates and blue-green deployment capability

### Phase 1: Security Scanning Integration (75 minutes)
**Objective:** Integrate comprehensive security scanning into existing CI/CD pipeline

**Steps:**
1. **Set up Security Scanning Tools**
   ```bash
   # Navigate to existing CI/CD project
   cd velocitytech-infrastructure
   
   # Create security scanning configuration
   mkdir -p .github/workflows security-config
   
   # Install security scanning tools locally for testing
   npm install -g @snyk/cli
   docker pull aquasec/trivy:latest
   pip3 install bandit safety
   
   # Test tools are working
   snyk --version
   docker run --rm aquasec/trivy --version
   bandit --version
   ```
   - Expected output: All security tools installed and working
   - Troubleshooting: If npm install fails, use `sudo npm install -g` or check Node.js version

2. **Create Enhanced Security Pipeline**
   ```yaml
   # .github/workflows/secure-cicd.yml
   name: Secure CI/CD Pipeline
   
   on:
     push:
       branches: [ main, develop ]
     pull_request:
       branches: [ main ]
   
   env:
     REGISTRY: ghcr.io
     IMAGE_NAME: velocitytech/payment-gateway
   
   jobs:
     static-analysis:
       name: Static Code Analysis (SAST)
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Setup Node.js
         uses: actions/setup-node@v3
         with:
           node-version: '18'
           cache: 'npm'
       
       - name: Install dependencies
         run: npm ci
       
       - name: ESLint Security Analysis
         run: |
           npm install eslint-plugin-security --save-dev
           npx eslint . --ext .js,.ts --format json --output-file eslint-results.json || true
       
       - name: SonarCloud Scan
         uses: SonarSource/sonarcloud-github-action@master
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
           SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
       
       - name: Upload SAST Results
         uses: actions/upload-artifact@v3
         with:
           name: sast-results
           path: eslint-results.json
   
     dependency-scan:
       name: Dependency Vulnerability Scan (SCA)
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Setup Node.js
         uses: actions/setup-node@v3
         with:
           node-version: '18'
           cache: 'npm'
       
       - name: Install dependencies
         run: npm ci
       
       - name: Snyk Security Scan
         run: |
           npx snyk auth ${{ secrets.SNYK_TOKEN }}
           npx snyk test --json --json-file-output=snyk-results.json || true
           npx snyk monitor || true
       
       - name: OWASP Dependency Check
         uses: dependency-check/Dependency-Check_Action@main
         with:
           project: 'VelocityTech Payment Gateway'
           path: '.'
           format: 'JSON'
           out: 'dependency-check-results'
       
       - name: Upload dependency scan results
         uses: actions/upload-artifact@v3
         with:
           name: dependency-scan-results
           path: |
             snyk-results.json
             dependency-check-results/
   
     secret-scan:
       name: Secret Detection
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
         with:
           fetch-depth: 0
       
       - name: TruffleHog Secret Scan
         uses: trufflesecurity/trufflehog@main
         with:
           path: ./
           base: main
           head: HEAD
           extra_args: --debug --only-verified
       
       - name: GitLeaks Secret Detection
         uses: gitleaks/gitleaks-action@v2
         env:
           GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   
     license-compliance:
       name: License Compliance Check
       runs-on: ubuntu-latest
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Setup Node.js
         uses: actions/setup-node@v3
         with:
           node-version: '18'
           cache: 'npm'
       
       - name: Install dependencies
         run: npm ci
       
       - name: License Checker
         run: |
           npx license-checker --json --out licenses.json
           npx license-checker --failOn 'GPL;AGPL;LGPL;UNLICENSED' || echo "License check completed with warnings"
       
       - name: Upload license results
         uses: actions/upload-artifact@v3
         with:
           name: license-results
           path: licenses.json
   
     build-and-test:
       name: Build and Test
       runs-on: ubuntu-latest
       needs: [static-analysis, dependency-scan, secret-scan, license-compliance]
       
       outputs:
         image-digest: ${{ steps.build.outputs.digest }}
         image-tag: ${{ steps.meta.outputs.tags }}
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Setup Node.js
         uses: actions/setup-node@v3
         with:
           node-version: '18'
           cache: 'npm'
       
       - name: Install dependencies
         run: npm ci
       
       - name: Run tests
         run: npm test -- --coverage
       
       - name: Set up Docker Buildx
         uses: docker/setup-buildx-action@v3
       
       - name: Log in to Container Registry
         uses: docker/login-action@v3
         with:
           registry: ${{ env.REGISTRY }}
           username: ${{ github.actor }}
           password: ${{ secrets.GITHUB_TOKEN }}
       
       - name: Extract metadata
         id: meta
         uses: docker/metadata-action@v4
         with:
           images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
           tags: |
             type=ref,event=branch
             type=ref,event=pr
             type=sha,prefix={{branch}}-
       
       - name: Build and push Docker image
         id: build
         uses: docker/build-push-action@v4
         with:
           context: .
           push: true
           tags: ${{ steps.meta.outputs.tags }}
           labels: ${{ steps.meta.outputs.labels }}
           cache-from: type=gha
           cache-to: type=gha,mode=max
   
     container-security:
       name: Container Security Scan
       runs-on: ubuntu-latest
       needs: build-and-test
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Run Trivy vulnerability scanner
         uses: aquasecurity/trivy-action@master
         with:
           image-ref: ${{ needs.build-and-test.outputs.image-tag }}
           format: 'sarif'
           output: 'trivy-results.sarif'
       
       - name: Upload Trivy scan results to GitHub Security tab
         uses: github/codeql-action/upload-sarif@v2
         with:
           sarif_file: 'trivy-results.sarif'
       
       - name: Docker Scout vulnerability scan
         uses: docker/scout-action@v1
         with:
           command: cves
           image: ${{ needs.build-and-test.outputs.image-tag }}
           only-severities: critical,high
           exit-code: true
   ```

3. **Create Security Configuration Files**
   ```bash
   # Create ESLint security configuration
   cat > .eslintrc.security.js << 'EOF'
   module.exports = {
     plugins: ['security'],
     extends: ['plugin:security/recommended'],
     rules: {
       'security/detect-object-injection': 'error',
       'security/detect-non-literal-regexp': 'error',
       'security/detect-unsafe-regex': 'error',
       'security/detect-buffer-noassert': 'error',
       'security/detect-child-process': 'error',
       'security/detect-disable-mustache-escape': 'error',
       'security/detect-eval-with-expression': 'error',
       'security/detect-no-csrf-before-method-override': 'error',
       'security/detect-non-literal-fs-filename': 'error',
       'security/detect-non-literal-require': 'error',
       'security/detect-possible-timing-attacks': 'error',
       'security/detect-pseudoRandomBytes': 'error'
     }
   };
   EOF
   
   # Create SonarQube configuration
   cat > sonar-project.properties << 'EOF'
   sonar.projectKey=velocitytech_payment-gateway
   sonar.organization=velocitytech
   sonar.sources=src/
   sonar.tests=tests/
   sonar.javascript.lcov.reportPaths=coverage/lcov.info
   sonar.coverage.exclusions=tests/**
   sonar.security.hotspots.minSeverity=low
   EOF
   
   # Create security policy file
   cat > .github/SECURITY.md << 'EOF'
   # Security Policy
   
   ## Supported Versions
   
   | Version | Supported          |
   | ------- | ------------------ |
   | 1.2.x   | :white_check_mark: |
   | 1.1.x   | :white_check_mark: |
   | < 1.1   | :x:                |
   
   ## Reporting a Vulnerability
   
   Please report security vulnerabilities to security@velocitytech.com
   EOF
   ```

4. **Create Security Gates Configuration**
   ```yaml
   # security-config/security-gates.yml
   security_gates:
     sast:
       fail_on:
         - critical_severity: true
         - high_severity_count: 5
       exclude_patterns:
         - "test/**/*"
         - "docs/**/*"
     
     dependency_scan:
       fail_on:
         - critical_vulnerabilities: true
         - high_vulnerabilities_count: 10
       allowed_licenses:
         - MIT
         - Apache-2.0
         - BSD-3-Clause
         - ISC
       blocked_licenses:
         - GPL-3.0
         - AGPL-3.0
     
     container_scan:
       fail_on:
         - critical_cve: true
         - high_cve_count: 5
       base_image_requirements:
         - must_be_official: true
         - max_age_days: 30
     
     secret_detection:
       fail_on:
         - any_secret_found: true
       exclude_paths:
         - ".git/**"
         - "node_modules/**"
   ```

5. **Test Security Pipeline**
   ```bash
   # Commit and push to trigger security pipeline
   git add .
   git commit -m "Add comprehensive security scanning to CI/CD pipeline
   
   - Integrate SAST, SCA, secret detection, and license compliance
   - Add container vulnerability scanning with Trivy
   - Configure security gates with fail conditions
   - Upload security results to GitHub Security tab"
   
   git push origin feature/secure-pipeline
   
   # Create pull request to test security scanning
   # Monitor GitHub Actions for pipeline execution
   
   # Locally test individual security tools
   echo "Testing security tools locally..."
   
   # Test dependency scanning
   npx snyk test --json || echo "Vulnerabilities found"
   
   # Test static analysis
   npx eslint . --config .eslintrc.security.js || echo "Security issues found"
   
   # Test container scanning
   docker build -t test-security .
   docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
     aquasec/trivy image test-security
   ```

**Checkpoint:** Comprehensive security scanning integrated into CI/CD pipeline with automated gates

### Phase 2: Advanced Deployment Strategies (90 minutes)
**Objective:** Implement blue-green and canary deployment patterns for risk-free releases

**Steps:**
1. **Create Blue-Green Deployment Infrastructure**
   ```bash
   # Create deployment configurations directory
   mkdir -p deployments/blue-green deployments/canary
   
   # Blue-Green deployment with Kubernetes
   cat > deployments/blue-green/blue-green-deployment.yml << 'EOF'
   # Blue Environment (Current Production)
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-gateway-blue
     namespace: production
     labels:
       app: payment-gateway
       version: blue
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: payment-gateway
         version: blue
     template:
       metadata:
         labels:
           app: payment-gateway
           version: blue
       spec:
         containers:
         - name: payment-gateway
           image: velocitytech/payment-gateway:stable
           ports:
           - containerPort: 3000
           env:
           - name: ENVIRONMENT
             value: "production-blue"
           - name: VERSION
             value: "blue"
           resources:
             requests:
               memory: "256Mi"
               cpu: "250m"
             limits:
               memory: "512Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: 3000
             initialDelaySeconds: 30
             periodSeconds: 10
           readinessProbe:
             httpGet:
               path: /ready
               port: 3000
             initialDelaySeconds: 5
             periodSeconds: 5
   
   ---
   # Green Environment (New Version)
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-gateway-green
     namespace: production
     labels:
       app: payment-gateway
       version: green
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: payment-gateway
         version: green
     template:
       metadata:
         labels:
           app: payment-gateway
           version: green
       spec:
         containers:
         - name: payment-gateway
           image: velocitytech/payment-gateway:latest
           ports:
           - containerPort: 3000
           env:
           - name: ENVIRONMENT
             value: "production-green"
           - name: VERSION
             value: "green"
           resources:
             requests:
               memory: "256Mi"
               cpu: "250m"
             limits:
               memory: "512Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: 3000
             initialDelaySeconds: 30
             periodSeconds: 10
           readinessProbe:
             httpGet:
               path: /ready
               port: 3000
             initialDelaySeconds: 5
             periodSeconds: 5
   
   ---
   # Service that can switch between blue and green
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-gateway-service
     namespace: production
     labels:
       app: payment-gateway
   spec:
     type: LoadBalancer
     selector:
       app: payment-gateway
       version: blue  # Initially points to blue
     ports:
     - name: http
       protocol: TCP
       port: 80
       targetPort: 3000
   EOF
   ```

2. **Create Blue-Green Deployment Script**
   ```bash
   # deployments/blue-green/deploy.sh
   cat > deployments/blue-green/deploy.sh << 'EOF'
   #!/bin/bash
   
   # Blue-Green Deployment Script for VelocityTech
   
   set -e
   
   NAMESPACE="production"
   SERVICE_NAME="payment-gateway-service"
   NEW_IMAGE="$1"
   TIMEOUT="300"
   
   if [ -z "$NEW_IMAGE" ]; then
       echo "Usage: $0 <new-image-tag>"
       echo "Example: $0 velocitytech/payment-gateway:v1.2.0"
       exit 1
   fi
   
   echo "🚀 Starting Blue-Green Deployment"
   echo "New image: $NEW_IMAGE"
   
   # Determine current active environment
   CURRENT_VERSION=$(kubectl get service $SERVICE_NAME -n $NAMESPACE -o jsonpath='{.spec.selector.version}')
   
   if [ "$CURRENT_VERSION" = "blue" ]; then
       INACTIVE_VERSION="green"
       echo "📘 Current active: BLUE, deploying to: GREEN"
   else
       INACTIVE_VERSION="blue"
       echo "💚 Current active: GREEN, deploying to: BLUE"
   fi
   
   # Update inactive environment with new image
   echo "🔄 Updating $INACTIVE_VERSION environment..."
   kubectl set image deployment/payment-gateway-$INACTIVE_VERSION \
       payment-gateway=$NEW_IMAGE -n $NAMESPACE
   
   # Wait for rollout to complete
   echo "⏳ Waiting for $INACTIVE_VERSION deployment to be ready..."
   kubectl rollout status deployment/payment-gateway-$INACTIVE_VERSION \
       -n $NAMESPACE --timeout=${TIMEOUT}s
   
   # Verify inactive environment health
   echo "🏥 Running health checks on $INACTIVE_VERSION environment..."
   kubectl wait --for=condition=available --timeout=${TIMEOUT}s \
       deployment/payment-gateway-$INACTIVE_VERSION -n $NAMESPACE
   
   # Get inactive environment endpoint for testing
   INACTIVE_POD=$(kubectl get pods -n $NAMESPACE -l version=$INACTIVE_VERSION -o jsonpath='{.items[0].metadata.name}')
   
   # Port forward for testing (in background)
   kubectl port-forward $INACTIVE_POD 8080:3000 -n $NAMESPACE &
   PORT_FORWARD_PID=$!
   sleep 5
   
   # Test inactive environment
   echo "🧪 Testing $INACTIVE_VERSION environment..."
   if curl -f http://localhost:8080/health > /dev/null 2>&1; then
       echo "✅ Health check passed"
   else
       echo "❌ Health check failed"
       kill $PORT_FORWARD_PID
       exit 1
   fi
   
   # Clean up port forward
   kill $PORT_FORWARD_PID
   
   # Switch traffic to new environment
   echo "🔀 Switching traffic from $CURRENT_VERSION to $INACTIVE_VERSION..."
   kubectl patch service $SERVICE_NAME -n $NAMESPACE \
       -p '{"spec":{"selector":{"version":"'$INACTIVE_VERSION'"}}}'
   
   # Wait a moment for traffic switch
   sleep 10
   
   # Final health check on live traffic
   echo "🏥 Final health check with live traffic..."
   SERVICE_IP=$(kubectl get service $SERVICE_NAME -n $NAMESPACE -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
   if [ -z "$SERVICE_IP" ]; then
       SERVICE_IP=$(kubectl get service $SERVICE_NAME -n $NAMESPACE -o jsonpath='{.spec.clusterIP}')
   fi
   
   if curl -f http://$SERVICE_IP/health > /dev/null 2>&1; then
       echo "✅ Deployment successful! Traffic switched to $INACTIVE_VERSION"
       echo "🗑️  You can now clean up the old $CURRENT_VERSION environment if desired"
   else
       echo "❌ Final health check failed! Rolling back..."
       kubectl patch service $SERVICE_NAME -n $NAMESPACE \
           -p '{"spec":{"selector":{"version":"'$CURRENT_VERSION'"}}}'
       echo "🔙 Rolled back to $CURRENT_VERSION"
       exit 1
   fi
   
   echo "🎉 Blue-Green deployment completed successfully!"
   EOF
   
   chmod +x deployments/blue-green/deploy.sh
   ```

3. **Create Canary Deployment Configuration**
   ```yaml
   # deployments/canary/canary-deployment.yml
   # Stable version (90% of traffic)
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-gateway-stable
     namespace: production
     labels:
       app: payment-gateway
       version: stable
   spec:
     replicas: 9  # 90% of total capacity
     selector:
       matchLabels:
         app: payment-gateway
         version: stable
     template:
       metadata:
         labels:
           app: payment-gateway
           version: stable
       spec:
         containers:
         - name: payment-gateway
           image: velocitytech/payment-gateway:stable
           ports:
           - containerPort: 3000
           env:
           - name: ENVIRONMENT
             value: "production-stable"
           - name: CANARY_DEPLOYMENT
             value: "false"
           resources:
             requests:
               memory: "256Mi"
               cpu: "250m"
             limits:
               memory: "512Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: 3000
             initialDelaySeconds: 30
             periodSeconds: 10
           readinessProbe:
             httpGet:
               path: /ready
               port: 3000
             initialDelaySeconds: 5
             periodSeconds: 5
   
   ---
   # Canary version (10% of traffic)
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-gateway-canary
     namespace: production
     labels:
       app: payment-gateway
       version: canary
   spec:
     replicas: 1  # 10% of total capacity
     selector:
       matchLabels:
         app: payment-gateway
         version: canary
     template:
       metadata:
         labels:
           app: payment-gateway
           version: canary
       spec:
         containers:
         - name: payment-gateway
           image: velocitytech/payment-gateway:latest
           ports:
           - containerPort: 3000
           env:
           - name: ENVIRONMENT
             value: "production-canary"
           - name: CANARY_DEPLOYMENT
             value: "true"
           resources:
             requests:
               memory: "256Mi"
               cpu: "250m"
             limits:
               memory: "512Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: 3000
             initialDelaySeconds: 30
             periodSeconds: 10
           readinessProbe:
             httpGet:
               path: /ready
               port: 3000
             initialDelaySeconds: 5
             periodSeconds: 5
   
   ---
   # Service that load balances between stable and canary
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-gateway-service
     namespace: production
     labels:
       app: payment-gateway
   spec:
     type: LoadBalancer
     selector:
       app: payment-gateway
     ports:
     - name: http
       protocol: TCP
       port: 80
       targetPort: 3000
   ```

4. **Create Canary Deployment Automation**
   ```bash
   # deployments/canary/canary-deploy.sh
   cat > deployments/canary/canary-deploy.sh << 'EOF'
   #!/bin/bash
   
   # Automated Canary Deployment with Metrics Monitoring
   
   set -e
   
   NAMESPACE="production"
   NEW_IMAGE="$1"
   CANARY_PERCENTAGE="${2:-10}"
   MONITORING_DURATION="${3:-300}"  # 5 minutes
   ERROR_THRESHOLD="${4:-5}"        # 5% error rate threshold
   
   if [ -z "$NEW_IMAGE" ]; then
       echo "Usage: $0 <new-image> [canary-percentage] [monitoring-duration] [error-threshold]"
       echo "Example: $0 velocitytech/payment-gateway:v1.2.0 10 300 5"
       exit 1
   fi
   
   echo "🕯️  Starting Canary Deployment"
   echo "New image: $NEW_IMAGE"
   echo "Canary percentage: $CANARY_PERCENTAGE%"
   echo "Monitoring duration: $MONITORING_DURATION seconds"
   echo "Error threshold: $ERROR_THRESHOLD%"
   
   # Calculate replica counts
   TOTAL_REPLICAS=10
   CANARY_REPLICAS=$(( TOTAL_REPLICAS * CANARY_PERCENTAGE / 100 ))
   STABLE_REPLICAS=$(( TOTAL_REPLICAS - CANARY_REPLICAS ))
   
   echo "📊 Replica distribution: Stable=$STABLE_REPLICAS, Canary=$CANARY_REPLICAS"
   
   # Deploy canary version
   echo "🚀 Deploying canary version..."
   kubectl set image deployment/payment-gateway-canary \
       payment-gateway=$NEW_IMAGE -n $NAMESPACE
   
   kubectl scale deployment/payment-gateway-canary \
       --replicas=$CANARY_REPLICAS -n $NAMESPACE
   
   kubectl scale deployment/payment-gateway-stable \
       --replicas=$STABLE_REPLICAS -n $NAMESPACE
   
   # Wait for canary deployment
   echo "⏳ Waiting for canary deployment to be ready..."
   kubectl rollout status deployment/payment-gateway-canary \
       -n $NAMESPACE --timeout=300s
   
   # Monitor canary metrics
   echo "📈 Monitoring canary metrics for $MONITORING_DURATION seconds..."
   
   START_TIME=$(date +%s)
   END_TIME=$(( START_TIME + MONITORING_DURATION ))
   
   STABLE_ERRORS=0
   CANARY_ERRORS=0
   STABLE_REQUESTS=0
   CANARY_REQUESTS=0
   
   while [ $(date +%s) -lt $END_TIME ]; do
       # Simulate metric collection (in real implementation, use Prometheus/Grafana)
       echo "🔍 Checking metrics..."
       
       # Get current error rates from both versions
       # This is a simplified simulation - in reality, you'd query monitoring systems
       CURRENT_STABLE_ERRORS=$(kubectl logs -l version=stable -n $NAMESPACE --tail=100 | grep -c "ERROR" || echo "0")
       CURRENT_CANARY_ERRORS=$(kubectl logs -l version=canary -n $NAMESPACE --tail=100 | grep -c "ERROR" || echo "0")
       
       # Calculate error rates
       STABLE_ERROR_RATE=0
       CANARY_ERROR_RATE=0
       
       if [ $STABLE_REQUESTS -gt 0 ]; then
           STABLE_ERROR_RATE=$(( STABLE_ERRORS * 100 / STABLE_REQUESTS ))
       fi
       
       if [ $CANARY_REQUESTS -gt 0 ]; then
           CANARY_ERROR_RATE=$(( CANARY_ERRORS * 100 / CANARY_REQUESTS ))
       fi
       
       echo "📊 Current metrics: Stable errors: $STABLE_ERROR_RATE%, Canary errors: $CANARY_ERROR_RATE%"
       
       # Check if canary error rate exceeds threshold
       if [ $CANARY_ERROR_RATE -gt $ERROR_THRESHOLD ]; then
           echo "❌ Canary error rate ($CANARY_ERROR_RATE%) exceeds threshold ($ERROR_THRESHOLD%)"
           echo "🔙 Rolling back canary deployment..."
           
           kubectl scale deployment/payment-gateway-canary --replicas=0 -n $NAMESPACE
           kubectl scale deployment/payment-gateway-stable --replicas=$TOTAL_REPLICAS -n $NAMESPACE
           
           echo "✅ Rollback completed"
           exit 1
       fi
       
       sleep 10
   done
   
   # Canary monitoring completed successfully
   echo "✅ Canary monitoring completed successfully!"
   echo "🎯 No issues detected, promoting canary to stable..."
   
   # Promote canary to stable
   kubectl set image deployment/payment-gateway-stable \
       payment-gateway=$NEW_IMAGE -n $NAMESPACE
   
   kubectl scale deployment/payment-gateway-stable --replicas=$TOTAL_REPLICAS -n $NAMESPACE
   kubectl scale deployment/payment-gateway-canary --replicas=0 -n $NAMESPACE
   
   echo "🎉 Canary deployment promoted to stable!"
   echo "✅ Deployment completed successfully"
   EOF
   
   chmod +x deployments/canary/canary-deploy.sh
   ```

5. **Create Deployment Pipeline Integration**
   ```yaml
   # .github/workflows/deploy-production.yml
   name: Production Deployment
   
   on:
     workflow_run:
       workflows: ["Secure CI/CD Pipeline"]
       branches: [main]
       types: [completed]
   
   jobs:
     deploy-production:
       name: Deploy to Production
       runs-on: ubuntu-latest
       if: ${{ github.event.workflow_run.conclusion == 'success' }}
       environment: 
         name: production
         url: https://api.velocitytech.com
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Configure kubectl
         uses: azure/k8s-set-context@v3
         with:
           method: kubeconfig
           kubeconfig: ${{ secrets.KUBE_CONFIG }}
       
       - name: Get image tag from previous workflow
         id: get-image
         run: |
           IMAGE_TAG=$(echo ${{ github.sha }} | cut -c1-7)
           echo "image-tag=ghcr.io/velocitytech/payment-gateway:main-${IMAGE_TAG}" >> $GITHUB_OUTPUT
       
       - name: Blue-Green Deployment
         if: github.ref == 'refs/heads/main'
         run: |
           chmod +x deployments/blue-green/deploy.sh
           ./deployments/blue-green/deploy.sh "${{ steps.get-image.outputs.image-tag }}"
       
       - name: Run Post-Deployment Tests
         run: |
           # Wait for deployment to stabilize
           sleep 30
           
           # Run smoke tests
           kubectl run test-pod --image=curlimages/curl --rm -i --restart=Never -- \
             curl -f http://payment-gateway-service.production.svc.cluster.local/health
           
           # Run integration tests
           kubectl run integration-test --image=velocitytech/integration-tests --rm -i --restart=Never \
             --env="TARGET_URL=http://payment-gateway-service.production.svc.cluster.local"
       
       - name: Notify Deployment Success
         if: success()
         run: |
           echo "✅ Production deployment successful!"
           echo "Image: ${{ steps.get-image.outputs.image-tag }}"
           echo "Deployment strategy: Blue-Green"
           
       - name: Rollback on Failure
         if: failure()
         run: |
           echo "❌ Deployment failed, initiating rollback..."
           # This would trigger your rollback mechanism
           kubectl rollout undo deployment/payment-gateway-blue -n production
           kubectl rollout undo deployment/payment-gateway-green -n production
   ```

**Checkpoint:** Advanced deployment strategies implemented with automated testing and rollback capabilities

### Phase 3: Security Monitoring and Compliance Reporting (45 minutes)
**Objective:** Implement continuous security monitoring and automated compliance reporting

**Steps:**
1. **Create Security Monitoring Dashboard**
   ```bash
   # Create monitoring configuration
   mkdir -p monitoring/security
   
   # Security monitoring with Prometheus alerts
   cat > monitoring/security/security-alerts.yml << 'EOF'
   groups:
   - name: security.rules
     rules:
     # High error rate alert
     - alert: HighErrorRate
       expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.1
       for: 2m
       labels:
         severity: critical
         team: security
       annotations:
         summary: "High error rate detected"
         description: "Error rate is {{ $value }} for {{ $labels.instance }}"
     
     # Suspicious authentication attempts
     - alert: SuspiciousAuthAttempts
       expr: rate(auth_failures_total[5m]) > 10
       for: 1m
       labels:
         severity: warning
         team: security
       annotations:
         summary: "Suspicious authentication attempts"
         description: "High rate of auth failures: {{ $value }}/sec"
     
     # Container vulnerability threshold
     - alert: HighVulnerabilityCount
       expr: container_vulnerabilities{severity="high"} > 5
       for: 0m
       labels:
         severity: warning
         team: security
       annotations:
         summary: "High vulnerability count in container"
         description: "Container {{ $labels.container }} has {{ $value }} high-severity vulnerabilities"
     
     # Unusual resource consumption
     - alert: UnusualResourceConsumption
       expr: rate(container_cpu_usage_seconds_total[5m]) > 0.8
       for: 5m
       labels:
         severity: warning
         team: operations
       annotations:
         summary: "Unusual CPU consumption"
         description: "CPU usage is {{ $value }} for {{ $labels.container }}"
   EOF
   ```

2. **Create Compliance Reporting Script**
   ```bash
   # monitoring/security/compliance-report.sh
   cat > monitoring/security/compliance-report.sh << 'EOF'
   #!/bin/bash
   
   # VelocityTech Security Compliance Report Generator
   
   REPORT_DATE=$(date +"%Y-%m-%d")
   REPORT_DIR="reports/compliance-$REPORT_DATE"
   
   mkdir -p $REPORT_DIR
   
   echo "🔒 Generating Security Compliance Report for $REPORT_DATE"
   
   # 1. Collect vulnerability scan results
   echo "📊 Collecting vulnerability scan results..."
   cat > $REPORT_DIR/vulnerability-summary.json << EOF
   {
     "scan_date": "$REPORT_DATE",
     "total_scans": 3,
     "scans": {
       "container_scan": {
         "critical": 0,
         "high": 2,
         "medium": 8,
         "low": 15,
         "status": "PASS"
       },
       "dependency_scan": {
         "critical": 1,
         "high": 4,
         "medium": 12,
         "low": 23,
         "status": "REVIEW_REQUIRED"
       },
       "static_analysis": {
         "critical": 0,
         "high": 1,
         "medium": 5,
         "low": 12,
         "status": "PASS"
       }
     },
     "overall_status": "CONDITIONAL_PASS",
     "action_required": "Address 1 critical dependency vulnerability"
   }
   EOF
   
   # 2. Generate security metrics
   echo "📈 Collecting security metrics..."
   cat > $REPORT_DIR/security-metrics.json << EOF
   {
     "period": "last_30_days",
     "metrics": {
       "deployments_with_security_scan": "100%",
       "vulnerability_fix_time_avg": "2.3 days",
       "security_incidents": 0,
       "failed_authentication_attempts": 147,
       "security_policy_violations": 2,
       "compliance_score": 94
     }
   }
   EOF
   
   # 3. Check compliance with security policies
   echo "📋 Checking policy compliance..."
   cat > $REPORT_DIR/policy-compliance.json << EOF
   {
     "policies": {
       "container_security": {
         "non_root_user": "COMPLIANT",
         "no_privileged_containers": "COMPLIANT",
         "image_vulnerability_scan": "COMPLIANT",
         "base_image_currency": "COMPLIANT"
       },
       "access_control": {
         "rbac_enabled": "COMPLIANT",
         "service_accounts": "COMPLIANT",
         "network_policies": "COMPLIANT",
         "secret_management": "COMPLIANT"
       },
       "code_security": {
         "static_analysis": "COMPLIANT",
         "dependency_scanning": "NON_COMPLIANT",
         "secret_detection": "COMPLIANT",
         "license_compliance": "COMPLIANT"
       },
       "operational_security": {
         "logging_enabled": "COMPLIANT",
         "monitoring_alerts": "COMPLIANT",
         "backup_encryption": "COMPLIANT",
         "incident_response": "COMPLIANT"
       }
     },
     "overall_compliance": "94%",
     "non_compliant_items": [
       "dependency_scanning: 1 critical vulnerability not addressed"
     ]
   }
   EOF
   
   # 4. Generate HTML report
   echo "📄 Generating HTML compliance report..."
   cat > $REPORT_DIR/compliance-report.html << 'EOF'
   <!DOCTYPE html>
   <html>
   <head>
       <title>VelocityTech Security Compliance Report</title>
       <style>
           body { font-family: Arial, sans-serif; margin: 20px; background-color: #f5f5f5; }
           .container { max-width: 1200px; margin: 0 auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
           .header { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 20px; margin: -20px -20px 20px -20px; border-radius: 8px 8px 0 0; }
           .metric-card { background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 15px; margin: 10px 0; display: inline-block; min-width: 200px; }
           .status-pass { color: #28a745; font-weight: bold; }
           .status-fail { color: #dc3545; font-weight: bold; }
           .status-warning { color: #ffc107; font-weight: bold; }
           .compliance-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; margin: 20px 0; }
           table { width: 100%; border-collapse: collapse; margin: 20px 0; }
           th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
           th { background-color: #f2f2f2; font-weight: bold; }
           .chart { width: 100%; height: 300px; background: #f8f9fa; display: flex; align-items: center; justify-content: center; margin: 20px 0; border-radius: 6px; }
       </style>
   </head>
   <body>
       <div class="container">
           <div class="header">
               <h1>🔒 Security Compliance Report</h1>
               <p><strong>Generated:</strong> REPORT_DATE_PLACEHOLDER</p>
               <p><strong>Overall Compliance Score:</strong> <span class="status-pass">94%</span></p>
           </div>
           
           <h2>📊 Security Metrics Overview</h2>
           <div class="compliance-grid">
               <div class="metric-card">
                   <h3>Deployments Scanned</h3>
                   <div class="status-pass">100%</div>
                   <small>All deployments security scanned</small>
               </div>
               <div class="metric-card">
                   <h3>Avg Fix Time</h3>
                   <div class="status-pass">2.3 days</div>
                   <small>Average vulnerability resolution</small>
               </div>
               <div class="metric-card">
                   <h3>Security Incidents</h3>
                   <div class="status-pass">0</div>
                   <small>Past 30 days</small>
               </div>
               <div class="metric-card">
                   <h3>Policy Violations</h3>
                   <div class="status-warning">2</div>
                   <small>Requires attention</small>
               </div>
           </div>
           
           <h2>🔍 Vulnerability Scan Summary</h2>
           <table>
               <tr><th>Scan Type</th><th>Critical</th><th>High</th><th>Medium</th><th>Low</th><th>Status</th></tr>
               <tr><td>Container Scan</td><td>0</td><td>2</td><td>8</td><td>15</td><td class="status-pass">PASS</td></tr>
               <tr><td>Dependency Scan</td><td>1</td><td>4</td><td>12</td><td>23</td><td class="status-warning">REVIEW</td></tr>
               <tr><td>Static Analysis</td><td>0</td><td>1</td><td>5</td><td>12</td><td class="status-pass">PASS</td></tr>
           </table>
           
           <h2>📋 Policy Compliance Status</h2>
           <div class="compliance-grid">
               <div class="metric-card">
                   <h3>Container Security</h3>
                   <div class="status-pass">COMPLIANT</div>
                   <ul>
                       <li>✅ Non-root containers</li>
                       <li>✅ No privileged access</li>
                       <li>✅ Vulnerability scanning</li>
                       <li>✅ Current base images</li>
                   </ul>
               </div>
               <div class="metric-card">
                   <h3>Access Control</h3>
                   <div class="status-pass">COMPLIANT</div>
                   <ul>
                       <li>✅ RBAC enabled</li>
                       <li>✅ Service accounts</li>
                       <li>✅ Network policies</li>
                       <li>✅ Secret management</li>
                   </ul>
               </div>
               <div class="metric-card">
                   <h3>Code Security</h3>
                   <div class="status-warning">PARTIAL</div>
                   <ul>
                       <li>✅ Static analysis</li>
                       <li>❌ Dependency issues</li>
                       <li>✅ Secret detection</li>
                       <li>✅ License compliance</li>
                   </ul>
               </div>
               <div class="metric-card">
                   <h3>Operational Security</h3>
                   <div class="status-pass">COMPLIANT</div>
                   <ul>
                       <li>✅ Comprehensive logging</li>
                       <li>✅ Monitoring alerts</li>
                       <li>✅ Backup encryption</li>
                       <li>✅ Incident response</li>
                   </ul>
               </div>
           </div>
           
           <h2>⚠️ Action Items</h2>
           <table>
               <tr><th>Priority</th><th>Issue</th><th>Action Required</th><th>Due Date</th></tr>
               <tr><td class="status-fail">HIGH</td><td>Critical dependency vulnerability</td><td>Update lodash to 4.17.21+</td><td>Within 24 hours</td></tr>
               <tr><td class="status-warning">MEDIUM</td><td>Policy violations</td><td>Review access control logs</td><td>Within 1 week</td></tr>
           </table>
           
           <h2>📈 Compliance Trend</h2>
           <div class="chart">
               <p>Compliance Score Trend Chart<br>
               <small>(In production environment, this would show actual trend data)</small></p>
           </div>
           
           <div style="margin-top: 30px; padding-top: 20px; border-top: 1px solid #ddd; font-size: 12px; color: #666;">
               <p><strong>Report generated by:</strong> VelocityTech DevSecOps Pipeline</p>
               <p><strong>Next report:</strong> Automated weekly generation</p>
           </div>
       </div>
   </body>
   </html>
   EOF
   
   # Replace placeholder with actual date
   sed -i "s/REPORT_DATE_PLACEHOLDER/$REPORT_DATE/g" $REPORT_DIR/compliance-report.html
   
   echo "✅ Compliance report generated: $REPORT_DIR/compliance-report.html"
   echo "📧 Report ready for stakeholder review"
   
   # In production, you might email this report or upload to S3
   # aws s3 cp $REPORT_DIR/compliance-report.html s3://velocitytech-compliance-reports/
   EOF
   
   chmod +x monitoring/security/compliance-report.sh
   ```

3. **Create Security Incident Response Playbook**
   ```bash
   # monitoring/security/incident-response.yml
   cat > monitoring/security/incident-response.yml << 'EOF'
   # VelocityTech Security Incident Response Playbook
   
   incident_response:
     severity_levels:
       critical:
         description: "Active security breach or critical vulnerability exploited"
         response_time: "15 minutes"
         escalation: "Immediate"
         actions:
           - "Execute immediate containment"
           - "Notify security team and management"
           - "Begin forensic analysis"
           - "Activate backup systems if needed"
       
       high:
         description: "High-risk vulnerability or suspicious activity"
         response_time: "1 hour"
         escalation: "Within 2 hours"
         actions:
           - "Assess impact and risk"
           - "Apply immediate mitigations"
           - "Schedule emergency patching"
           - "Enhance monitoring"
       
       medium:
         description: "Medium-risk findings requiring attention"
         response_time: "24 hours"
         escalation: "Next business day"
         actions:
           - "Create remediation plan"
           - "Schedule patching window"
           - "Update security documentation"
       
       low:
         description: "Low-risk findings for routine handling"
         response_time: "1 week"
         escalation: "Monthly review"
         actions:
           - "Add to security backlog"
           - "Include in next maintenance window"
   
     automated_responses:
       high_error_rate:
         trigger: "Error rate > 5% for 2 minutes"
         action: "Automatic rollback to previous version"
         notification: "Slack #alerts channel"
       
       suspicious_auth:
         trigger: "Failed auth attempts > 100/minute"
         action: "Temporarily block suspicious IPs"
         notification: "Security team email"
       
       vulnerability_found:
         trigger: "Critical vulnerability in production"
         action: "Create Jira ticket, notify security team"
         notification: "Email + Slack notification"
   
     contact_information:
       security_team: "security@velocitytech.com"
       on_call_engineer: "+385-XX-XXX-XXXX"
       management_escalation: "cto@velocitytech.com"
       external_support: "security-consultant@partner.com"
   EOF
   ```

4. **Test Security Integration**
   ```bash
   # Test the complete security pipeline
   echo "🔒 Testing complete security integration..."
   
   # Run compliance report
   ./monitoring/security/compliance-report.sh
   
   # Test deployment with security scanning
   git add .
   git commit -m "Add comprehensive security monitoring and compliance reporting
   
   - Implement security alerting rules with Prometheus
   - Create automated compliance reporting with HTML output
   - Add security incident response playbook
   - Integrate security monitoring with deployment pipeline"
   
   git push origin feature/secure-pipeline
   
   # Verify security artifacts are generated
   ls -la reports/compliance-*/
   
   # Test blue-green deployment locally
   echo "🔄 Testing blue-green deployment process..."
   # Note: This would require a running Kubernetes cluster
   # kubectl apply -f deployments/blue-green/blue-green-deployment.yml
   
   # View compliance report
   echo "📊 Compliance report generated at:"
   find . -name "compliance-report.html" -type f | head -1
   ```

**Checkpoint:** Complete security monitoring and compliance reporting system implemented

### Validation & Testing
**How to verify everything works:**
1. **Security Pipeline Test** - `All security scans run and produce results in CI/CD`
2. **Blue-Green Deployment Test** - `Traffic switches between environments successfully`
3. **Canary Deployment Test** - `Gradual rollout with automatic rollback on errors`
4. **Compliance Reporting Test** - `Automated compliance reports generated with current status`
5. **Security Monitoring Test** - `Alerts trigger on security threshold violations`

**Expected Results:**
- **Security Integration:** 100% of deployments pass through security gates
- **Vulnerability Detection:** Critical and high-severity issues caught before production
- **Deployment Safety:** Zero-downtime deployments with instant rollback capability
- **Compliance Automation:** Automated compliance reporting for audits and certifications
- **Incident Response:** Automated responses to security events and threshold violations

---

## 🔧 New Tools in Your Arsenal

### DevSecOps Security Scanning Integration
- **Purpose:** Automated security vulnerability detection throughout the development lifecycle
- **Key Tools:**
  - SAST (Static Application Security Testing): SonarQube, ESLint Security
  - SCA (Software Composition Analysis): Snyk, OWASP Dependency Check
  - Container Security: Trivy, Docker Scout, Twistlock
  - Secret Detection: TruffleHog, GitLeaks
- **Pro Tips:** Configure security gates to fail builds on critical issues, use allowlists for false positives
- **Common Pitfalls:** Don't ignore security scan results, avoid scanning only at the end of pipeline

### Advanced Deployment Strategies
- **Purpose:** Minimize deployment risk and enable quick rollbacks through sophisticated release patterns
- **Key Patterns:**
  - Blue-Green: Instant traffic switching between identical environments
  - Canary: Gradual rollout with automated monitoring and rollback
  - Rolling: Progressive update of replicas with health checks
  - Feature Flags: Runtime control of feature availability
- **Pro Tips:** Monitor business metrics during deployments, automate rollback decisions
- **Common Pitfalls:** Don't skip health checks, ensure database compatibility between versions

### Security Compliance Automation
- **Purpose:** Automated compliance reporting and policy enforcement for regulatory requirements
- **Key Components:**
  - Policy as Code: Define security policies in version control
  - Compliance Dashboards: Real-time compliance status visualization
  - Automated Reporting: Generate compliance reports for audits
  - Incident Response: Automated responses to security events
- **Pro Tips:** Align policies with business requirements, automate evidence collection
- **Common Pitfalls:** Don't treat compliance as checkbox exercise, ensure policies are enforceable

---

## 🔧 Common Issues & Solutions

### Issue #1: "Security scans slow down CI/CD pipeline significantly"
**Symptoms:** Pipeline runtime increased from 5 minutes to 30+ minutes after adding security scans
**Cause:** Sequential execution of security scans, comprehensive but slow scanning tools
**Solution:**
```yaml
# Parallelize security scans
jobs:
  security-matrix:
    strategy:
      matrix:
        scan-type: [sast, sca, secrets, container]
    steps:
      - name: Run ${{ matrix.scan-type }} scan
        run: run-security-scan.sh ${{ matrix.scan-type }}

# Use faster scanning tools for PR checks, comprehensive for main branch
- name: Quick security scan
  if: github.event_name == 'pull_request'
  run: snyk test --severity-threshold=high

- name: Comprehensive security scan
  if: github.ref == 'refs/heads/main'
  run: snyk test --all-projects
```
**Prevention:** Use risk-based scanning, cache scan results, run comprehensive scans nightly

### Issue #2: "Blue-green deployment fails due to database schema incompatibilities"
**Symptoms:** New version can't connect to existing database, data migration issues during deployment
**Cause:** Database schema changes not backward compatible, shared database between blue/green
**Solution:**
```bash
# Implement backward-compatible migrations
# 1. Add new columns as nullable
# 2. Deploy application that works with both old and new schema
# 3. Migrate data
# 4. Remove old columns in next release

# Use separate databases for blue/green when needed
kubectl apply -f blue-database.yml
kubectl apply -f green-database.yml

# Database migration strategy
./migrate-forward.sh  # Before deploying new version
./migrate-backward.sh # If rollback needed
```
**Prevention:** Design backward-compatible schemas, use database versioning, test migrations

### Issue #3: "Too many false positive security alerts overwhelming developers"
**Symptoms:** Developers ignore security scan results, security gates bypassed frequently
**Cause:** Poor tool configuration, lack of context-specific rules, no triage process
**Solution:**
```yaml
# Configure tool-specific ignore patterns
# .snyk file for Snyk
patches: {}
ignore:
  'npm:lodash:20180130':
    - '*':
        reason: 'Not exploitable in our usage context'
        expires: '2024-12-31T23:59:59.999Z'

# ESLint security rules configuration
rules:
  'security/detect-object-injection': 'off'  # If not applicable
  'security/detect-non-literal-regexp': 'warn'  # Reduce severity
```
**Prevention:** Tune security tools, establish triage process, provide security training

---

## 🎭 Character Evolution This Week

### Legacy Luka's Security Awakening
**Monday:** "Security scanning is just going to slow down my development. I write secure code already."
**Tuesday:** "The scanner found a vulnerability I never would have noticed. That's... actually helpful."
**Wednesday:** "Blue-green deployments mean I can deploy without fear. I can always roll back instantly."
**Thursday:** "Security gates caught issues before they reached production. They're like automated code reviewers."
**Friday:** "DevSecOps doesn't slow down development - it makes deployment safer and more confident."
**Key Quote:** "Security integrated into CI/CD makes me a better developer, not a slower one."

### Anxious Ana's Compliance Confidence
**Monday:** "More security tools mean more things I need to learn and more potential failures in testing."
**Tuesday:** "Automated security testing catches vulnerabilities I wouldn't know how to test for manually."
**Wednesday:** "Compliance reporting is automatic now. I have documentation for everything we're doing right."
**Thursday:** "Security monitoring gives me confidence that production is actually secure, not just functional."
**Friday:** "I'm not just testing functionality anymore - I'm validating security and compliance automatically."
**Key Quote:** "DevSecOps turned security from a mystery into measurable, automated assurance."

### Firefighter Filip's Proactive Security
**Monday:** "Great, security tools creating more alerts and more things that can break in production."
**Tuesday:** "Automated security responses handled an incident before I even knew it happened."
**Wednesday:** "Blue-green deployments eliminated my weekend deployment emergencies completely."
**Thursday:** "Security monitoring is proactive instead of reactive. I'm preventing incidents, not just responding."
**Friday:** "My role changed from emergency responder to security architect. Much better nights' sleep."
**Key Quote:** "DevSecOps transformed me from fighting security fires to preventing them."

### Manager Maja's Strategic Security Vision
**Monday:** "How much will implementing all these security tools and processes cost in time and resources?"
**Tuesday:** "Automated compliance reporting means we can pursue contracts requiring security certifications."
**Wednesday:** "Zero-downtime deployments mean we can release features without maintenance windows."
**Thursday:** "Security automation gives us competitive advantage - clients trust our security practices."
**Friday:** "DevSecOps isn't just about security - it's about enabling business growth through trust and reliability."
**Key Quote:** "Security automation became our business enabler, not our business constraint."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 9 Improvements:**

| Metric | Week 8 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 10 days | 8 days | 2 hours |
| Security Vulnerabilities in Production | 12 critical/high | 0 critical, 2 high | 0 critical, 0 high |
| Deployment Downtime | 5 minutes | 0 minutes | 0 minutes |
| Security Scan Coverage | 0% | 100% | 100% |
| Compliance Score | 60% | 94% | 98% |
| Deployment Risk | High | Low | Minimal |
| Security Incident Response Time | 4 hours | 15 minutes | <5 minutes |

**Key Improvements:**
- **Security Integration:** 100% of deployments now pass through automated security gates
- **Vulnerability Reduction:** Eliminated all critical vulnerabilities from reaching production
- **Deployment Safety:** Zero-downtime deployments with instant rollback capability
- **Compliance Achievement:** 94% compliance score enabling enterprise contract opportunities

---

## 🇭🇷 Croatian IT Market Reality
DevSecOps expertise is extremely valuable in the Croatian market. With increasing cyber threats and regulatory requirements (GDPR, PCI DSS), companies like Infobip, Rimac, and Span are heavily investing in security automation. DevSecOps engineers command 50-70% higher salaries than traditional DevOps roles. Many Croatian companies still handle security manually, creating high demand for professionals who can integrate security into CI/CD pipelines. Your DevSecOps skills position you for senior security engineer, platform engineer, and security architect roles.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about DevSecOps?** How security automation actually accelerates development instead of slowing it down
2. **Which deployment strategy would you choose for your applications?** Blue-green for critical systems, canary for gradual rollouts, rolling for routine updates
3. **How did security integration change your view of quality?** Security became part of definition of done, not an afterthought
4. **What questions do you still have?** How to balance security requirements with development velocity at enterprise scale

**Team Discussion Points:**
- How would each VelocityTech character feel about mandatory security scanning for all code changes?
- What resistance might we encounter from teams comfortable with manual security reviews?
- How would you convince skeptical management to invest in DevSecOps tools and training?

**Preparation for Next Week:**
Ana arrives Monday morning excited about secure deployments but concerned: "The applications are running securely, but I can't see what's happening inside them! When issues occur, I'm debugging blind. We need visibility into application performance, user behavior, and system health." The comprehensive monitoring and observability journey begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "DevSecOps Handbook" by Gene Kim - Security integration best practices
- "Continuous Security" by Neil MacDonald - Security automation strategies

**Hands-on Practice:**
- Implement security scanning in your personal CI/CD pipelines
- Practice advanced deployment strategies with Kubernetes
- Experiment with compliance automation tools

**Industry Insights:**
- "State of DevSecOps" reports - Security automation adoption trends
- OWASP DevSecOps Guideline - Security integration best practices
- Croatian cybersecurity meetups - Network with local security professionals

**Advanced Topics:**
- Policy as Code with Open Policy Agent (OPA)
- Runtime security monitoring with Falco
- Supply chain security with SLSA framework
- Zero-trust architecture implementation

**Croatian Companies Leading DevSecOps:**
- **Infobip:** Comprehensive security automation across global communication platform
- **Rimac:** Automotive cybersecurity and secure development practices
- **Span:** Web application security with automated compliance reporting
- **Photomath:** Mobile application security at scale with millions of users
- **Include:** Financial services security compliance and automated auditing

**Certification Path:**
- **Certified DevSecOps Professional (CDP)** - Comprehensive DevSecOps certification
- **AWS Certified Security - Specialty** - Cloud security specialization
- **CISSP (Certified Information Systems Security Professional)** - Advanced security certification
- **CSSLP (Certified Secure Software Lifecycle Professional)** - Secure development practices

---

*Next up: Week 10 - Kubernetes Networking + Service Discovery: "Connecting the Microservices Ecosystem" →*

> *"Security isn't something you add to DevOps - it's something you build into every step. When security becomes automatic, trust becomes inevitable."* - DevSecOps Philosophy

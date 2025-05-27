# Week 7: Infrastructure Provisioning - "From Snowflakes to Consistency" 🏗️
**VelocityTech Transformation Week 7**

**Story Context:** Kubernetes works great locally, but production servers are manually configured snowflakes that take weeks to provision  
**Focus:** Implementing Infrastructure as Code with Terraform for consistent, repeatable infrastructure  
**Characters in Focus:** Filip's server provisioning nightmares, Maja's scaling frustrations, Ana's environment inconsistencies

---

## 🔥 The Weekly Crisis: "The Great Server Provisioning Disaster"
**Setting:** VelocityTech office, Wednesday 9:15 AM. Filip surrounded by server documentation, sticky notes, and three cups of coffee.  
**The Problem:** Client demands production Kubernetes cluster by Friday, but provisioning new servers manually takes 2-3 weeks

**Scene:** [VelocityTech main floor and server room. Filip flipping through a thick binder of server setup procedures. Ana trying to replicate production environment for testing. Maja on the phone with a frustrated client. Luka waiting for a development server that was requested last month.]

**FILIP** *(frantically reading through a 67-page server setup manual)*  
> "Step 43: Configure firewall rules... Step 44: Install Docker... Step 45: Set up monitoring agent..."  
> *(looking exhausted)* "I've been working on this production server for 8 days, and I'm only halfway through the checklist."  
> "And that's assuming nothing goes wrong. Yesterday I had to restart because I missed step 23."

**MAJA** *(hanging up phone, visibly stressed)*  
> "Client needs their production environment by Friday for the big launch..."  
> "They're willing to pay extra for expedited setup. How much extra time do you need, Filip?"

**FILIP** *(checking his notebook)*  
> "If everything goes perfectly... maybe another week? But that's optimistic."  
> "We need 3 servers for the Kubernetes cluster, plus database server, monitoring server, load balancer..."  
> *(overwhelmed)* "Each one takes 3-4 days to configure properly."

**ANA** *(frustrated, pointing at two different servers)*  
> "My staging environment behaves differently than production because they were set up months apart!"  
> "Staging server has Ubuntu 20.04, production has 18.04... different Docker versions, different Kubernetes versions..."  
> "How am I supposed to test reliably when every environment is unique?"

**LUKA** *(checking his development server request)*  
> "I requested a development server 4 weeks ago for the new microservice..."  
> "The project deadline is next week, but I still don't have a server to deploy to!"

**FILIP** *(pulling out another manual)*  
> "Each server is configured slightly differently because the documentation keeps evolving..."  
> "Server A was built using v1.2 of my procedure, Server B with v2.1, Server C with v2.8..."  
> *(frustrated)* "They're all snowflakes! No two are exactly alike!"

**MAJA** *(calculating costs)*  
> "We need to scale up for three more clients next month..."  
> "At this rate, we'll need to hire two more people just to provision servers!"

**ANA** *(looking at her testing matrix)*  
> "I have 6 different environment configurations to test against..."  
> "Each behaves slightly differently because of setup variations. It's impossible to get consistent results!"

**MARKO** *(entering, immediately recognizing the chaos)*  
> "Let me guess - manual server provisioning is becoming the bottleneck for business growth?"  
> *(to you)* "This is exactly why Infrastructure as Code exists. Ready to turn servers into software?"

**FILIP** *(hopeful but skeptical)*  
> "I've heard about this 'infrastructure as code' thing, but doesn't that just add more complexity?"  
> "Now I have to learn programming on top of system administration?"

**MAJA** *(desperately)*  
> "There has to be a way to provision servers automatically..."  
> "Something that guarantees consistency and doesn't require Filip to work 16-hour days!"

**[All eyes turn to you - the intern who might understand modern infrastructure practices.]**

**LUKA** *(curious)*  
> "If infrastructure can be code, does that mean we can version control it like applications?"  
> "And test it, and deploy it automatically?"

**[Your systems thinking activates. This isn't just about servers - it's about treating infrastructure like software.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Create infrastructure with Terraform** - Write declarative code that provisions cloud resources consistently and repeatably
2. **Implement infrastructure versioning and collaboration** - Use Git workflows for infrastructure changes with proper review processes
3. **Design modular, reusable infrastructure components** - Build infrastructure modules that can be shared across projects and environments

**Real-world Connection:** Infrastructure as Code skills are essential for scalable operations in Croatian IT companies. Organizations like Infobip, Rimac, and Span use Terraform extensively for managing their cloud infrastructure. IaC expertise commands premium salaries and is required for senior DevOps and platform engineering roles.

---

## 📚 The Knowledge Foundation

### Infrastructure as Code (IaC) Principles and Benefits
**The Problem it Solves:** Manual server provisioning, configuration drift, inconsistent environments, slow scaling, human error, no change tracking  
**How it Works:** Infrastructure defined in code files that can be versioned, tested, and automatically applied to create consistent environments

**Traditional vs IaC Approach:**
```
Traditional Infrastructure:        Infrastructure as Code:
┌─────────────────────────┐       ┌─────────────────────────┐
│ Manual server setup     │       │ Declarative config      │
│ 67-page checklist      │       │ Version controlled      │
│ 3-4 days per server    │       │ 5 minutes per stack     │
│ Unique snowflakes       │       │ Identical replicas      │
│ No change tracking      │       │ Full audit trail        │
│ Human error prone       │       │ Automated and reliable  │
└─────────────────────────┘       └─────────────────────────┘
```

**Core IaC Principles:**
- **Declarative:** Describe desired state, not steps to achieve it
- **Idempotent:** Running multiple times produces same result
- **Versionable:** Infrastructure changes tracked in Git
- **Testable:** Validate infrastructure before applying
- **Reproducible:** Same code creates identical environments

**IaC Benefits:**
- **Speed:** Provision complex environments in minutes
- **Consistency:** Eliminate configuration drift and snowflakes
- **Scalability:** Easily replicate environments for growth
- **Reliability:** Reduce human error through automation
- **Auditability:** Track all infrastructure changes over time
- **Cost Control:** Consistent resource sizing and cleanup

**Real-world Example:** Netflix provisions thousands of AWS resources daily using Infrastructure as Code, enabling rapid scaling globally  
**VelocityTech Connection:** IaC would eliminate Filip's manual provisioning bottleneck and give Ana consistent test environments

### Terraform Fundamentals and AWS Provider
**The Problem it Solves:** Cloud resource management complexity, vendor lock-in, inconsistent provisioning across environments  
**How it Works:** Terraform uses declarative configuration files to create, update, and destroy infrastructure resources across multiple cloud providers

**Terraform Core Concepts:**
- **Providers:** Interfaces to cloud platforms (AWS, Azure, GCP)
- **Resources:** Infrastructure components (VMs, networks, databases)
- **Data Sources:** Query existing infrastructure information
- **Variables:** Parameterize configurations for reusability
- **Outputs:** Export values for use by other configurations
- **State:** Track actual infrastructure vs desired configuration

**Basic Terraform Workflow:**
```bash
terraform init     # Initialize and download providers
terraform plan     # Preview changes before applying
terraform apply    # Create/modify infrastructure
terraform destroy  # Clean up resources
```

**Simple AWS Infrastructure Example:**
```hcl
# Configure AWS provider
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Configure AWS region
provider "aws" {
  region = "eu-west-1"  # Ireland region for European users
}

# Create VPC
resource "aws_vpc" "velocitytech_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name        = "VelocityTech-VPC"
    Environment = "production"
    Project     = "payment-gateway"
  }
}

# Create EC2 instance
resource "aws_instance" "web_server" {
  ami           = "ami-0c02fb55956c7d316"  # Amazon Linux 2
  instance_type = "t3.micro"
  
  vpc_security_group_ids = [aws_security_group.web_sg.id]
  subnet_id              = aws_subnet.public_subnet.id
  
  tags = {
    Name = "VelocityTech-WebServer"
  }
}
```

**Real-world Example:** Airbnb manages thousands of AWS resources across multiple regions using Terraform, ensuring consistent deployments worldwide  
**VelocityTech Connection:** Terraform would allow Filip to provision entire Kubernetes clusters in minutes instead of weeks

### State Management and Team Collaboration
**The Problem it Solves:** Multiple team members working on same infrastructure, state conflicts, no change coordination, lost infrastructure state  
**How it Works:** Terraform state files track actual infrastructure, remote backends enable team collaboration with locking

**Terraform State Fundamentals:**
- **Local State:** terraform.tfstate file stores resource mapping
- **Remote State:** Centralized state storage for team collaboration
- **State Locking:** Prevent concurrent modifications
- **State Backup:** Automatic versioning and recovery

**Remote Backend Configuration:**
```hcl
# Backend configuration for team collaboration
terraform {
  backend "s3" {
    bucket         = "velocitytech-terraform-state"
    key            = "production/terraform.tfstate"
    region         = "eu-west-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}
```

**Infrastructure Versioning Best Practices:**
- **Git Integration:** Store Terraform configs in version control
- **Branch Strategy:** Use feature branches for infrastructure changes
- **Code Review:** Peer review infrastructure changes like application code
- **Automated Testing:** Validate configurations before applying
- **Change Documentation:** Link infrastructure changes to tickets/issues

**Team Collaboration Workflow:**
```bash
# Developer workflow
git checkout -b feature/add-monitoring-server
# Make Terraform changes
terraform plan  # Review changes locally
git commit -m "Add monitoring server infrastructure"
git push origin feature/add-monitoring-server
# Create pull request for review
# After approval and merge:
terraform apply  # Apply changes to shared infrastructure
```

**Real-world Example:** Spotify's infrastructure team uses shared Terraform state with automated workflows, enabling hundreds of developers to safely modify infrastructure  
**VelocityTech Connection:** Proper state management would allow the team to collaborate on infrastructure changes without conflicts

**Key Takeaways:**
- Infrastructure as Code treats servers like software with version control
- Terraform provides declarative, cloud-agnostic infrastructure management
- Remote state and locking enable safe team collaboration
- Modular design allows infrastructure reuse across projects

---

## 🛠️ Lab Mission: "The Infrastructure Automation Revolution"
**Scenario:** VelocityTech needs production Kubernetes cluster by Friday, but manual provisioning takes weeks  
**Your Mission:** Create Terraform infrastructure that provisions complete VelocityTech environment in minutes  
**Success Criteria:** Reproducible AWS infrastructure including VPC, security groups, EC2 instances, and basic Kubernetes setup

### Phase 1: AWS Account Setup and Basic Terraform (60 minutes)
**Objective:** Set up AWS credentials and create first Terraform-managed infrastructure

**Steps:**
1. **AWS Account and Credentials Setup**
   ```bash
   # Install AWS CLI (if not already installed)
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   
   # Configure AWS credentials (use AWS Academy credentials if available)
   aws configure
   # AWS Access Key ID: [Your AWS Academy Key]
   # AWS Secret Access Key: [Your AWS Academy Secret]
   # Default region name: eu-west-1
   # Default output format: json
   
   # Verify AWS access
   aws sts get-caller-identity
   aws ec2 describe-regions
   ```
   - Expected output: Your AWS account information and available regions
   - Troubleshooting: If credentials fail, check AWS Academy lab status and regenerate if needed

2. **Terraform Installation and Initialization**
   ```bash
   # Install Terraform
   wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
   unzip terraform_1.6.0_linux_amd64.zip
   sudo mv terraform /usr/local/bin/
   
   # Verify installation
   terraform version
   # Expected: Terraform v1.6.0
   
   # Create VelocityTech infrastructure project
   mkdir velocitytech-infrastructure
   cd velocitytech-infrastructure
   
   # Initialize Git repository
   git init
   echo "*.tfstate*" >> .gitignore
   echo "*.terraform*" >> .gitignore
   echo ".terraform.lock.hcl" >> .gitignore
   ```

3. **Create Basic VPC Infrastructure**
   ```hcl
   # main.tf
   terraform {
     required_version = ">= 1.0"
     required_providers {
       aws = {
         source  = "hashicorp/aws"
         version = "~> 5.0"
       }
     }
   }
   
   # Configure AWS Provider
   provider "aws" {
     region = var.aws_region
   }
   
   # Data source for availability zones
   data "aws_availability_zones" "available" {
     state = "available"
   }
   
   # Create VPC
   resource "aws_vpc" "velocitytech_vpc" {
     cidr_block           = var.vpc_cidr
     enable_dns_hostnames = true
     enable_dns_support   = true
   
     tags = {
       Name        = "${var.project_name}-VPC"
       Environment = var.environment
       Project     = var.project_name
     }
   }
   
   # Create Internet Gateway
   resource "aws_internet_gateway" "velocitytech_igw" {
     vpc_id = aws_vpc.velocitytech_vpc.id
   
     tags = {
       Name = "${var.project_name}-IGW"
     }
   }
   
   # Create public subnets
   resource "aws_subnet" "public_subnets" {
     count = 2
   
     vpc_id                  = aws_vpc.velocitytech_vpc.id
     cidr_block              = "10.0.${count.index + 1}.0/24"
   availability_zone       = data.aws_availability_zones.available.names[count.index]
     map_public_ip_on_launch = true
   
     tags = {
       Name = "${var.name_prefix}-Public-Subnet-${count.index + 1}"
       Type = "public"
     }
   }
   
   resource "aws_subnet" "private" {
     count = var.private_subnet_count
   
     vpc_id            = aws_vpc.main.id
     cidr_block        = "10.0.${count.index + 10}.0/24"
     availability_zone = data.aws_availability_zones.available.names[count.index]
   
     tags = {
       Name = "${var.name_prefix}-Private-Subnet-${count.index + 1}"
       Type = "private"
     }
   }
   
   data "aws_availability_zones" "available" {
     state = "available"
   }
   EOF
   
   # VPC module variables
   cat > modules/vpc/variables.tf << 'EOF'
   variable "vpc_cidr" {
     description = "CIDR block for VPC"
     type        = string
     default     = "10.0.0.0/16"
   }
   
   variable "name_prefix" {
     description = "Prefix for resource names"
     type        = string
   }
   
   variable "environment" {
     description = "Environment name"
     type        = string
   }
   
   variable "public_subnet_count" {
     description = "Number of public subnets"
     type        = number
     default     = 2
   }
   
   variable "private_subnet_count" {
     description = "Number of private subnets"
     type        = number
     default     = 2
   }
   EOF
   
   # VPC module outputs
   cat > modules/vpc/outputs.tf << 'EOF'
   output "vpc_id" {
     value = aws_vpc.main.id
   }
   
   output "public_subnet_ids" {
     value = aws_subnet.public[*].id
   }
   
   output "private_subnet_ids" {
     value = aws_subnet.private[*].id
   }
   
   output "internet_gateway_id" {
     value = aws_internet_gateway.main.id
   }
   EOF
   ```

2. **Create Environment-Specific Configurations**
   ```bash
   # Create environments
   mkdir -p environments/development environments/staging environments/production
   
   # Development environment
   cat > environments/development/main.tf << 'EOF'
   terraform {
     required_providers {
       aws = {
         source  = "hashicorp/aws"
         version = "~> 5.0"
       }
     }
   }
   
   provider "aws" {
     region = var.aws_region
   }
   
   module "vpc" {
     source = "../../modules/vpc"
     
     name_prefix           = "VelocityTech-Dev"
     environment          = "development"
     vpc_cidr            = "10.1.0.0/16"
     public_subnet_count = 1
     private_subnet_count = 1
   }
   
   # Smaller instances for development
   resource "aws_instance" "dev_server" {
     ami           = data.aws_ami.amazon_linux.id
     instance_type = "t3.micro"
     subnet_id     = module.vpc.public_subnet_ids[0]
     
     tags = {
       Name = "VelocityTech-Dev-Server"
       Environment = "development"
     }
   }
   
   data "aws_ami" "amazon_linux" {
     most_recent = true
     owners      = ["amazon"]
     
     filter {
       name   = "name"
       values = ["amzn2-ami-hvm-*-x86_64-gp2"]
     }
   }
   EOF
   
   # Development variables
   cat > environments/development/variables.tf << 'EOF'
   variable "aws_region" {
     description = "AWS region"
     type        = string
     default     = "eu-west-1"
   }
   EOF
   
   # Production environment
   cat > environments/production/main.tf << 'EOF'
   terraform {
     required_providers {
       aws = {
         source  = "hashicorp/aws"
         version = "~> 5.0"
       }
     }
   }
   
   provider "aws" {
     region = var.aws_region
   }
   
   module "vpc" {
     source = "../../modules/vpc"
     
     name_prefix           = "VelocityTech-Prod"
     environment          = "production"
     vpc_cidr            = "10.0.0.0/16"
     public_subnet_count = 2
     private_subnet_count = 2
   }
   
   # Production-grade instances
   resource "aws_instance" "prod_servers" {
     count = 3
     
     ami           = data.aws_ami.amazon_linux.id
     instance_type = "t3.medium"
     subnet_id     = module.vpc.public_subnet_ids[count.index % length(module.vpc.public_subnet_ids)]
     
     tags = {
       Name = "VelocityTech-Prod-Server-${count.index + 1}"
       Environment = "production"
     }
   }
   
   data "aws_ami" "amazon_linux" {
     most_recent = true
     owners      = ["amazon"]
     
     filter {
       name   = "name"
       values = ["amzn2-ami-hvm-*-x86_64-gp2"]
     }
   }
   EOF
   ```

3. **Implement Remote State Backend**
   ```bash
   # Create S3 bucket for state storage
   aws s3 mb s3://velocitytech-terraform-state-$(date +%s) --region eu-west-1
   BUCKET_NAME=$(aws s3 ls | grep velocitytech-terraform-state | awk '{print $3}')
   
   # Create DynamoDB table for state locking
   aws dynamodb create-table \
     --table-name terraform-state-lock \
     --attribute-definitions AttributeName=LockID,AttributeType=S \
     --key-schema AttributeName=LockID,KeyType=HASH \
     --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 \
     --region eu-west-1
   
   # Update main configuration to use remote backend
   cat > backend.tf << EOF
   terraform {
     backend "s3" {
       bucket         = "$BUCKET_NAME"
       key            = "velocitytech/terraform.tfstate"
       region         = "eu-west-1"
       encrypt        = true
       dynamodb_table = "terraform-state-lock"
     }
   }
   EOF
   ```

4. **Test Multi-Environment Deployment**
   ```bash
   # Deploy development environment
   cd environments/development
   terraform init
   terraform plan
   terraform apply -auto-approve
   
   # Deploy production environment
   cd ../production
   terraform init
   terraform plan
   terraform apply -auto-approve
   
   # Compare environments
   echo "=== Development Environment ==="
   terraform output
   
   cd ../development
   echo "=== Production Environment ==="
   terraform output
   
   # Verify both environments exist
   aws ec2 describe-instances \
     --filters "Name=tag:Environment,Values=development,production" \
     --query 'Reservations[*].Instances[*].[Tags[?Key==`Name`].Value|[0],State.Name,InstanceType]' \
     --output table
   ```

5. **Create Infrastructure Documentation**
   ```markdown
   # Create README.md
   cat > README.md << 'EOF'
   # VelocityTech Infrastructure as Code
   
   This repository contains Terraform configurations for VelocityTech's AWS infrastructure.
   
   ## Structure
   
   ```
   velocitytech-infrastructure/
   ├── modules/                  # Reusable Terraform modules
   │   ├── vpc/                 # VPC and networking
   │   ├── kubernetes/          # Kubernetes cluster
   │   └── security-groups/     # Security configurations
   ├── environments/            # Environment-specific configs
   │   ├── development/         # Dev environment
   │   ├── staging/            # Staging environment
   │   └── production/         # Production environment
   └── scripts/                # Initialization scripts
   ```
   
   ## Quick Start
   
   1. Configure AWS credentials:
      ```bash
      aws configure
      ```
   
   2. Deploy development environment:
      ```bash
      cd environments/development
      terraform init
      terraform plan
      terraform apply
      ```
   
   3. Deploy production environment:
      ```bash
      cd environments/production
      terraform init
      terraform plan  
      terraform apply
      ```
   
   ## Best Practices
   
   - Always run `terraform plan` before `terraform apply`
   - Use feature branches for infrastructure changes
   - Peer review all infrastructure modifications
   - Tag all resources appropriately
   - Use remote state for team collaboration
   
   ## Environment Comparison
   
   | Resource | Development | Production |
   |----------|-------------|------------|
   | Instance Type | t3.micro | t3.medium |
   | Instance Count | 1 | 3 |
   | Subnets | 1 public, 1 private | 2 public, 2 private |
   | High Availability | No | Yes |
   
   EOF
   ```

**Checkpoint:** Modular infrastructure with multiple environments and remote state management

### Validation & Testing
**How to verify everything works:**
1. **Infrastructure Validation** - `terraform validate` passes for all configurations
2. **Multi-Environment Test** - `Both dev and prod environments deploy successfully`
3. **State Management Test** - `Remote state stored in S3 with DynamoDB locking`
4. **Resource Verification** - `AWS console shows resources with proper tags`
5. **Module Reusability Test** - `VPC module used across multiple environments`

**Expected Results:**
- **Provisioning Speed:** Complete environment in 5-10 minutes vs 2-3 weeks manually
- **Consistency:** Identical configurations across all environments
- **Repeatability:** Can destroy and recreate environments reliably
- **Team Collaboration:** Multiple developers can work on infrastructure safely
- **Documentation:** Clear documentation and version control for all changes

---

## 🔧 New Tools in Your Arsenal

### Terraform Infrastructure Management
- **Purpose:** Define and provision cloud infrastructure using declarative configuration files
- **Key Commands:**
  - `terraform init` - Initialize working directory and download providers
  - `terraform plan` - Preview changes before applying
  - `terraform apply` - Create/modify infrastructure resources
  - `terraform destroy` - Remove all managed infrastructure
- **Pro Tips:** Always use remote state for team projects, pin provider versions
- **Common Pitfalls:** Don't edit resources manually in cloud console, always use Terraform

### AWS Provider for Terraform
- **Purpose:** Manage AWS resources through Terraform with full API coverage
- **Key Resources:**
  - `aws_vpc` - Virtual Private Cloud networking
  - `aws_instance` - EC2 virtual machines
  - `aws_security_group` - Network access controls
  - `aws_s3_bucket` - Object storage for state files
- **Pro Tips:** Use data sources to reference existing resources, implement proper tagging
- **Common Pitfalls:** Watch for resource limits, understand eventual consistency

### Terraform State Management
- **Purpose:** Track actual infrastructure state and enable team collaboration
- **Key Concepts:**
  - Remote backends (S3 + DynamoDB) for team sharing
  - State locking to prevent concurrent changes
  - State versioning for rollback capabilities
- **Pro Tips:** Never edit state files manually, use workspaces for environment isolation
- **Common Pitfalls:** Don't commit state files to Git, always configure backend before team collaboration

---

## 🔧 Common Issues & Solutions

### Issue #1: "Terraform apply fails with resource conflicts"
**Symptoms:** Error messages about resources already existing or dependency conflicts
**Cause:** Resources created manually or state file out of sync with actual infrastructure
**Solution:**
```bash
# Import existing resources into Terraform state
terraform import aws_vpc.main vpc-12345678

# Or refresh state to sync with reality
terraform refresh

# Check for drift between state and actual resources
terraform plan -detailed-exitcode
```
**Prevention:** Always use Terraform for infrastructure changes, avoid manual modifications

### Issue #2: "State file locked, cannot make changes"
**Symptoms:** "Error locking state" messages, unable to run terraform commands
**Cause:** Previous terraform operation interrupted or crashed, leaving lock in place
**Solution:**
```bash
# Check lock status
aws dynamodb get-item \
  --table-name terraform-state-lock \
  --key '{"LockID":{"S":"path/to/your/state"}}'

# Force unlock (use with caution)
terraform force-unlock <lock-id>

# Verify lock is removed
terraform plan
```
**Prevention:** Always let terraform operations complete, use proper CI/CD pipelines

### Issue #3: "Provider version conflicts or deprecated resources"
**Symptoms:** Terraform complains about provider versions or deprecated resource attributes
**Cause:** Using outdated provider versions or legacy resource configurations
**Solution:**
```bash
# Update provider versions
terraform init -upgrade

# Check for deprecated usage
terraform plan

# Update resource configurations based on provider documentation
# Example: Update deprecated attributes
# OLD: availability_zone_names
# NEW: availability_zones
```
**Prevention:** Pin provider versions, regularly update configurations, follow provider migration guides

---

## 🎭 Character Evolution This Week

### Legacy Luka's Infrastructure Awakening
**Monday:** "Infrastructure as Code? Sounds like overengineering. Servers worked fine before programming."
**Tuesday:** "I can version control my server configurations? And see exactly what changed when?"
**Wednesday:** "Rolling back infrastructure changes is just like reverting Git commits. This makes sense!"
**Thursday:** "I can spin up identical development environments for testing new features instantly."
**Friday:** "Infrastructure as Code isn't just about servers - it's about reproducible, testable environments."
**Key Quote:** "Treating infrastructure like software made everything more reliable, not more complicated."

### Anxious Ana's Testing Environment Victory
**Monday:** "Another tool to learn? How many systems do I need to understand to do my job?"
**Tuesday:** "I can create identical test environments on demand? No more waiting for Filip?"
**Wednesday:** "Every test runs in exactly the same environment. My results are finally consistent!"
**Thursday:** "I can test infrastructure changes before they affect production. Revolutionary!"
**Friday:** "Terraform gave me control over my testing environments. I'm not dependent on anyone anymore."
**Key Quote:** "Infrastructure as Code turned environment management from archaeology into engineering."

### Firefighter Filip's Operations Liberation
**Monday:** "Great, now I need to learn programming to do system administration. More complexity."
**Tuesday:** "I can document every server configuration in code. No more mystery configurations!"
**Wednesday:** "Provisioning servers went from weeks to minutes. And they're all identical!"
**Thursday:** "I can test infrastructure changes in development before applying to production."
**Friday:** "My weekends are free again. Automated infrastructure provisioning eliminated emergency setups."
**Key Quote:** "Infrastructure as Code transformed me from a server builder to an infrastructure architect."

### Manager Maja's Business Transformation
**Monday:** "How much will it cost to learn and implement Infrastructure as Code?"
**Tuesday:** "We can provision complete environments for new clients in minutes instead of weeks?"
**Wednesday:** "Our infrastructure costs are predictable and optimized through code-defined resource limits."
**Thursday:** "We can scale up for new projects without hiring more infrastructure staff."
**Friday:** "Infrastructure as Code isn't a cost - it's a competitive advantage that enables rapid business growth."
**Key Quote:** "Terraform turned infrastructure from a business bottleneck into a business accelerator."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 7 Improvements:**

| Metric | Week 6 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 15 days | 12 days | 2 hours |
| Infrastructure Provisioning Time | 2-3 weeks | 10 minutes | <5 minutes |
| Environment Consistency | 100% | 100% | 100% |
| Infrastructure Documentation | 30% | 95% | 100% |
| Manual Server Configuration | 10 minutes/week | 0 minutes/week | 0 minutes/week |
| Environment Setup Errors | Weekly | Zero | Zero |
| New Environment Creation | 3 weeks | 10 minutes | <5 minutes |

**Key Improvements:**
- **Provisioning Speed:** 2,000x faster infrastructure creation (3 weeks → 10 minutes)
- **Documentation Quality:** 95% of infrastructure documented in code
- **Error Elimination:** Zero configuration drift or manual setup errors
- **Team Productivity:** Developers can create environments independently

---

## 🇭🇷 Croatian IT Market Reality
Infrastructure as Code expertise is extremely valuable in the Croatian market. Companies like Infobip, Rimac, and Span are heavily investing in IaC practices to scale their operations. Terraform skills command 30-50% salary premiums over traditional infrastructure roles. Many Croatian companies are still manually provisioning infrastructure, creating high demand for professionals who can implement IaC practices. Your Terraform expertise positions you for senior infrastructure engineer, platform engineer, and cloud architect roles.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about Infrastructure as Code?** How treating infrastructure like software improves reliability and collaboration
2. **Which aspect had the biggest impact on team productivity?** Eliminating manual provisioning bottlenecks
3. **How did IaC change your view of infrastructure management?** From manual processes to automated, repeatable systems
4. **What questions do you still have?** How to manage infrastructure at scale with proper governance and security

**Team Discussion Points:**
- How would each VelocityTech character feel about migrating all infrastructure to Terraform?
- What resistance might we encounter from teams comfortable with manual server management?
- How would you convince skeptical infrastructure teams to adopt Infrastructure as Code practices?

**Preparation for Next Week:**
Ana arrives Monday morning excited about consistent environments but concerned: "The Terraform infrastructure is great, but every server still needs to be configured with specific software, users, and settings. Filip's still logging into each server to install Docker, configure monitoring agents, and set up applications manually. There has to be a way to automate the configuration too!" The configuration management journey begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Terraform: Up & Running" by Yevgeniy Brikman - Comprehensive IaC guide
- "Infrastructure as Code" by Kief Morris - Best practices and patterns

**Hands-on Practice:**
- Implement Terraform for your personal cloud projects
- Practice with Terraform modules and different cloud providers
- Experiment with Terraform testing frameworks (Terratest)

**Industry Insights:**
- "State of Infrastructure as Code" reports - Adoption trends and challenges
- HashiCorp user surveys - Terraform usage patterns and best practices
- Croatian DevOps meetups - Network with local IaC practitioners

**Advanced Topics:**
- Terraform testing and validation strategies
- Multi-cloud infrastructure management
- Terraform Enterprise and governance features
- GitOps workflows for infrastructure

**Croatian Companies Using Terraform:**
- **Infobip:** Multi-cloud infrastructure across 50+ countries
- **Rimac:** Automotive cloud infrastructure and edge computing
- **Span:** Complete web platform infrastructure automation
- **Photomath:** Mobile backend infrastructure at scale
- **Include:** Financial services compliance-focused infrastructure

**Certification Path:**
- **HashiCorp Certified: Terraform Associate** - Fundamental Terraform skills
- **AWS Certified DevOps Engineer** - Cloud-specific infrastructure automation
- **Azure DevOps Engineer Expert** - Multi-cloud infrastructure management

---

*Next up: Week 8 - Configuration Management: "Consistent Server Configuration" →*

> *"Infrastructure as Code doesn't just automate provisioning - it automates possibilities. When infrastructure becomes software, scalability becomes inevitable."* - Infrastructure Philosophy[count.index]
     map_public_ip_on_launch = true
   
     tags = {
       Name = "${var.project_name}-Public-Subnet-${count.index + 1}"
       Type = "public"
     }
   }
   
   # Create private subnets
   resource "aws_subnet" "private_subnets" {
     count = 2
   
     vpc_id            = aws_vpc.velocitytech_vpc.id
     cidr_block        = "10.0.${count.index + 10}.0/24"
     availability_zone = data.aws_availability_zones.available.names[count.index]
   
     tags = {
       Name = "${var.project_name}-Private-Subnet-${count.index + 1}"
       Type = "private"
     }
   }
   
   # Create route table for public subnets
   resource "aws_route_table" "public_rt" {
     vpc_id = aws_vpc.velocitytech_vpc.id
   
     route {
       cidr_block = "0.0.0.0/0"
       gateway_id = aws_internet_gateway.velocitytech_igw.id
     }
   
     tags = {
       Name = "${var.project_name}-Public-RT"
     }
   }
   
   # Associate public subnets with route table
   resource "aws_route_table_association" "public_subnet_associations" {
     count = length(aws_subnet.public_subnets)
   
     subnet_id      = aws_subnet.public_subnets[count.index].id
     route_table_id = aws_route_table.public_rt.id
   }
   ```

4. **Create Variables and Outputs Files**
   ```hcl
   # variables.tf
   variable "aws_region" {
     description = "AWS region for resources"
     type        = string
     default     = "eu-west-1"
   }
   
   variable "project_name" {
     description = "Name of the project"
     type        = string
     default     = "VelocityTech"
   }
   
   variable "environment" {
     description = "Environment name"
     type        = string
     default     = "production"
   }
   
   variable "vpc_cidr" {
     description = "CIDR block for VPC"
     type        = string
     default     = "10.0.0.0/16"
   }
   
   variable "instance_type" {
     description = "EC2 instance type"
     type        = string
     default     = "t3.micro"
   }
   ```

   ```hcl
   # outputs.tf
   output "vpc_id" {
     description = "ID of the VPC"
     value       = aws_vpc.velocitytech_vpc.id
   }
   
   output "public_subnet_ids" {
     description = "IDs of public subnets"
     value       = aws_subnet.public_subnets[*].id
   }
   
   output "private_subnet_ids" {
     description = "IDs of private subnets"
     value       = aws_subnet.private_subnets[*].id
   }
   
   output "internet_gateway_id" {
     description = "ID of the Internet Gateway"
     value       = aws_internet_gateway.velocitytech_igw.id
   }
   ```

5. **Initialize and Apply Basic Infrastructure**
   ```bash
   # Initialize Terraform
   terraform init
   # Expected: Provider plugins downloaded
   
   # Validate configuration
   terraform validate
   # Expected: Success! The configuration is valid.
   
   # Plan infrastructure changes
   terraform plan
   # Review planned changes - should show VPC, subnets, etc.
   
   # Apply infrastructure
   terraform apply
   # Type 'yes' when prompted
   
   # Verify resources were created
   aws ec2 describe-vpcs --filters "Name=tag:Name,Values=VelocityTech-VPC"
   aws ec2 describe-subnets --filters "Name=vpc-id,Values=$(terraform output -raw vpc_id)"
   
   # View Terraform outputs
   terraform output
   ```

**Checkpoint:** Basic AWS VPC infrastructure created and managed by Terraform

### Phase 2: Security Groups and EC2 Instances (75 minutes)
**Objective:** Add security groups and compute resources for Kubernetes cluster

**Steps:**
1. **Create Security Groups**
   ```hcl
   # Add to main.tf
   
   # Security group for Kubernetes master nodes
   resource "aws_security_group" "k8s_master_sg" {
     name_prefix = "${var.project_name}-k8s-master"
     vpc_id      = aws_vpc.velocitytech_vpc.id
   
     # SSH access
     ingress {
       from_port   = 22
       to_port     = 22
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     # Kubernetes API server
     ingress {
       from_port   = 6443
       to_port     = 6443
       protocol    = "tcp"
       cidr_blocks = [var.vpc_cidr]
     }
   
     # etcd server client API
     ingress {
       from_port   = 2379
       to_port     = 2380
       protocol    = "tcp"
       cidr_blocks = [var.vpc_cidr]
     }
   
     # Kubelet API
     ingress {
       from_port   = 10250
       to_port     = 10250
       protocol    = "tcp"
       cidr_blocks = [var.vpc_cidr]
     }
   
     # All outbound traffic
     egress {
       from_port   = 0
       to_port     = 0
       protocol    = "-1"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     tags = {
       Name = "${var.project_name}-k8s-master-sg"
     }
   }
   
   # Security group for Kubernetes worker nodes
   resource "aws_security_group" "k8s_worker_sg" {
     name_prefix = "${var.project_name}-k8s-worker"
     vpc_id      = aws_vpc.velocitytech_vpc.id
   
     # SSH access
     ingress {
       from_port   = 22
       to_port     = 22
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     # Kubelet API
     ingress {
       from_port   = 10250
       to_port     = 10250
       protocol    = "tcp"
       cidr_blocks = [var.vpc_cidr]
     }
   
     # NodePort services
     ingress {
       from_port   = 30000
       to_port     = 32767
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     # All outbound traffic
     egress {
       from_port   = 0
       to_port     = 0
       protocol    = "-1"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     tags = {
       Name = "${var.project_name}-k8s-worker-sg"
     }
   }
   
   # Security group for application load balancer
   resource "aws_security_group" "alb_sg" {
     name_prefix = "${var.project_name}-alb"
     vpc_id      = aws_vpc.velocitytech_vpc.id
   
     # HTTP access
     ingress {
       from_port   = 80
       to_port     = 80
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     # HTTPS access
     ingress {
       from_port   = 443
       to_port     = 443
       protocol    = "tcp"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     # All outbound traffic
     egress {
       from_port   = 0
       to_port     = 0
       protocol    = "-1"
       cidr_blocks = ["0.0.0.0/0"]
     }
   
     tags = {
       Name = "${var.project_name}-alb-sg"
     }
   }
   ```

2. **Create Key Pair for SSH Access**
   ```bash
   # Generate SSH key pair
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/velocitytech-key -N ""
   
   # Add public key to AWS
   aws ec2 import-key-pair \
     --key-name "velocitytech-key" \
     --public-key-material fileb://~/.ssh/velocitytech-key.pub
   ```

3. **Create EC2 Instances for Kubernetes**
   ```hcl
   # Add to main.tf
   
   # Data source for latest Amazon Linux AMI
   data "aws_ami" "amazon_linux" {
     most_recent = true
     owners      = ["amazon"]
   
     filter {
       name   = "name"
       values = ["amzn2-ami-hvm-*-x86_64-gp2"]
     }
   }
   
   # Kubernetes master node
   resource "aws_instance" "k8s_master" {
     ami                    = data.aws_ami.amazon_linux.id
     instance_type          = var.instance_type
     key_name               = "velocitytech-key"
     subnet_id              = aws_subnet.public_subnets[0].id
     vpc_security_group_ids = [aws_security_group.k8s_master_sg.id]
   
     user_data = base64encode(templatefile("${path.module}/scripts/k8s-master-init.sh", {
       cluster_name = "${var.project_name}-cluster"
     }))
   
     tags = {
       Name = "${var.project_name}-k8s-master"
       Type = "kubernetes-master"
     }
   }
   
   # Kubernetes worker nodes
   resource "aws_instance" "k8s_workers" {
     count = 2
   
     ami                    = data.aws_ami.amazon_linux.id
     instance_type          = var.instance_type
     key_name               = "velocitytech-key"
     subnet_id              = aws_subnet.public_subnets[count.index % length(aws_subnet.public_subnets)].id
     vpc_security_group_ids = [aws_security_group.k8s_worker_sg.id]
   
     user_data = base64encode(templatefile("${path.module}/scripts/k8s-worker-init.sh", {
       master_ip = aws_instance.k8s_master.private_ip
     }))
   
     tags = {
       Name = "${var.project_name}-k8s-worker-${count.index + 1}"
       Type = "kubernetes-worker"
     }
   
     depends_on = [aws_instance.k8s_master]
   }
   ```

4. **Create Initialization Scripts**
   ```bash
   # Create scripts directory
   mkdir scripts
   
   # scripts/k8s-master-init.sh
   cat > scripts/k8s-master-init.sh << 'EOF'
   #!/bin/bash
   
   # Update system
   yum update -y
   
   # Install Docker
   amazon-linux-extras install docker
   systemctl start docker
   systemctl enable docker
   usermod -a -G docker ec2-user
   
   # Install kubectl
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x kubectl
   mv kubectl /usr/local/bin/
   
   # Install kubeadm and kubelet
   cat <<EOL > /etc/yum.repos.d/kubernetes.repo
   [kubernetes]
   name=Kubernetes
   baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
   enabled=1
   gpgcheck=1
   repo_gpgcheck=1
   gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
   EOL
   
   yum install -y kubelet kubeadm
   systemctl enable kubelet
   
   # Initialize Kubernetes cluster
   kubeadm init --pod-network-cidr=192.168.0.0/16 --cluster-name=${cluster_name}
   
   # Set up kubectl for ec2-user
   mkdir -p /home/ec2-user/.kube
   cp /etc/kubernetes/admin.conf /home/ec2-user/.kube/config
   chown ec2-user:ec2-user /home/ec2-user/.kube/config
   
   # Install Calico network plugin
   kubectl --kubeconfig=/home/ec2-user/.kube/config apply -f https://docs.projectcalico.org/manifests/calico.yaml
   
   # Save join command for worker nodes
   kubeadm token create --print-join-command > /home/ec2-user/join-command.sh
   chmod +x /home/ec2-user/join-command.sh
   EOF
   
   # scripts/k8s-worker-init.sh
   cat > scripts/k8s-worker-init.sh << 'EOF'
   #!/bin/bash
   
   # Update system
   yum update -y
   
   # Install Docker
   amazon-linux-extras install docker
   systemctl start docker
   systemctl enable docker
   usermod -a -G docker ec2-user
   
   # Install kubeadm and kubelet
   cat <<EOL > /etc/yum.repos.d/kubernetes.repo
   [kubernetes]
   name=Kubernetes
   baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
   enabled=1
   gpgcheck=1
   repo_gpgcheck=1
   gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
   EOL
   
   yum install -y kubelet kubeadm
   systemctl enable kubelet
   
   # Note: Join command will be executed manually after master is ready
   echo "#!/bin/bash" > /home/ec2-user/join-cluster.sh
   echo "# Run the join command from master node here" >> /home/ec2-user/join-cluster.sh
   chmod +x /home/ec2-user/join-cluster.sh
   EOF
   ```

5. **Apply Infrastructure with EC2 Instances**
   ```bash
   # Plan and apply changes
   terraform plan
   terraform apply
   
   # Wait for instances to initialize (5-10 minutes)
   echo "Waiting for instances to initialize..."
   sleep 300
   
   # Get instance information
   terraform output
   
   # Connect to master node to verify Kubernetes
   MASTER_IP=$(terraform output -raw k8s_master_public_ip)
   ssh -i ~/.ssh/velocitytech-key ec2-user@$MASTER_IP "kubectl get nodes"
   
   # Expected: Master node in Ready state
   ```

**Checkpoint:** Complete Kubernetes infrastructure provisioned with Terraform

### Phase 3: Infrastructure Modules and Environments (45 minutes)
**Objective:** Create reusable Terraform modules for different environments

**Steps:**
1. **Create Modular Infrastructure Structure**
   ```bash
   # Reorganize into modules
   mkdir -p modules/vpc modules/kubernetes modules/security-groups
   
   # Move VPC resources to module
   cat > modules/vpc/main.tf << 'EOF'
   # VPC Module
   resource "aws_vpc" "main" {
     cidr_block           = var.vpc_cidr
     enable_dns_hostnames = true
     enable_dns_support   = true
   
     tags = {
       Name        = "${var.name_prefix}-VPC"
       Environment = var.environment
     }
   }
   
   resource "aws_internet_gateway" "main" {
     vpc_id = aws_vpc.main.id
   
     tags = {
       Name = "${var.name_prefix}-IGW"
     }
   }
   
   resource "aws_subnet" "public" {
     count = var.public_subnet_count
   
     vpc_id                  = aws_vpc.main.id
     cidr_block              = "10.0.${count.index + 1}.0/24"
     availability_zone       = data.aws_availability_zones.available.names

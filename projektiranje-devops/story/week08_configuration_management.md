# Week 8: Configuration Management - "Consistent Server Configuration" 🔧
**VelocityTech Transformation Week 8**

**Story Context:** Terraform provisions servers quickly, but each still needs manual configuration with software, users, and applications  
**Focus:** Implementing Configuration Management with Ansible for consistent, automated server setup  
**Characters in Focus:** Filip's post-provisioning configuration chaos, Ana's software version inconsistencies, Luka's deployment environment drift

---

## 🔥 The Weekly Crisis: "The Great Configuration Drift Disaster"
**Setting:** VelocityTech office, Monday 10:30 AM. Three identical Terraform-provisioned servers, three completely different configurations.  
**The Problem:** Servers are provisioned consistently, but manual software installation creates unique snowflakes again

**Scene:** [VelocityTech main floor. Filip jumping between three terminals, each showing a different server. Ana comparing software versions across environments with growing frustration. Luka debugging why his application behaves differently on each server.]

**FILIP** *(frantically switching between server terminals)*  
> "Server 1 has Docker 20.10.8, Server 2 has 20.10.12, Server 3 has 20.10.21..."  
> "I installed them on different days, so they got different versions!"  
> *(checking his notes)* "Server 1 has Python 3.8, Server 2 has 3.9, Server 3... I can't remember what I installed!"

**ANA** *(running version checks across servers)*  
> "The payment gateway behaves differently on each server because of library differences!"  
> "Server 1 fails SSL certificate validation, Server 2 has database connection timeouts..."  
> *(frustrated)* "Server 3 works perfectly, but I have no idea what's different about it!"

**LUKA** *(debugging application startup errors)*  
> "My Node.js app requires specific npm packages, but each server has different global packages installed..."  
> "On Server 1, I'm missing 'pm2', on Server 2, 'nodemon' is the wrong version..."  
> *(confused)* "How did three identical servers become three different environments?"

**MAJA** *(checking deployment schedule)*  
> "We have five more servers to configure for the new client by Thursday..."  
> "At this rate, Filip will need to manually configure each one differently, and we'll have more inconsistencies!"

**FILIP** *(pulling out a worn notebook)*  
> "I have my server configuration checklist, but it's evolved over time..."  
> "Version 1.0 from January, 2.1 from March, 3.2 from last month..."  
> *(overwhelmed)* "Each server was configured with a different version of my procedures!"

**ANA** *(testing application deployments)*  
> "I can't certify the application when it behaves differently on every server!"  
> "What if the production behavior depends on which specific configuration drift we have?"

**LUKA** *(looking at monitoring dashboards)*  
> "Server 3 uses 30% less memory than the others for the same workload..."  
> "But I can't figure out what configuration difference is causing the efficiency!"

**MARKO** *(entering, recognizing the pattern immediately)*  
> "Let me guess - Terraform gave you identical servers, but manual configuration made them unique again?"  
> *(to you)* "This is exactly why Configuration Management exists. Ready to make software installation as consistent as server provisioning?"

**FILIP** *(hopeful but concerned)*  
> "I've heard about Ansible, but isn't that just another complex tool to maintain?"  
> "Will I need to learn YAML on top of everything else?"

**MAJA** *(calculating the impact)*  
> "If we can't configure servers consistently, we'll need separate testing for each server configuration..."  
> "That multiplies our testing complexity by the number of servers!"

**[All eyes turn to you - the intern who might understand modern configuration practices.]**

**ANA** *(pleading)*  
> "Please tell me there's a way to ensure every server has exactly the same software configuration!"  
> "Something that's automated and repeatable?"

**[Your systems thinking activates. This isn't just about software installation - it's about configuration drift prevention and infrastructure consistency.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Automate server configuration with Ansible** - Write playbooks that consistently configure servers with required software and settings
2. **Implement idempotent configuration management** - Ensure configurations can be applied multiple times safely without side effects
3. **Design role-based configuration architecture** - Create reusable configuration components that can be shared across projects

**Real-world Connection:** Configuration Management skills are essential for maintaining consistent infrastructure at scale. Croatian companies like Infobip, Rimac, and Span use Ansible extensively to manage thousands of servers. Configuration automation expertise is highly valued and often required for senior DevOps positions.

---

## 📚 The Knowledge Foundation

### Configuration Management Principles and the Problem They Solve
**The Problem it Solves:** Configuration drift, manual installation errors, inconsistent environments, time-consuming server setup, undocumented changes  
**How it Works:** Automated tools ensure servers maintain desired configuration state through declarative instructions

**Manual vs Automated Configuration:**
```
Manual Configuration:           Automated Configuration:
┌─────────────────────────┐    ┌─────────────────────────┐
│ SSH to each server      │    │ Run playbook once       │
│ Run commands manually   │    │ Configure all servers   │
│ Hope nothing breaks     │    │ Idempotent operations   │
│ No documentation        │    │ Version controlled      │
│ Configuration drift     │    │ Consistent results      │
│ Hours per server        │    │ Minutes for all servers │
└─────────────────────────┘    └─────────────────────────┘
```

**Core Configuration Management Principles:**
- **Idempotency:** Running configuration multiple times produces same result
- **Declarative:** Describe desired state, not steps to achieve it
- **Convergence:** Automatically fix configuration drift
- **Documentation:** Configuration is self-documenting code
- **Scalability:** Configure hundreds of servers as easily as one
- **Testability:** Validate configurations before applying

**Configuration Management Benefits:**
- **Consistency:** Eliminate configuration drift across servers
- **Speed:** Configure multiple servers simultaneously
- **Reliability:** Reduce human error through automation
- **Auditability:** Track all configuration changes in version control
- **Disaster Recovery:** Rebuild servers from known configurations
- **Compliance:** Ensure security and policy requirements

**Real-world Example:** Facebook uses configuration management to maintain consistent configurations across 100,000+ servers globally  
**VelocityTech Connection:** Automated configuration would eliminate Filip's manual setup variations and give Ana consistent test environments

### Ansible Fundamentals and Architecture
**The Problem it Solves:** Complex configuration management, agent-based overhead, difficult multi-server orchestration  
**How it Works:** Ansible uses agentless SSH connections to configure servers through YAML playbooks

**Ansible Core Components:**
- **Inventory:** Lists of servers to configure
- **Playbooks:** YAML files describing configurations
- **Tasks:** Individual configuration steps
- **Modules:** Built-in functions for common operations
- **Roles:** Reusable configuration collections
- **Handlers:** Actions triggered by configuration changes

**Ansible Architecture:**
```
Control Node (Your Machine)
├── Inventory (server list)
├── Playbooks (configuration)
└── SSH → Managed Nodes
            ├── Server 1
            ├── Server 2
            └── Server 3
```

**Basic Ansible Concepts:**
```yaml
# Simple playbook example
- name: Configure web servers
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: present
    
    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

**Ansible Advantages:**
- **Agentless:** No software installation on managed servers
- **Simple:** YAML-based configuration language
- **Powerful:** Extensive module library for common tasks
- **Flexible:** Can manage any system accessible via SSH
- **Parallel:** Configure multiple servers simultaneously

**Real-world Example:** NASA uses Ansible to manage complex spacecraft ground systems with zero-downtime requirements  
**VelocityTech Connection:** Ansible would automate Filip's server configuration procedures and eliminate version inconsistencies

### Advanced Ansible: Playbooks, Roles, and Best Practices
**The Problem it Solves:** Complex configuration organization, code reuse, team collaboration on infrastructure automation  
**How it Works:** Structured approach using roles, variables, and templates for maintainable configuration code

**Ansible Playbook Structure:**
```yaml
# site.yml - Main playbook
- name: Configure VelocityTech Infrastructure
  hosts: all
  become: yes
  vars:
    app_name: payment-gateway
    app_version: "1.2.0"
  
  roles:
    - common
    - docker
    - monitoring
    - payment-gateway

# Inventory organization
[webservers]
web1.velocitytech.com
web2.velocitytech.com

[databases]
db1.velocitytech.com

[all:vars]
ansible_user=ec2-user
environment=production
```

**Ansible Roles Architecture:**
```
roles/
├── common/
│   ├── tasks/main.yml      # Main task list
│   ├── handlers/main.yml   # Event handlers
│   ├── templates/          # Jinja2 templates
│   ├── files/             # Static files
│   ├── vars/main.yml      # Role variables
│   └── defaults/main.yml  # Default variables
├── docker/
│   ├── tasks/main.yml
│   └── templates/daemon.json.j2
└── payment-gateway/
    ├── tasks/main.yml
    └── templates/config.yml.j2
```

**Advanced Ansible Features:**
```yaml
# Dynamic inventories
- name: Get EC2 instances
  amazon.aws.ec2_info:
    filters:
      tag:Environment: production
      instance-state-name: running
  register: ec2_instances

# Conditional execution
- name: Install Docker on CentOS
  yum:
    name: docker
    state: present
  when: ansible_distribution == "CentOS"

# Loops and iteration
- name: Create user accounts
  user:
    name: "{{ item.name }}"
    groups: "{{ item.groups }}"
  loop:
    - { name: alice, groups: wheel }
    - { name: bob, groups: users }

# Vault for secrets
- name: Configure database
  template:
    src: database.conf.j2
    dest: /etc/app/database.conf
  vars:
    db_password: "{{ vault_db_password }}"
```

**Configuration Management Best Practices:**
- **Role Design:** Create small, focused, reusable roles
- **Variable Management:** Use host_vars, group_vars, and defaults appropriately
- **Secret Management:** Use Ansible Vault for sensitive data
- **Testing:** Validate playbooks with ansible-lint and molecule
- **Documentation:** Comment complex tasks and role purposes
- **Version Control:** Track all configuration changes in Git

**Real-world Example:** Netflix uses Ansible roles to maintain consistent configurations across thousands of microservice deployments  
**VelocityTech Connection:** Well-structured roles would allow the team to share configuration components and maintain consistency

**Key Takeaways:**
- Configuration management eliminates drift through automated consistency
- Ansible provides agentless, YAML-based server configuration
- Roles enable reusable, maintainable configuration architecture
- Idempotent operations ensure safe repeated execution

---

## 🛠️ Lab Mission: "The Configuration Consistency Revolution"
**Scenario:** VelocityTech's Terraform-provisioned servers need consistent software configuration across environments  
**Your Mission:** Create Ansible playbooks that automatically configure servers with identical software stacks  
**Success Criteria:** All servers have identical software versions, configurations, and application deployments

### Phase 1: Ansible Setup and Basic Server Configuration (60 minutes)
**Objective:** Install Ansible and create basic playbooks for server standardization

**Steps:**
1. **Ansible Installation and Setup**
   ```bash
   # Install Ansible
   sudo apt update && sudo apt install -y software-properties-common
   sudo add-apt-repository --yes --update ppa:ansible/ansible
   sudo apt install -y ansible
   
   # Verify installation
   ansible --version
   # Expected: ansible [core 2.15.x]
   
   # Create VelocityTech configuration project
   mkdir velocitytech-config
   cd velocitytech-config
   
   # Initialize Git repository
   git init
   echo "*.retry" >> .gitignore
   echo "host_vars/*.yml" >> .gitignore
   echo "group_vars/all/vault.yml" >> .gitignore
   
   # Create basic directory structure
   mkdir -p {inventories,playbooks,roles,group_vars,host_vars}
   ```
   - Expected output: Ansible installed and project structure created
   - Troubleshooting: If ansible installation fails, try using pip: `pip3 install ansible`

2. **Create Inventory and Basic Server Access**
   ```bash
   # Create inventory file for VelocityTech servers
   cat > inventories/production << 'EOF'
   [webservers]
   web1 ansible_host=<YOUR_EC2_IP_1> ansible_user=ec2-user
   web2 ansible_host=<YOUR_EC2_IP_2> ansible_user=ec2-user
   web3 ansible_host=<YOUR_EC2_IP_3> ansible_user=ec2-user
   
   [databases]
   db1 ansible_host=<YOUR_DB_IP> ansible_user=ec2-user
   
   [all:vars]
   ansible_ssh_private_key_file=~/.ssh/velocitytech-key
   ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   EOF
   
   # Replace <YOUR_EC2_IP_X> with actual IPs from Terraform output
   # Get IPs from previous week's Terraform deployment
   cd ../velocitytech-infrastructure
   terraform output | grep public_ip
   cd ../velocitytech-config
   
   # Test connectivity to all servers
   ansible all -i inventories/production -m ping
   # Expected: All servers respond with "pong"
   
   # Gather server facts
   ansible all -i inventories/production -m setup | head -20
   ```

3. **Create Basic System Configuration Playbook**
   ```yaml
   # playbooks/site.yml
   - name: Configure VelocityTech Infrastructure
     hosts: all
     become: yes
     gather_facts: yes
     
     vars:
       timezone: "Europe/Zagreb"
       ntp_servers:
         - "0.hr.pool.ntp.org"
         - "1.hr.pool.ntp.org"
       
     tasks:
       - name: Update system packages
         yum:
           name: '*'
           state: latest
         register: package_update
         
       - name: Install essential packages
         yum:
           name:
             - git
             - curl
             - wget
             - unzip
             - vim
             - htop
             - tree
           state: present
           
       - name: Set timezone
         timezone:
           name: "{{ timezone }}"
           
       - name: Configure NTP
         template:
           src: ntp.conf.j2
           dest: /etc/ntp.conf
           backup: yes
         notify: restart ntp
         
       - name: Create velocitytech user
         user:
           name: velocitytech
           groups: wheel
           shell: /bin/bash
           create_home: yes
           
       - name: Configure sudo for velocitytech user
         lineinfile:
           path: /etc/sudoers.d/velocitytech
           line: 'velocitytech ALL=(ALL) NOPASSWD:ALL'
           create: yes
           mode: '0440'
           
     handlers:
       - name: restart ntp
         service:
           name: ntpd
           state: restarted
   ```

4. **Create Configuration Templates**
   ```bash
   # Create templates directory
   mkdir -p playbooks/templates
   
   # NTP configuration template
   cat > playbooks/templates/ntp.conf.j2 << 'EOF'
   # NTP configuration for VelocityTech servers
   # Generated by Ansible
   
   driftfile /var/lib/ntp/drift
   
   # NTP servers for Croatia
   {% for server in ntp_servers %}
   server {{ server }} iburst
   {% endfor %}
   
   # Fallback to local clock
   server 127.127.1.0
   fudge 127.127.1.0 stratum 10
   
   # Access control
   restrict default nomodify notrap nopeer noquery
   restrict 127.0.0.1
   restrict ::1
   
   # Log configuration
   logfile /var/log/ntp.log
   EOF
   ```

5. **Run Basic Configuration Playbook**
   ```bash
   # Check playbook syntax
   ansible-playbook -i inventories/production playbooks/site.yml --syntax-check
   # Expected: playbook syntax is OK
   
   # Run in check mode (dry run)
   ansible-playbook -i inventories/production playbooks/site.yml --check
   
   # Apply configuration to all servers
   ansible-playbook -i inventories/production playbooks/site.yml
   
   # Verify configuration consistency
   ansible all -i inventories/production -m command -a "timedatectl status"
   ansible all -i inventories/production -m command -a "whoami" --become --become-user=velocitytech
   
   # Check package versions for consistency
   ansible all -i inventories/production -m command -a "rpm -qa | grep -E '(git|curl|vim)' | sort"
   ```

**Checkpoint:** All servers have consistent basic configuration and system packages

### Phase 2: Docker and Application Stack Configuration (75 minutes)
**Objective:** Create roles for Docker installation and application deployment

**Steps:**
1. **Create Docker Installation Role**
   ```bash
   # Create Docker role structure
   mkdir -p roles/docker/{tasks,templates,defaults,handlers}
   
   # Docker role tasks
   cat > roles/docker/tasks/main.yml << 'EOF'
   ---
   - name: Remove old Docker versions
     yum:
       name:
         - docker
         - docker-client
         - docker-client-latest
         - docker-common
         - docker-latest
         - docker-latest-logrotate
         - docker-logrotate
         - docker-engine
       state: absent
   
   - name: Install Docker repository
     yum_repository:
       name: docker-ce-stable
       description: Docker CE Stable - $basearch
       baseurl: https://download.docker.com/linux/centos/7/$basearch/stable
       gpgcheck: yes
       gpgkey: https://download.docker.com/linux/centos/gpg
       enabled: yes
   
   - name: Install Docker CE
     yum:
       name:
         - docker-ce-{{ docker_version }}
         - docker-ce-cli-{{ docker_version }}
         - containerd.io
       state: present
   
   - name: Configure Docker daemon
     template:
       src: daemon.json.j2
       dest: /etc/docker/daemon.json
       mode: '0644'
     notify: restart docker
   
   - name: Create docker group
     group:
       name: docker
       state: present
   
   - name: Add users to docker group
     user:
       name: "{{ item }}"
       groups: docker
       append: yes
     loop: "{{ docker_users }}"
   
   - name: Start and enable Docker service
     systemd:
       name: docker
       state: started
       enabled: yes
   
   - name: Install Docker Compose
     get_url:
       url: "https://github.com/docker/compose/releases/download/{{ docker_compose_version }}/docker-compose-Linux-x86_64"
       dest: /usr/local/bin/docker-compose
       mode: '0755'
   
   - name: Verify Docker installation
     command: docker --version
     register: docker_version_output
     changed_when: false
   
   - name: Display Docker version
     debug:
       msg: "Docker installed: {{ docker_version_output.stdout }}"
   EOF
   
   # Docker role defaults
   cat > roles/docker/defaults/main.yml << 'EOF'
   ---
   docker_version: "24.0.7-1.el7"
   docker_compose_version: "v2.21.0"
   docker_users:
     - ec2-user
     - velocitytech
   
   docker_daemon_config:
     log-driver: "json-file"
     log-opts:
       max-size: "10m"
       max-file: "3"
     storage-driver: "overlay2"
   EOF
   
   # Docker daemon template
   cat > roles/docker/templates/daemon.json.j2 << 'EOF'
   {
   {% for key, value in docker_daemon_config.items() %}
     "{{ key }}": {% if value is mapping %}{{ value | to_nice_json }}{% else %}"{{ value }}"{% endif %}{% if not loop.last %},{% endif %}
   {% endfor %}
   }
   EOF
   
   # Docker role handlers
   cat > roles/docker/handlers/main.yml << 'EOF'
   ---
   - name: restart docker
     systemd:
       name: docker
       state: restarted
       daemon_reload: yes
   EOF
   ```

2. **Create Payment Gateway Application Role**
   ```bash
   # Create payment gateway role structure
   mkdir -p roles/payment-gateway/{tasks,templates,files,defaults}
   
   # Payment gateway role tasks
   cat > roles/payment-gateway/tasks/main.yml << 'EOF'
   ---
   - name: Create application directory
     file:
       path: "{{ app_directory }}"
       state: directory
       owner: "{{ app_user }}"
       group: "{{ app_group }}"
       mode: '0755'
   
   - name: Create configuration directory
     file:
       path: "{{ app_directory }}/config"
       state: directory
       owner: "{{ app_user }}"
       group: "{{ app_group }}"
       mode: '0755'
   
   - name: Generate application configuration
     template:
       src: app-config.yml.j2
       dest: "{{ app_directory }}/config/production.yml"
       owner: "{{ app_user }}"
       group: "{{ app_group }}"
       mode: '0644'
     notify: restart payment gateway
   
   - name: Create Docker Compose file
     template:
       src: docker-compose.yml.j2
       dest: "{{ app_directory }}/docker-compose.yml"
       owner: "{{ app_user }}"
       group: "{{ app_group }}"
       mode: '0644'
     notify: restart payment gateway
   
   - name: Create systemd service file
     template:
       src: payment-gateway.service.j2
       dest: /etc/systemd/system/payment-gateway.service
       mode: '0644'
     notify:
       - reload systemd
       - restart payment gateway
   
   - name: Pull Docker images
     docker_image:
       name: "{{ item }}"
       source: pull
     loop:
       - "{{ app_image }}"
       - "postgres:14-alpine"
       - "redis:7-alpine"
   
   - name: Start payment gateway service
     systemd:
       name: payment-gateway
       state: started
       enabled: yes
       daemon_reload: yes
   EOF
   
   # Payment gateway defaults
   cat > roles/payment-gateway/defaults/main.yml << 'EOF'
   ---
   app_name: payment-gateway
   app_version: "1.2.0"
   app_image: "velocitytech/payment-gateway:{{ app_version }}"
   app_directory: "/opt/{{ app_name }}"
   app_user: velocitytech
   app_group: velocitytech
   app_port: 3000
   
   database:
     host: postgres
     port: 5432
     name: payment_prod
     user: prod_user
     password: "{{ vault_database_password }}"
   
   redis:
     host: redis
     port: 6379
   EOF
   
   # Application configuration template
   cat > roles/payment-gateway/templates/app-config.yml.j2 << 'EOF'
   # VelocityTech Payment Gateway Configuration
   # Generated by Ansible
   
   server:
     port: {{ app_port }}
     host: 0.0.0.0
   
   database:
     host: {{ database.host }}
     port: {{ database.port }}
     database: {{ database.name }}
     username: {{ database.user }}
     password: {{ database.password }}
   
   redis:
     host: {{ redis.host }}
     port: {{ redis.port }}
   
   logging:
     level: info
     format: json
   
   security:
     cors_enabled: true
     rate_limiting: true
   EOF
   
   # Docker Compose template
   cat > roles/payment-gateway/templates/docker-compose.yml.j2 << 'EOF'
   version: '3.8'
   
   services:
     postgres:
       image: postgres:14-alpine
       environment:
         POSTGRES_DB: {{ database.name }}
         POSTGRES_USER: {{ database.user }}
         POSTGRES_PASSWORD: {{ database.password }}
       volumes:
         - postgres_data:/var/lib/postgresql/data
       networks:
         - app_network
   
     redis:
       image: redis:7-alpine
       volumes:
         - redis_data:/data
       networks:
         - app_network
   
     payment-gateway:
       image: {{ app_image }}
       ports:
         - "{{ app_port }}:{{ app_port }}"
       environment:
         NODE_ENV: production
         CONFIG_FILE: /app/config/production.yml
       volumes:
         - ./config:/app/config:ro
       depends_on:
         - postgres
         - redis
       networks:
         - app_network
   
   volumes:
     postgres_data:
     redis_data:
   
   networks:
     app_network:
       driver: bridge
   EOF
   
   # Systemd service template
   cat > roles/payment-gateway/templates/payment-gateway.service.j2 << 'EOF'
   [Unit]
   Description=VelocityTech Payment Gateway
   Requires=docker.service
   After=docker.service
   
   [Service]
   Type=oneshot
   RemainAfterExit=true
   WorkingDirectory={{ app_directory }}
   ExecStart=/usr/local/bin/docker-compose up -d
   ExecStop=/usr/local/bin/docker-compose down
   TimeoutStartSec=0
   
   [Install]
   WantedBy=multi-user.target
   EOF
   
   # Payment gateway handlers
   cat > roles/payment-gateway/handlers/main.yml << 'EOF'
   ---
   - name: reload systemd
     systemd:
       daemon_reload: yes
   
   - name: restart payment gateway
     systemd:
       name: payment-gateway
       state: restarted
   EOF
   ```

3. **Create Monitoring Role**
   ```bash
   # Create monitoring role structure
   mkdir -p roles/monitoring/{tasks,templates,defaults}
   
   # Monitoring role tasks
   cat > roles/monitoring/tasks/main.yml << 'EOF'
   ---
   - name: Install monitoring tools
     yum:
       name:
         - htop
         - iotop
         - nethogs
         - tcpdump
         - sysstat
       state: present
   
   - name: Create monitoring user
     user:
       name: monitoring
       system: yes
       shell: /bin/false
       home: /var/lib/monitoring
       create_home: yes
   
   - name: Install Node Exporter
     get_url:
       url: "https://github.com/prometheus/node_exporter/releases/download/v{{ node_exporter_version }}/node_exporter-{{ node_exporter_version }}.linux-amd64.tar.gz"
       dest: /tmp/node_exporter.tar.gz
       mode: '0644'
   
   - name: Extract Node Exporter
     unarchive:
       src: /tmp/node_exporter.tar.gz
       dest: /tmp
       remote_src: yes
   
   - name: Install Node Exporter binary
     copy:
       src: "/tmp/node_exporter-{{ node_exporter_version }}.linux-amd64/node_exporter"
       dest: /usr/local/bin/node_exporter
       mode: '0755'
       owner: root
       group: root
       remote_src: yes
   
   - name: Create Node Exporter systemd service
     template:
       src: node_exporter.service.j2
       dest: /etc/systemd/system/node_exporter.service
       mode: '0644'
     notify:
       - reload systemd
       - restart node exporter
   
   - name: Start and enable Node Exporter
     systemd:
       name: node_exporter
       state: started
       enabled: yes
       daemon_reload: yes
   EOF
   
   # Monitoring defaults
   cat > roles/monitoring/defaults/main.yml << 'EOF'
   ---
   node_exporter_version: "1.6.1"
   node_exporter_port: 9100
   EOF
   
   # Node Exporter service template
   cat > roles/monitoring/templates/node_exporter.service.j2 << 'EOF'
   [Unit]
   Description=Node Exporter
   After=network.target
   
   [Service]
   Type=simple
   User=monitoring
   Group=monitoring
   ExecStart=/usr/local/bin/node_exporter --web.listen-address=:{{ node_exporter_port }}
   SyslogIdentifier=node_exporter
   Restart=always
   
   [Install]
   WantedBy=multi-user.target
   EOF
   
   # Monitoring handlers
   cat > roles/monitoring/handlers/main.yml << 'EOF'
   ---
   - name: reload systemd
     systemd:
       daemon_reload: yes
   
   - name: restart node exporter
     systemd:
       name: node_exporter
       state: restarted
   EOF
   ```

4. **Create Main Playbook with Roles**
   ```yaml
   # playbooks/deploy.yml
   - name: Deploy VelocityTech Application Stack
     hosts: webservers
     become: yes
     gather_facts: yes
     
     vars:
       vault_database_password: "{{ vault_db_password }}"
     
     pre_tasks:
       - name: Update package cache
         yum:
           update_cache: yes
     
     roles:
       - docker
       - monitoring
       - payment-gateway
     
     post_tasks:
       - name: Verify application is running
         uri:
           url: "http://localhost:{{ app_port }}/health"
           status_code: 200
         register: health_check
         retries: 5
         delay: 10
       
       - name: Display health check result
         debug:
           msg: "Application health check: {{ health_check.json }}"
   ```

5. **Configure Secrets with Ansible Vault**
   ```bash
   # Create vault file for secrets
   ansible-vault create group_vars/all/vault.yml
   # Enter vault password when prompted
   # Add to vault file:
   # vault_db_password: super_secure_password_123
   
   # Deploy complete application stack
   ansible-playbook -i inventories/production playbooks/deploy.yml --ask-vault-pass
   
   # Verify deployment across all servers
   ansible webservers -i inventories/production -m uri -a "url=http://localhost:3000/health"
   
   # Check Docker containers are running consistently
   ansible webservers -i inventories/production -m command -a "docker ps --format 'table {{.Names}}\t{{.Status}}'"
   
   # Verify software versions are identical
   ansible webservers -i inventories/production -m command -a "docker --version"
   ansible webservers -i inventories/production -m command -a "/usr/local/bin/docker-compose --version"
   ```

**Checkpoint:** Complete application stack deployed consistently across all servers with monitoring

### Phase 3: Configuration Drift Detection and Remediation (45 minutes)
**Objective:** Implement systems to detect and automatically fix configuration drift

**Steps:**
1. **Create Configuration Validation Playbook**
   ```yaml
   # playbooks/validate.yml
   - name: Validate VelocityTech Server Configurations
     hosts: all
     become: yes
     gather_facts: yes
     
     vars:
       expected_packages:
         - docker-ce
         - git
         - curl
         - htop
       expected_services:
         - docker
         - node_exporter
         - payment-gateway
       expected_users:
         - velocitytech
         - monitoring
     
     tasks:
       - name: Check required packages are installed
         yum:
           list: "{{ item }}"
         register: package_check
         loop: "{{ expected_packages }}"
         failed_when: package_check.results | selectattr('results', 'defined') | selectattr('results', 'length', 0) | list | length > 0
       
       - name: Verify services are running
         service_facts:
       
       - name: Check service status
         assert:
           that:
             - ansible_facts.services[item + '.service'].state == 'running'
           fail_msg: "Service {{ item }} is not running"
           success_msg: "Service {{ item }} is running correctly"
         loop: "{{ expected_services }}"
       
       - name: Verify users exist
         getent:
           database: passwd
           key: "{{ item }}"
         loop: "{{ expected_users }}"
       
       - name: Check Docker container health
         command: docker ps --filter "status=running" --format "{{.Names}}"
         register: running_containers
         changed_when: false
       
       - name: Validate application containers
         assert:
           that:
             - "'payment-gateway' in running_containers.stdout"
             - "'postgres' in running_containers.stdout"
             - "'redis' in running_containers.stdout"
           fail_msg: "Required containers are not running"
           success_msg: "All application containers are running"
       
       - name: Test application endpoints
         uri:
           url: "http://localhost:3000/health"
           method: GET
           status_code: 200
         register: health_response
       
       - name: Validate health response
         assert:
           that:
             - health_response.json.status == "healthy"
           fail_msg: "Application health check failed"
           success_msg: "Application is healthy"
       
       - name: Check system resource usage
         shell: |
           echo "CPU: $(top -bn1 | grep 'Cpu(s)' | awk '{print $2}' | sed 's/%us,//')"
           echo "Memory: $(free | grep Mem | awk '{printf "%.1f%%", $3/$2 * 100.0}')"
           echo "Disk: $(df -h / | awk 'NR==2{printf "%s", $5}')"
         register: resource_usage
         changed_when: false
       
       - name: Display resource usage
         debug:
           msg: "{{ resource_usage.stdout_lines }}"
   ```

2. **Create Drift Detection Script**
   ```bash
   # Create scripts directory
   mkdir scripts
   
   # scripts/check-drift.sh
   cat > scripts/check-drift.sh << 'EOF'
   #!/bin/bash
   
   # VelocityTech Configuration Drift Detection
   
   INVENTORY="inventories/production"
   LOGFILE="/var/log/ansible-drift-check.log"
   
   echo "=== Configuration Drift Check - $(date) ===" | tee -a $LOGFILE
   
   # Run validation playbook
   if ansible-playbook -i $INVENTORY playbooks/validate.yml --vault-password-file ~/.vault_pass; then
       echo "✅ All servers pass configuration validation" | tee -a $LOGFILE
   else
       echo "❌ Configuration drift detected!" | tee -a $LOGFILE
       echo "🔧 Attempting automatic remediation..." | tee -a $LOGFILE
       
       # Run deployment playbook to fix drift
       if ansible-playbook -i $INVENTORY playbooks/deploy.yml --vault-password-file ~/.vault_pass; then
           echo "✅ Configuration drift remediated successfully" | tee -a $LOGFILE
       else
           echo "❌ Automatic remediation failed - manual intervention required" | tee -a $LOGFILE
           exit 1
       fi
   fi
   
   echo "=== Drift Check Complete ===" | tee -a $LOGFILE
   EOF
   
   chmod +x scripts/check-drift.sh
   
   # Store vault password securely (for automation)
   echo "your_vault_password" > ~/.vault_pass
   chmod 600 ~/.vault_pass
   ```

3. **Implement Configuration Compliance Reporting**
   ```yaml
   # playbooks/compliance-report.yml
   - name: Generate VelocityTech Compliance Report
     hosts: all
     become: yes
     gather_facts: yes
     
     tasks:
       - name: Collect system information
         set_fact:
           system_info:
             hostname: "{{ ansible_hostname }}"
             os_version: "{{ ansible_distribution }} {{ ansible_distribution_version }}"
             kernel_version: "{{ ansible_kernel }}"
             uptime: "{{ ansible_uptime_seconds | int | human_readable }}"
             cpu_count: "{{ ansible_processor_vcpus }}"
             memory_mb: "{{ ansible_memtotal_mb }}"
       
       - name: Check Docker version
         command: docker --version
         register: docker_version_check
         changed_when: false
       
       - name: Get running containers
         command: docker ps --format "{{.Names}}:{{.Status}}"
         register: container_status
         changed_when: false
       
       - name: Check disk usage
         command: df -h /
         register: disk_usage
         changed_when: false
       
       - name: Collect compliance data
         set_fact:
           compliance_data:
             system: "{{ system_info }}"
             docker_version: "{{ docker_version_check.stdout }}"
             containers: "{{ container_status.stdout_lines }}"
             disk_usage: "{{ disk_usage.stdout_lines[1] }}"
             timestamp: "{{ ansible_date_time.iso8601 }}"
       
       - name: Generate compliance report
         template:
           src: compliance-report.html.j2
           dest: "/tmp/compliance-report-{{ ansible_hostname }}.html"
           mode: '0644'
         delegate_to: localhost
   ```

4. **Create Compliance Report Template**
   ```html
   <!-- playbooks/templates/compliance-report.html.j2 -->
   <!DOCTYPE html>
   <html>
   <head>
       <title>VelocityTech Server Compliance Report</title>
       <style>
           body { font-family: Arial, sans-serif; margin: 20px; }
           .header { background-color: #f0f0f0; padding: 20px; }
           .section { margin: 20px 0; padding: 15px; border: 1px solid #ddd; }
           .good { color: green; }
           .warning { color: orange; }
           .error { color: red; }
           table { border-collapse: collapse; width: 100%; }
           th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
           th { background-color: #f2f2f2; }
       </style>
   </head>
   <body>
       <div class="header">
           <h1>VelocityTech Server Compliance Report</h1>
           <p><strong>Server:</strong> {{ compliance_data.system.hostname }}</p>
           <p><strong>Generated:</strong> {{ compliance_data.timestamp }}</p>
       </div>
       
       <div class="section">
           <h2>System Information</h2>
           <table>
               <tr><th>Property</th><th>Value</th></tr>
               <tr><td>Operating System</td><td>{{ compliance_data.system.os_version }}</td></tr>
               <tr><td>Kernel Version</td><td>{{ compliance_data.system.kernel_version }}</td></tr>
               <tr><td>Uptime</td><td>{{ compliance_data.system.uptime }}</td></tr>
               <tr><td>CPU Cores</td><td>{{ compliance_data.system.cpu_count }}</td></tr>
               <tr><td>Memory (MB)</td><td>{{ compliance_data.system.memory_mb }}</td></tr>
           </table>
       </div>
       
       <div class="section">
           <h2>Docker Configuration</h2>
           <p><strong>Version:</strong> {{ compliance_data.docker_version }}</p>
           <h3>Running Containers</h3>
           <ul>
           {% for container in compliance_data.containers %}
               <li class="good">{{ container }}</li>
           {% endfor %}
           </ul>
       </div>
       
       <div class="section">
           <h2>Disk Usage</h2>
           <pre>{{ compliance_data.disk_usage }}</pre>
       </div>
   </body>
   </html>
   ```

5. **Test Drift Detection and Remediation**
   ```bash
   # Run initial compliance report
   ansible-playbook -i inventories/production playbooks/compliance-report.yml --ask-vault-pass
   
   # View generated reports
   ls -la /tmp/compliance-report-*.html
   
   # Simulate configuration drift on one server
   ansible web1 -i inventories/production -m service -a "name=node_exporter state=stopped" --become
   
   # Run drift detection
   ./scripts/check-drift.sh
   
   # Verify automatic remediation worked
   ansible web1 -i inventories/production -m service -a "name=node_exporter" --become
   
   # Test validation playbook
   ansible-playbook -i inventories/production playbooks/validate.yml --ask-vault-pass
   
   # Create cron job for regular drift checking
   cat > /tmp/ansible-cron << 'EOF'
   # Check for configuration drift every 2 hours
   0 */2 * * * /path/to/velocitytech-config/scripts/check-drift.sh
   EOF
   
   # Install cron job (optional)
   # crontab /tmp/ansible-cron
   ```

**Checkpoint:** Configuration drift detection and automatic remediation system working

### Validation & Testing
**How to verify everything works:**
1. **Configuration Consistency Test** - `All servers have identical software versions and configurations`
2. **Idempotency Test** - `Running playbooks multiple times produces no changes`
3. **Drift Detection Test** - `Manual changes are detected and automatically corrected`
4. **Application Deployment Test** - `Payment gateway runs consistently across all servers`
5. **Compliance Reporting Test** - `HTML reports generated with complete system information`

**Expected Results:**
- **Configuration Consistency:** 100% identical configurations across all servers
- **Deployment Speed:** Complete application stack deployment in under 10 minutes
- **Drift Detection:** Automatic detection and remediation of configuration changes
- **Documentation:** Self-documenting infrastructure through Ansible playbooks
- **Reliability:** Zero configuration-related deployment failures

---

## 🔧 New Tools in Your Arsenal

### Ansible Configuration Management Platform
- **Purpose:** Automate server configuration and application deployment at scale
- **Key Commands:**
  - `ansible-playbook -i inventory playbook.yml` - Execute configuration playbooks
  - `ansible all -m ping` - Test connectivity to managed servers
  - `ansible-vault create secrets.yml` - Secure sensitive configuration data
  - `ansible-galaxy init role-name` - Create new role structure
- **Pro Tips:** Use `--check` for dry runs, `--diff` to see changes, group servers logically
- **Common Pitfalls:** Don't hardcode secrets, always test playbooks before production runs

### Ansible Roles and Playbook Organization
- **Purpose:** Create reusable, maintainable configuration components
- **Key Structure:**
  - Tasks: Configuration steps to execute
  - Templates: Dynamic configuration files with variables
  - Defaults: Default variable values
  - Handlers: Actions triggered by configuration changes
- **Pro Tips:** Keep roles focused and single-purpose, use role dependencies appropriately
- **Common Pitfalls:** Avoid complex role interdependencies, document role variables clearly

### Ansible Vault Secret Management
- **Purpose:** Secure storage and management of sensitive configuration data
- **Key Features:**
  - Encrypt sensitive variables and files
  - Integrate with existing playbooks seamlessly
  - Support for multiple vault passwords
- **Pro Tips:** Use separate vault files for different environments, rotate vault passwords regularly
- **Common Pitfalls:** Don't commit vault passwords to version control, use password files for automation

---

## 🔧 Common Issues & Solutions

### Issue #1: "Playbook runs successfully but configuration doesn't persist"
**Symptoms:** Ansible reports success but servers revert to previous configuration after reboot
**Cause:** Services not enabled, systemd daemon not reloaded, or temporary changes made
**Solution:**
```yaml
# Ensure services are enabled and started
- name: Start and enable service
  systemd:
    name: docker
    state: started
    enabled: yes
    daemon_reload: yes

# Use handlers for service restarts
handlers:
  - name: restart service
    systemd:
      name: docker
      state: restarted
      daemon_reload: yes
```
**Prevention:** Always enable services, use systemd module with daemon_reload, test after reboots

### Issue #2: "Ansible fails with SSH connection errors"
**Symptoms:** "SSH Error: Permission denied" or "Host key verification failed"
**Cause:** SSH keys not configured, strict host checking enabled, wrong user specified
**Solution:**
```bash
# Configure SSH properly in inventory
[webservers]
web1 ansible_host=1.2.3.4 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/key.pem

# Disable strict host checking for automation
ansible_ssh_common_args: '-o StrictHostKeyChecking=no'

# Test SSH connectivity manually first
ssh -i ~/.ssh/key.pem ec2-user@1.2.3.4
```
**Prevention:** Test SSH access manually, use consistent user accounts, manage host keys properly

### Issue #3: "Playbook tasks fail with permission denied errors"
**Symptoms:** Tasks fail with "Permission denied" when trying to install packages or modify system files
**Cause:** Ansible running without proper privileges, sudo not configured correctly
**Solution:**
```yaml
# Use become for privilege escalation
- name: Install packages
  yum:
    name: docker
    state: present
  become: yes

# Configure sudo access for automation user
- name: Configure sudoers
  lineinfile:
    path: /etc/sudoers.d/ansible
    line: 'ansible ALL=(ALL) NOPASSWD:ALL'
    create: yes
    mode: '0440'
  become: yes
```
**Prevention:** Plan privilege requirements, configure passwordless sudo for automation, use become consistently

---

## 🎭 Character Evolution This Week

### Legacy Luka's Configuration Enlightenment
**Monday:** "More YAML to learn? I thought we were done with new configuration languages after Docker Compose."
**Tuesday:** "Wait... I can version control my server configurations just like application code?"
**Wednesday:** "Ansible playbooks are like deployment scripts, but much more powerful and reliable."
**Thursday:** "I can test server configurations before applying them. This prevents so many mistakes!"
**Friday:** "Configuration as Code makes server management predictable. No more mystery configurations!"
**Key Quote:** "Ansible turned server configuration from art into engineering."

### Anxious Ana's Testing Environment Mastery
**Monday:** "Another automation tool? How many systems do I need to master to do effective testing?"
**Tuesday:** "I can provision identical test environments instantly! No more waiting for Filip or environment variations."
**Wednesday:** "Every test runs against exactly the same server configuration. My results are finally reliable!"
**Thursday:** "I can test infrastructure changes before they affect our applications. Revolutionary!"
**Friday:** "Ansible gave me control over the entire testing stack. I'm not dependent on anyone for environments."
**Key Quote:** "Configuration management turned environment creation from weeks to minutes."

### Firefighter Filip's Operations Transformation
**Monday:** "Great, now I need to learn Ansible on top of Terraform, Docker, and Kubernetes. More complexity."
**Tuesday:** "Automated configuration means no more manual server setup variations. Every server is identical!"
**Wednesday:** "I can detect and fix configuration drift automatically. No more mysterious server differences!"
**Thursday:** "Disaster recovery became trivial - I can rebuild any server from playbooks in minutes."
**Friday:** "My weekends are mine again. Ansible eliminated configuration emergencies and manual maintenance."
**Key Quote:** "Configuration management transformed me from a server maintainer to an infrastructure orchestrator."

### Manager Maja's Strategic Operations Vision
**Monday:** "How much time will it take to learn and implement Ansible across our infrastructure?"
**Tuesday:** "Server configuration time dropped from hours to minutes. Our provisioning bottleneck is eliminated!"
**Wednesday:** "We can guarantee consistent environments for every client. That's a competitive advantage!"
**Thursday:** "Configuration compliance and audit trails are automatic. Regulatory compliance becomes manageable."
**Friday:** "Ansible isn't just about efficiency - it's about reliability and scalability that enables business growth."
**Key Quote:** "Configuration management turned infrastructure from a constraint into a capability."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 8 Improvements:**

| Metric | Week 7 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 12 days | 10 days | 2 hours |
| Server Configuration Time | 4-6 hours | 10 minutes | <5 minutes |
| Configuration Consistency | 60% | 100% | 100% |
| Configuration Drift Incidents | Weekly | Zero | Zero |
| Manual Configuration Tasks | 15 hours/week | 1 hour/week | <30 minutes/week |
| Environment Provisioning | 1 day | 15 minutes | <10 minutes |
| Application Deployment Reliability | 85% | 98% | 99% |

**Key Improvements:**
- **Configuration Speed:** 24x faster server configuration (4-6 hours → 10 minutes)
- **Consistency Achievement:** 100% identical configurations across all environments
- **Drift Elimination:** Zero configuration drift incidents through automated detection
- **Manual Work Reduction:** 93% reduction in manual configuration tasks

---

## 🇭🇷 Croatian IT Market Reality
Configuration Management expertise is highly valued in Croatian companies. Organizations like Infobip, Rimac, and Span use Ansible extensively to manage thousands of servers consistently. Configuration automation skills are essential for senior DevOps roles and command premium salaries. Many Croatian companies still manually configure servers, creating high demand for professionals who can implement configuration management practices. Your Ansible expertise positions you for senior infrastructure engineer, DevOps engineer, and platform engineer roles.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about configuration management?** How treating server configuration like code improves reliability and reduces manual work
2. **Which aspect had the biggest impact on team productivity?** Eliminating configuration drift and manual setup variations
3. **How did Ansible change your view of server management?** From manual procedures to automated, testable configurations
4. **What questions do you still have?** How to manage configuration at enterprise scale with proper governance and security

**Team Discussion Points:**
- How would each VelocityTech character feel about automating all server configurations with Ansible?
- What resistance might we encounter from teams comfortable with manual server management?
- How would you convince skeptical system administrators to adopt configuration management practices?

**Preparation for Next Week:**
Filip arrives Monday morning relieved about consistent server configurations but concerned: "The Ansible playbooks work great, but we still have security vulnerabilities. Every deployment potentially exposes sensitive data, and I'm worried about container security and network access controls. We need to build security into our CI/CD pipeline!" The DevSecOps and secure deployment journey begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Ansible for DevOps" by Jeff Geerling - Comprehensive configuration management guide
- "Infrastructure as Code" by Kief Morris - Chapters on configuration management best practices

**Hands-on Practice:**
- Implement Ansible for your personal server configurations
- Practice with Ansible Galaxy roles and community playbooks
- Experiment with Ansible Tower/AWX for enterprise features

**Industry Insights:**
- "State of Configuration Management" reports - Adoption trends and challenges
- Red Hat Ansible surveys - Usage patterns and best practices
- Croatian DevOps meetups - Network with local configuration management practitioners

**Advanced Topics:**
- Ansible testing with Molecule and Testinfra
- Dynamic inventories and cloud integration
- Ansible Vault advanced features and key management
- CI/CD integration with configuration management

**Croatian Companies Using Ansible:**
- **Infobip:** Managing communication platform infrastructure across 50+ countries
- **Rimac:** Automotive development environment configuration automation
- **Span:** Web platform server configuration and application deployment
- **Photomath:** Mobile backend infrastructure consistency at scale
- **Include:** Financial services compliance-focused configuration management

**Certification Path:**
- **Red Hat Certified Specialist in Ansible Automation** - Official Ansible certification
- **Red Hat Certified Engineer (RHCE)** - Advanced Linux automation skills
- **AWS DevOps Engineer** - Cloud-specific configuration management

---

*Next up: Week 9 - Secure CI/CD + Advanced Deployments: "Building Security Into the Pipeline" →*

> *"Configuration management doesn't just automate server setup - it automates consistency. When infrastructure becomes predictable, operations become reliable."* - Configuration Philosophy


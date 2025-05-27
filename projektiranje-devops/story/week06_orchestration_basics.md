# Week 6: Orchestration Basics - "Managing the Container Zoo" ☸️
**VelocityTech Transformation Week 6**

**Story Context:** Containers work perfectly locally, but managing them across multiple servers creates chaos  
**Focus:** Implementing Kubernetes basics for container orchestration and service management  
**Characters in Focus:** Filip's multi-server deployment nightmares, Ana's cross-environment testing complexity, Luka's scaling confusion

---

## 🔥 The Weekly Crisis: "The Great Container Stampede"
**Setting:** VelocityTech office, Tuesday 11:30 AM. Three servers, dozens of containers, and growing chaos.  
**The Problem:** Payment gateway containers work beautifully in isolation but managing them across production infrastructure is becoming impossible

**Scene:** [VelocityTech server room and main floor. Filip juggling three laptops connected to different servers. Ana trying to test across multiple container instances. Luka staring at server monitoring dashboards showing inconsistent loads.]

**FILIP** *(frantically switching between server terminals)*  
> "Server 1 has payment-gateway running on port 3001, Server 2 on port 3002..."  
> "But the load balancer is configured for 3000, and I can't remember which version is where!"  
> *(typing commands rapidly)* "Container crashed on Server 3, manually restarting... again."

**ANA** *(testing payment flows, growing frustrated)*  
> "Which container instance should I test against? They're all running different versions!"  
> "Server 1 has yesterday's build, Server 2 has this morning's, and Server 3... I have no idea."  
> *(checking her test results)* "My test results are inconsistent because I'm hitting different container versions!"

**LUKA** *(looking at server load graphs)*  
> "Server 1 is at 90% CPU while Server 2 is idle at 15%..."  
> "Shouldn't the containers distribute the load automatically?"  
> *(confused)* "How do I scale up during peak hours? Start more containers manually on each server?"

**MAJA** *(entering with a client email on her phone)*  
> "Client says the payment system was slow this morning but fast this afternoon..."  
> "How can the same system have different performance at different times?"

**FILIP** *(exhausted, pulling up a manual deployment checklist)*  
> "I have a 47-step process for deploying containers across all servers..."  
> "Check Server 1, update container, restart, test, move to Server 2, repeat..."  
> *(showing his notebook)* "And if one fails, I have to manually roll everything back."

**ANA** *(overwhelmed)*  
> "I need consistent test environments, but every server setup is slightly different!"  
> "Container ports, network configurations, storage locations - nothing's standardized!"

**LUKA** *(defensive but concerned)*  
> "The containers work fine! It's not a code problem!"  
> "But... how do users know which server to connect to? What happens if Server 2 goes down?"

**MARKO** *(entering, immediately recognizing the pattern)*  
> "Let me guess - containers are great, but managing them manually across servers is chaos?"  
> *(to you)* "This is exactly why container orchestration exists. Ready to meet Kubernetes?"

**FILIP** *(hopeful but skeptical)*  
> "I've heard Kubernetes is incredibly complex. Isn't that overkill for three servers?"

**MAJA** *(desperately)*  
> "There has to be a way to manage containers that doesn't require Filip to work weekends!"  
> "Something that automatically handles failures, scaling, and deployment?"

**[All eyes turn to you - the intern who might understand modern container management.]**

**ANA** *(pleading)*  
> "Please tell me there's a solution that gives me consistent environments to test against!"

**[Your systems thinking activates. This isn't about individual containers - it's about orchestrating a distributed system.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Deploy and manage Kubernetes clusters** - Set up local Kubernetes environments and understand core concepts
2. **Create Kubernetes deployments and services** - Deploy containerized applications with automatic scaling and service discovery
3. **Implement basic container orchestration** - Manage application lifecycle, rolling updates, and failure recovery

**Real-world Connection:** Kubernetes skills are the most sought-after expertise in Croatian DevOps market. Companies like Infobip, Rimac, and Span are actively hiring Kubernetes engineers. Even basic K8s knowledge significantly increases your market value and opens doors to cloud-native architecture roles.

---

## 📚 The Knowledge Foundation

### Kubernetes Fundamentals and Architecture
**The Problem it Solves:** Manual container management, inconsistent deployments, no automatic scaling, complex service discovery, manual failure recovery  
**How it Works:** Kubernetes orchestrates containers across clusters of machines, providing automated deployment, scaling, and management

**Kubernetes Core Architecture:**
```
┌─────────────────────────────────────────────────────────────────┐
│                        Kubernetes Cluster                       │
├─────────────────────────────────────────────────────────────────┤
│  Control Plane (Master)          │    Worker Nodes              │
│  ┌─────────────────────────────┐  │  ┌─────────────────────────┐ │
│  │ API Server                  │  │  │ Pod: payment-gateway    │ │
│  │ etcd (cluster state)        │  │  │ Pod: postgres           │ │
│  │ Scheduler                   │  │  │ kubelet (node agent)    │ │
│  │ Controller Manager          │  │  │ kube-proxy (networking) │ │
│  └─────────────────────────────┘  │  └─────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

**Key Kubernetes Objects:**
- **Pod:** Smallest deployable unit, usually one container
- **Deployment:** Manages replica sets and rolling updates
- **Service:** Provides stable networking for pods
- **ConfigMap/Secret:** Configuration and sensitive data management
- **Namespace:** Logical resource isolation

**Kubernetes Benefits:**
- **Declarative Configuration:** Describe desired state, K8s makes it happen
- **Self-Healing:** Automatically restarts failed containers
- **Horizontal Scaling:** Add/remove replicas based on load
- **Service Discovery:** Automatic networking between services
- **Rolling Updates:** Zero-downtime deployments

**Real-world Example:** Netflix runs thousands of microservices on Kubernetes, handling millions of requests with automatic scaling  
**VelocityTech Connection:** K8s would eliminate Filip's manual container management and provide Ana with consistent test environments

### Pods, Services, and Deployments
**The Problem it Solves:** Inconsistent container deployment, manual scaling, no load balancing, complex networking  
**How it Works:** Kubernetes objects provide abstractions for deploying, scaling, and networking containerized applications

**Pod Fundamentals:**
```yaml
# Basic Pod (usually not created directly)
apiVersion: v1
kind: Pod
metadata:
  name: payment-gateway-pod
  labels:
    app: payment-gateway
spec:
  containers:
  - name: payment-gateway
    image: velocitytech/payment-gateway:latest
    ports:
    - containerPort: 3000
    env:
    - name: NODE_ENV
      value: "production"
```

**Deployment for Production:**
```yaml
# Deployment manages Pods and ReplicaSets
apiVersion: apps/v1
kind: Deployment
metadata:
  name: payment-gateway-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: payment-gateway
  template:
    metadata:
      labels:
        app: payment-gateway
    spec:
      containers:
      - name: payment-gateway
        image: velocitytech/payment-gateway:latest
        ports:
        - containerPort: 3000
        resources:
          requests:
            memory: "128Mi"
            cpu: "250m"
          limits:
            memory: "256Mi"
            cpu: "500m"
```

**Service for Networking:**
```yaml
# Service provides stable networking
apiVersion: v1
kind: Service
metadata:
  name: payment-gateway-service
spec:
  selector:
    app: payment-gateway
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

**Real-world Example:** Spotify uses Deployments to manage thousands of microservice instances with automatic scaling  
**VelocityTech Connection:** Deployments would give Ana consistent environments and Filip automatic failure recovery

### Local Kubernetes Setup and kubectl Basics
**The Problem it Solves:** Complex Kubernetes learning curve, expensive cloud costs for development, inconsistent local environments  
**How it Works:** Local Kubernetes distributions provide full K8s functionality on development machines

**Local Kubernetes Options:**
- **minikube:** Full Kubernetes cluster in VM or container
- **kind:** Kubernetes in Docker, fastest for development
- **Docker Desktop:** Built-in Kubernetes, easiest setup
- **k3d:** Lightweight K3s in Docker containers

**kubectl Essential Commands:**
```bash
# Cluster information
kubectl cluster-info
kubectl get nodes

# Working with resources
kubectl get pods
kubectl get deployments
kubectl get services

# Creating resources
kubectl apply -f deployment.yaml
kubectl create deployment nginx --image=nginx

# Debugging and logs
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh

# Scaling and updates
kubectl scale deployment <name> --replicas=5
kubectl set image deployment/<name> container=new-image:tag
```

**Kubernetes Namespaces for Organization:**
```bash
# Create namespace for different environments
kubectl create namespace development
kubectl create namespace testing
kubectl create namespace production

# Deploy to specific namespace
kubectl apply -f deployment.yaml -n development
kubectl get pods -n testing
```

**Real-world Example:** Development teams at Google use local Kubernetes clusters that mirror production environments  
**VelocityTech Connection:** Local K8s would give the team production-like environments for development and testing

**Key Takeaways:**
- Kubernetes orchestrates containers across multiple machines automatically
- Deployments provide declarative application management with scaling and updates
- Services handle networking and load balancing between containers
- Local Kubernetes enables learning and development without cloud costs

---

## 🛠️ Lab Mission: "The Kubernetes Command Center"
**Scenario:** VelocityTech's manual container management is breaking down - need orchestrated deployment across servers  
**Your Mission:** Deploy payment gateway to Kubernetes cluster with automatic scaling and service discovery  
**Success Criteria:** Payment gateway running reliably with 3 replicas, accessible through stable service, auto-recovery from failures

### Phase 1: Local Kubernetes Cluster Setup (45 minutes)
**Objective:** Set up local Kubernetes environment and deploy first application

**Steps:**
1. **Install and Configure Local Kubernetes**
   ```bash
   # Option 1: Using minikube (recommended for learning)
   # Install minikube (follow OS-specific instructions)
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   
   # Start minikube cluster
   minikube start --driver=docker --cpus=2 --memory=4g
   
   # Verify cluster is running
   kubectl cluster-info
   kubectl get nodes
   
   # Expected output: 
   # NAME       STATUS   ROLES           AGE   VERSION
   # minikube   Ready    control-plane   1m    v1.28.3
   
   # Option 2: Using kind (alternative)
   # kind create cluster --name velocitytech
   # kubectl cluster-info --context kind-velocitytech
   ```
   - Expected output: Working Kubernetes cluster with one node
   - Troubleshooting: If minikube fails to start, try different driver: `--driver=virtualbox` or `--driver=hyperv`

2. **Create Kubernetes Namespace and Basic Pod**
   ```bash
   # Create namespace for VelocityTech
   kubectl create namespace velocitytech
   
   # Set default namespace for convenience
   kubectl config set-context --current --namespace=velocitytech
   
   # Verify namespace
   kubectl get namespaces
   
   # Create simple pod to test cluster
   cat > simple-pod.yaml << 'EOF'
   apiVersion: v1
   kind: Pod
   metadata:
     name: test-pod
     namespace: velocitytech
     labels:
       app: test
   spec:
     containers:
     - name: nginx
       image: nginx:alpine
       ports:
       - containerPort: 80
   EOF
   
   # Deploy pod
   kubectl apply -f simple-pod.yaml
   
   # Verify pod is running
   kubectl get pods
   kubectl describe pod test-pod
   
   # Test pod connectivity
   kubectl port-forward test-pod 8080:80 &
   curl http://localhost:8080
   # Expected: nginx welcome page
   
   # Clean up test pod
   kubectl delete pod test-pod
   ```

3. **Deploy Payment Gateway Container to Kubernetes**
   ```yaml
   # payment-gateway-pod.yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: payment-gateway-pod
     namespace: velocitytech
     labels:
       app: payment-gateway
       version: "1.0"
   spec:
     containers:
     - name: payment-gateway
       image: velocitytech/payment-gateway:optimized
       ports:
       - containerPort: 3000
         name: http
       env:
       - name: NODE_ENV
         value: "production"
       - name: PORT
         value: "3000"
       - name: DB_HOST
         value: "postgres-service"
       - name: DB_PORT
         value: "5432"
       - name: DB_NAME
         value: "payment_prod"
       - name: DB_USER
         value: "prod_user"
       - name: DB_PASSWORD
         value: "prod_password"  # In real world, use Secrets
       resources:
         requests:
           memory: "128Mi"
           cpu: "100m"
         limits:
           memory: "256Mi"
           cpu: "500m"
       livenessProbe:
         httpGet:
           path: /health
           port: 3000
         initialDelaySeconds: 30
         periodSeconds: 10
       readinessProbe:
         httpGet:
           path: /health
           port: 3000
         initialDelaySeconds: 5
         periodSeconds: 5
   ```

4. **Test Pod Deployment and Basic Operations**
   ```bash
   # Deploy payment gateway pod
   kubectl apply -f payment-gateway-pod.yaml
   
   # Watch pod startup
   kubectl get pods -w
   # Wait for STATUS: Running
   
   # Check pod details
   kubectl describe pod payment-gateway-pod
   
   # View pod logs
   kubectl logs payment-gateway-pod
   
   # Test application inside pod
   kubectl exec -it payment-gateway-pod -- curl http://localhost:3000/health
   
   # Port forward to test locally
   kubectl port-forward payment-gateway-pod 3000:3000 &
   curl http://localhost:3000/health
   # Expected: {"status": "healthy", "timestamp": "..."}
   
   # Clean up port forward
   pkill -f "kubectl port-forward"
   ```

**Checkpoint:** Payment gateway pod running successfully in Kubernetes with health checks

### Phase 2: Deployments and Scaling (75 minutes)
**Objective:** Create production-ready Kubernetes Deployment with multiple replicas and automatic management

**Steps:**
1. **Create Production Deployment Configuration**
   ```yaml
   # payment-gateway-deployment.yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-gateway-deployment
     namespace: velocitytech
     labels:
       app: payment-gateway
   spec:
     replicas: 3
     strategy:
       type: RollingUpdate
       rollingUpdate:
         maxSurge: 1
         maxUnavailable: 1
     selector:
       matchLabels:
         app: payment-gateway
     template:
       metadata:
         labels:
           app: payment-gateway
           version: "1.0"
       spec:
         containers:
         - name: payment-gateway
           image: velocitytech/payment-gateway:optimized
           ports:
           - containerPort: 3000
             name: http
           env:
           - name: NODE_ENV
             value: "production"
           - name: PORT
             value: "3000"
           - name: DB_HOST
             value: "postgres-service"
           - name: REDIS_URL
             value: "redis://redis-service:6379"
           resources:
             requests:
               memory: "128Mi"
               cpu: "100m"
             limits:
               memory: "256Mi"
               cpu: "500m"
           livenessProbe:
             httpGet:
               path: /health
               port: 3000
             initialDelaySeconds: 30
             periodSeconds: 10
             timeoutSeconds: 5
             failureThreshold: 3
           readinessProbe:
             httpGet:
               path: /ready
               port: 3000
             initialDelaySeconds: 5
             periodSeconds: 5
             timeoutSeconds: 3
             failureThreshold: 3
   ```

2. **Deploy and Test Scaling Operations**
   ```bash
   # Delete single pod first (if still running)
   kubectl delete pod payment-gateway-pod --ignore-not-found
   
   # Deploy the deployment
   kubectl apply -f payment-gateway-deployment.yaml
   
   # Watch deployment rollout
   kubectl rollout status deployment/payment-gateway-deployment
   
   # Check deployment and pods
   kubectl get deployments
   kubectl get pods -l app=payment-gateway
   
   # Expected: 3 pods running
   # NAME                                         READY   STATUS    RESTARTS   AGE
   # payment-gateway-deployment-xxx-yyy          1/1     Running   0          1m
   # payment-gateway-deployment-xxx-zzz          1/1     Running   0          1m
   # payment-gateway-deployment-xxx-aaa          1/1     Running   0          1m
   
   # Test horizontal scaling
   kubectl scale deployment payment-gateway-deployment --replicas=5
   kubectl get pods -l app=payment-gateway
   # Should see 5 pods
   
   # Scale back down
   kubectl scale deployment payment-gateway-deployment --replicas=3
   kubectl get pods -l app=payment-gateway -w
   # Watch 2 pods terminate gracefully
   ```

3. **Test Self-Healing and Failure Recovery**
   ```bash
   # Get list of pods
   kubectl get pods -l app=payment-gateway
   
   # Delete one pod to test self-healing
   POD_NAME=$(kubectl get pods -l app=payment-gateway -o jsonpath='{.items[0].metadata.name}')
   echo "Deleting pod: $POD_NAME"
   kubectl delete pod $POD_NAME
   
   # Watch Kubernetes automatically create replacement
   kubectl get pods -l app=payment-gateway -w
   # Should see: 
   # 1. Pod terminating
   # 2. New pod creating  
   # 3. New pod running
   # Total pods should remain 3
   
   # Test deployment history
   kubectl rollout history deployment/payment-gateway-deployment
   
   # Test rollback capability (simulate bad deployment)
   kubectl set image deployment/payment-gateway-deployment payment-gateway=nginx:broken-tag
   kubectl rollout status deployment/payment-gateway-deployment
   # Should fail or timeout
   
   # Rollback to previous version
   kubectl rollout undo deployment/payment-gateway-deployment
   kubectl rollout status deployment/payment-gateway-deployment
   # Should return to working state
   ```

4. **Create ConfigMap for Environment Configuration**
   ```yaml
   # payment-gateway-config.yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: payment-gateway-config
     namespace: velocitytech
   data:
     NODE_ENV: "production"
     PORT: "3000"
     LOG_LEVEL: "info"
     MAX_CONNECTIONS: "100"
     TIMEOUT_MS: "30000"
   ---
   apiVersion: v1
   kind: Secret
   metadata:
     name: payment-gateway-secrets
     namespace: velocitytech
   type: Opaque
   data:
     DB_PASSWORD: cHJvZF9wYXNzd29yZA==  # base64 encoded "prod_password"
     API_KEY: c2VjcmV0X2FwaV9rZXk=       # base64 encoded "secret_api_key"
   ```

   ```bash
   # Apply configuration
   kubectl apply -f payment-gateway-config.yaml
   
   # Update deployment to use ConfigMap and Secret
   # (Add to deployment.yaml under containers.env:)
   ```

   ```yaml
   # Update deployment to use external config
           envFrom:
           - configMapRef:
               name: payment-gateway-config
           env:
           - name: DB_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: payment-gateway-secrets
                 key: DB_PASSWORD
           - name: API_KEY
             valueFrom:
               secretKeyRef:
                 name: payment-gateway-secrets
                 key: API_KEY
   ```

**Checkpoint:** Deployment with 3 replicas, self-healing, scaling, and external configuration

### Phase 3: Services and Networking (60 minutes)
**Objective:** Create stable networking for payment gateway and enable external access

**Steps:**
1. **Create Service for Internal Communication**
   ```yaml
   # payment-gateway-service.yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-gateway-service
     namespace: velocitytech
     labels:
       app: payment-gateway
   spec:
     type: ClusterIP
     selector:
       app: payment-gateway
     ports:
     - name: http
       protocol: TCP
       port: 80
       targetPort: 3000
     sessionAffinity: None
   ---
   # External service for testing
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-gateway-external
     namespace: velocitytech
     labels:
       app: payment-gateway
   spec:
     type: NodePort
     selector:
       app: payment-gateway
     ports:
     - name: http
       protocol: TCP
       port: 80
       targetPort: 3000
       nodePort: 30080
   ```

2. **Deploy Services and Test Connectivity**
   ```bash
   # Deploy services
   kubectl apply -f payment-gateway-service.yaml
   
   # Check services
   kubectl get services
   # Expected output:
   # NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
   # payment-gateway-service     ClusterIP   10.96.x.x       <none>        80/TCP         1m
   # payment-gateway-external    NodePort    10.96.x.x       <none>        80:30080/TCP   1m
   
   # Test internal service connectivity
   kubectl run test-client --image=curlimages/curl -it --rm --restart=Never -- \
     curl -s http://payment-gateway-service/health
   # Expected: {"status": "healthy", ...}
   
   # Test external access (minikube)
   minikube service payment-gateway-external --url -n velocitytech
   # Get URL and test
   EXTERNAL_URL=$(minikube service payment-gateway-external --url -n velocitytech)
   curl $EXTERNAL_URL/health
   ```

3. **Create Complete Application Stack with Dependencies**
   ```yaml
   # postgres-deployment.yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: postgres-deployment
     namespace: velocitytech
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: postgres
     template:
       metadata:
         labels:
           app: postgres
       spec:
         containers:
         - name: postgres
           image: postgres:14-alpine
           ports:
           - containerPort: 5432
           env:
           - name: POSTGRES_DB
             value: "payment_prod"
           - name: POSTGRES_USER
             value: "prod_user"
           - name: POSTGRES_PASSWORD
             value: "prod_password"
           - name: PGDATA
             value: "/var/lib/postgresql/data/pgdata"
           volumeMounts:
           - name: postgres-storage
             mountPath: /var/lib/postgresql/data
           readinessProbe:
             exec:
               command:
                 - /bin/sh
                 - -c
                 - pg_isready -U prod_user -d payment_prod
             initialDelaySeconds: 15
             periodSeconds: 5
         volumes:
         - name: postgres-storage
           emptyDir: {}
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: postgres-service
     namespace: velocitytech
   spec:
     selector:
       app: postgres
     ports:
     - protocol: TCP
       port: 5432
       targetPort: 5432
   ```

4. **Deploy Full Stack and Test Integration**
   ```bash
   # Deploy PostgreSQL
   kubectl apply -f postgres-deployment.yaml
   
   # Wait for postgres to be ready
   kubectl wait --for=condition=available --timeout=300s deployment/postgres-deployment
   
   # Update payment gateway deployment to connect to postgres
   kubectl rollout restart deployment/payment-gateway-deployment
   
   # Wait for rollout to complete
   kubectl rollout status deployment/payment-gateway-deployment
   
   # Test full application stack
   EXTERNAL_URL=$(minikube service payment-gateway-external --url -n velocitytech)
   
   # Test health endpoint
   curl $EXTERNAL_URL/health
   
   # Test payment processing (if endpoint exists)
   curl -X POST $EXTERNAL_URL/api/payments/process \
     -H "Content-Type: application/json" \
     -d '{
       "amount": 99.99,
       "cardNumber": "4111111111111111",
       "expiryDate": "12/25",
       "cvv": "123"
     }' || echo "Payment endpoint test complete"
   
   # Check logs for database connectivity
   kubectl logs deployment/payment-gateway-deployment
   
   # Verify all components are running
   kubectl get all -n velocitytech
   ```

**Checkpoint:** Complete application stack running in Kubernetes with service discovery and external access

### Validation & Testing
**How to verify everything works:**
1. **Cluster Health Test** - `kubectl get nodes shows Ready status`
2. **Application Test** - `3 payment gateway pods running and healthy`
3. **Service Discovery Test** - `Internal services can communicate`
4. **Scaling Test** - `Can scale from 3 to 5 replicas successfully`
5. **Self-Healing Test** - `Deleting pod triggers automatic replacement`

**Expected Results:**
- **High Availability:** 3 replicas with automatic failover
- **Service Discovery:** Stable internal networking between services
- **External Access:** Application accessible from outside cluster
- **Self-Healing:** Automatic recovery from pod failures
- **Scalability:** Easy horizontal scaling of application components

---

## 🔧 New Tools in Your Arsenal

### kubectl Command Line Interface
- **Purpose:** Interact with Kubernetes clusters for deployment, management, and debugging
- **Key Commands:**
  - `kubectl get pods` - List running pods
  - `kubectl apply -f file.yaml` - Deploy resources declaratively
  - `kubectl logs pod-name` - View application logs
  - `kubectl exec -it pod-name -- bash` - Interactive shell in pod
- **Pro Tips:** Use `-o wide` for detailed output, `--watch` for real-time updates
- **Common Pitfalls:** Don't forget namespace flags, always check resource quotas

### Kubernetes Deployments
- **Purpose:** Manage application lifecycle with scaling, updates, and rollbacks
- **Key Features:**
  - ReplicaSet management for desired pod count
  - Rolling updates with zero downtime
  - Rollback capability to previous versions
- **Pro Tips:** Use resource limits, health checks, and update strategies
- **Common Pitfalls:** Don't set replicas too high without resource limits

### Kubernetes Services
- **Purpose:** Provide stable networking and load balancing for pods
- **Key Types:**
  - ClusterIP: Internal cluster communication
  - NodePort: External access via node ports
  - LoadBalancer: Cloud provider load balancer integration
- **Pro Tips:** Use meaningful service names, configure session affinity when needed
- **Common Pitfalls:** Don't expose services externally unless necessary

---

## 🔧 Common Issues & Solutions

### Issue #1: "Pods stuck in Pending state"
**Symptoms:** `kubectl get pods` shows STATUS: Pending for extended time
**Cause:** Insufficient cluster resources, scheduling constraints, or image pull issues
**Solution:**
```bash
# Check pod details for specific error
kubectl describe pod <pod-name>

# Common fixes:
# 1. Resource limits too high
kubectl get nodes
kubectl describe nodes

# 2. Image pull problems
kubectl get events --sort-by=.metadata.creationTimestamp

# 3. Node selector issues
kubectl get nodes --show-labels
```
**Prevention:** Set appropriate resource requests/limits, use available images, check node capacity

### Issue #2: "Service not accessible from outside cluster"
**Symptoms:** Can't reach application via service URL, connection timeouts
**Cause:** Service type misconfiguration, firewall rules, or networking issues
**Solution:**
```bash
# Check service configuration
kubectl get svc -o wide
kubectl describe svc <service-name>

# For minikube, use service URL
minikube service <service-name> --url

# Check if pods are ready
kubectl get pods -o wide
kubectl get endpoints <service-name>
```
**Prevention:** Use correct service types, ensure pod readiness probes pass, check security groups

### Issue #3: "Rolling update stuck or failing"
**Symptoms:** Deployment update hangs, old pods not terminating, mixed versions running
**Cause:** Health check failures, resource constraints, or configuration errors
**Solution:**
```bash
# Check rollout status
kubectl rollout status deployment/<name>

# Check deployment events
kubectl describe deployment <name>

# Check pod logs for errors
kubectl logs deployment/<name>

# Rollback if necessary
kubectl rollout undo deployment/<name>
```
**Prevention:** Test deployments in staging, use proper health checks, set appropriate timeouts

---

## 🎭 Character Evolution This Week

### Legacy Luka's Orchestration Understanding
**Monday:** "Kubernetes looks incredibly complex. Why not just use simple scripts to manage containers?"
**Tuesday:** "Wait... I can describe what I want and Kubernetes makes it happen automatically?"
**Wednesday:** "Scaling is just changing a number in a YAML file. This is actually simpler than manual management."
**Thursday:** "Rolling updates mean I can deploy without downtime. No more midnight maintenance windows!"
**Friday:** "Kubernetes abstracts away server management. I can focus on applications, not infrastructure."
**Key Quote:** "Kubernetes isn't complex - it's powerful. There's a difference."

### Anxious Ana's Testing Consistency Victory
**Monday:** "Another system to learn? How many tools do I need to master?"
**Tuesday:** "Every test environment is identical now. No more environment-specific bugs!"
**Wednesday:** "I can spin up complete test environments in seconds, not hours."
**Thursday:** "Namespace isolation means I can test multiple versions simultaneously without conflicts."
**Friday:** "Kubernetes gave me the consistent, reliable test environments I've always dreamed of."
**Key Quote:** "Finally, testing infrastructure that works with me instead of against me."

### Firefighter Filip's Operations Revolution
**Monday:** "Great, now I need to learn Kubernetes on top of Docker. More complexity to manage."
**Tuesday:** "Self-healing containers? Pods restart automatically when they fail?"
**Wednesday:** "I haven't had a single manual restart request this week. Kubernetes handles failures gracefully."
**Thursday:** "Scaling up for peak traffic is a single command. No more server provisioning panic."
**Friday:** "My monitoring shows 99.9% uptime without any manual intervention. This is incredible."
**Key Quote:** "Kubernetes turned me from a firefighter into an architect."

### Manager Maja's Strategic Transformation
**Monday:** "Kubernetes seems like overkill for our simple applications. Can't we stick with simpler solutions?"
**Tuesday:** "Zero-downtime deployments mean we can release features anytime, not just maintenance windows."
**Wednesday:** "Automatic scaling saves us money - we only use resources when we need them."
**Thursday:** "Client confidence increased dramatically with our 99.9% uptime improvements."
**Friday:** "Kubernetes isn't just technology - it's a competitive advantage that enables business agility."
**Key Quote:** "Kubernetes transformed our deployment from risk to routine."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 6 Improvements:**

| Metric | Week 5 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 18 days | 15 days | 2 hours |
| Deployment Downtime | 15 minutes | 0 minutes | 0 minutes |
| Manual Server Management | 2 hours/week | 10 minutes/week | <5 minutes/week |
| Application Uptime | 99.2% | 99.9% | 99.99% |
| Scaling Response Time | 30 minutes | 30 seconds | <10 seconds |
| Environment Consistency | 100% | 100% | 100% |
| Recovery Time from Failures | 15 minutes | <1 minute | <30 seconds |

**Key Improvements:**
- **Zero-Downtime Deployments:** Eliminated deployment windows completely
- **Automatic Scaling:** 60x faster response to load changes (30 min → 30 sec)
- **Self-Healing Infrastructure:** 15x faster recovery from failures
- **Operational Efficiency:** 92% reduction in manual server management tasks

---

## 🇭🇷 Croatian IT Market Reality
Kubernetes expertise is the most valuable skill in the Croatian DevOps market. Companies like Infobip, Rimac, and Span are actively seeking Kubernetes engineers with salaries 40-60% higher than traditional infrastructure roles. Even basic K8s knowledge opens doors to cloud-native architecture positions. Croatian companies are rapidly adopting container orchestration, creating high demand for professionals who can implement and manage Kubernetes clusters. Your orchestration skills position you for senior DevOps engineer and platform engineer roles.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about Kubernetes?** How it simplifies complex operations through declarative configuration
2. **Which Kubernetes concept had the biggest impact?** Self-healing capabilities that eliminate manual intervention
3. **How did orchestration change your view of infrastructure management?** From imperative commands to declarative desired state
4. **What questions do you still have?** How to manage Kubernetes clusters in production with monitoring and security

**Team Discussion Points:**
- How would each VelocityTech character feel about migrating all applications to Kubernetes?
- What resistance might we encounter from teams comfortable with traditional VM deployments?
- How would you convince skeptical management to invest in Kubernetes training and migration?

**Preparation for Next Week:**
Filip arrives Monday morning looking relieved but concerned: "Kubernetes is working great locally, but now we need to set up our servers properly. Each server has different configurations, no standard way to provision new ones, and I'm worried about reproducibility. How do we ensure our production Kubernetes clusters are set up consistently?" The Infrastructure as Code journey begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Kubernetes: Up and Running" by Kelsey Hightower - Comprehensive K8s guide
- "The Kubernetes Book" by Nigel Poulton - Practical Kubernetes implementation

**Hands-on Practice:**
- Deploy your personal projects to local Kubernetes clusters
- Practice with Kubernetes interactive tutorials (katacoda.com)
- Experiment with different Kubernetes deployment strategies

**Industry Insights:**
- "State of Kubernetes" reports - Adoption trends and challenges
- CNCF surveys - Cloud-native technology adoption patterns
- Croatian Kubernetes meetups - Network with local K8s practitioners

**Advanced Topics:**
- Kubernetes security and RBAC (Role-Based Access Control)
- Helm package manager for Kubernetes applications
- Kubernetes monitoring with Prometheus and Grafana
- Service mesh architectures (Istio, Linkerd)

**Croatian Companies Using Kubernetes:**
- **Infobip:** Large-scale microservices platform on Kubernetes
- **Rimac:** Automotive applications and edge computing with K8s
- **Span:** Web application platform fully containerized
- **Photomath:** Mobile backend services orchestrated with Kubernetes
- **Include:** Financial services platform running on managed Kubernetes

**Certification Path:**
- **CKA (Certified Kubernetes Administrator):** Infrastructure-focused certification
- **CKAD (Certified Kubernetes Application Developer):** Development-focused certification
- **CKS (Certified Kubernetes Security Specialist):** Security-focused certification

---

*Next up: Week 7 - Infrastructure Provisioning: "From Snowflakes to Consistency" →*

> *"Kubernetes doesn't just orchestrate containers - it orchestrates possibilities. When infrastructure becomes code, dreams become deployments."* - Container Orchestration Philosophy

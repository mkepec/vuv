# Week 10: Kubernetes Networking + Service Discovery - "Connecting the Microservices Ecosystem" ☸️
**VelocityTech Transformation Week 10**

**Story Context:** Secure deployments work perfectly in isolation, but services can't communicate reliably and network troubleshooting is a nightmare  
**Focus:** Implementing robust Kubernetes networking with service discovery, load balancing, and network security  
**Characters in Focus:** Ana's inter-service testing chaos, Filip's network debugging nightmares, Luka's service communication confusion

---

## 🔥 The Weekly Crisis: "The Great Service Discovery Disaster"
**Setting:** VelocityTech office, Wednesday 11:15 AM. Multiple services deployed securely but unable to find each other.  
**The Problem:** Microservices architecture with perfect security but broken communication - services failing to discover and connect to each other

**Scene:** [VelocityTech main floor. Ana staring at failed integration tests showing connection timeouts. Filip tracing network routes through multiple terminals. Luka confused why his API calls work locally but fail in Kubernetes. Maja fielding angry calls from users experiencing intermittent failures.]

**ANA** *(frantically testing service endpoints)*  
> "Payment service can't reach the user authentication service... sometimes!"  
> "Order processing works for 5 minutes, then fails for 2 minutes, then works again..."  
> *(frustrated)* "How am I supposed to test integration when services randomly can't find each other?"

**FILIP** *(deep in kubectl commands and network diagnostics)*  
> "Pod IP addresses keep changing when containers restart..."  
> "The payment service is trying to connect to 10.244.1.15, but that IP doesn't exist anymore!"  
> *(checking DNS resolution)* "Sometimes DNS resolves, sometimes it doesn't. It's completely inconsistent!"

**LUKA** *(debugging API connection failures)*  
> "My authentication API calls work perfectly when I test locally..."  
> "But in Kubernetes, I get 'connection refused' errors about 30% of the time..."  
> *(confused)* "The payment service pod logs show it's trying to connect to 'auth-service' but getting no response!"

**MAJA** *(ending a frustrating customer call)*  
> "Users are reporting that payments work sometimes but fail other times..."  
> "Customer tried to buy something, payment failed, tried again 2 minutes later and it worked!"  
> *(stressed)* "This intermittent behavior is destroying our reputation!"

**FILIP** *(checking service configurations)*  
> "We have three payment service pods, but traffic isn't being distributed evenly..."  
> "One pod gets 80% of the traffic, two pods are idle, and the overloaded pod keeps crashing!"  
> *(overwhelmed)* "And when it crashes, users get errors until Kubernetes starts a replacement!"

**ANA** *(looking at external access issues)*  
> "External users can't even reach our services consistently..."  
> "The load balancer sometimes works, sometimes returns 502 errors..."  
> *(checking ingress logs)* "I see '503 Service Temporarily Unavailable' but I don't know why!"

**LUKA** *(frustrated with hardcoded connections)*  
> "I hardcoded the database service IP address because DNS wasn't working..."  
> "Now the database pod restarted and got a new IP, so everything's broken again!"  
> "There has to be a better way than constantly updating IP addresses in code!"

**MARKO** *(entering, immediately recognizing the networking chaos)*  
> "Let me guess - microservices are deployed but they can't reliably communicate with each other?"  
> *(to you)* "This is exactly why Kubernetes networking and service discovery exist. Ready to connect the ecosystem?"

**FILIP** *(desperate)*  
> "I understand containers and deployments, but Kubernetes networking is a black box!"  
> "Pods, Services, Ingress, DNS... how does it all work together?"

**MAJA** *(calculating business impact)*  
> "We're losing customers because of these intermittent failures..."  
> "If services can't communicate reliably, our microservices architecture is useless!"

**[All eyes turn to you - the intern who might understand modern networking practices.]**

**ANA** *(pleading)*  
> "Please tell me there's a way to make service communication reliable and predictable!"  
> "Something that handles service discovery automatically?"

**[Your systems thinking activates. This isn't just about network configuration - it's about building a reliable service mesh with proper traffic management.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Design robust Kubernetes networking architectures** - Implement Services, Ingress controllers, and DNS-based service discovery that work reliably at scale
2. **Configure advanced traffic management** - Set up load balancing, traffic routing, and network policies for secure microservices communication
3. **Troubleshoot network connectivity issues** - Diagnose and resolve service discovery, DNS, and connectivity problems in Kubernetes environments

**Real-world Connection:** Kubernetes networking expertise is critical for microservices architectures. Croatian companies like Infobip, Rimac, and Span rely on sophisticated networking configurations to manage hundreds of services. Advanced Kubernetes networking skills are essential for senior platform engineer and cloud architect roles.

---

## 📚 The Knowledge Foundation

### Kubernetes Networking Concepts and Architecture
**The Problem it Solves:** Service discovery failures, unreliable communication, network complexity, manual IP management, traffic routing challenges  
**How it Works:** Kubernetes provides multiple networking abstractions that enable reliable service-to-service communication regardless of pod lifecycle

**Kubernetes Networking Model:**
```
┌─────────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster Network                   │
├─────────────────────────────────────────────────────────────────┤
│  Node 1                           Node 2                        │
│  ┌─────────────────────────┐     ┌─────────────────────────┐    │
│  │ Pod Network             │     │ Pod Network             │    │
│  │ 10.244.1.0/24          │     │ 10.244.2.0/24          │    │
│  │ ┌─────┐ ┌─────┐        │     │ ┌─────┐ ┌─────┐        │    │
│  │ │Pod A│ │Pod B│        │     │ │Pod C│ │Pod D│        │    │
│  │ │10.1 │ │10.2 │        │     │ │10.3 │ │10.4 │        │    │
│  │ └─────┘ └─────┘        │     │ └─────┘ └─────┘        │    │
│  └─────────────────────────┘     └─────────────────────────┘    │
│           │                               │                     │
│      ┌────▼────┐                     ┌────▼────┐               │
│      │  Node   │◄────────────────────►│  Node   │               │
│      │Network  │     Cluster Network  │Network  │               │
│      │Interface│                      │Interface│               │
│      └─────────┘                      └─────────┘               │
└─────────────────────────────────────────────────────────────────┘
```

**Core Networking Principles:**
- **Every Pod gets its own IP:** Pods can communicate directly without NAT
- **Pod-to-Pod communication:** Any pod can talk to any other pod across nodes
- **Service abstraction:** Stable endpoints for groups of pods
- **DNS-based discovery:** Services discoverable by name, not IP
- **Network policies:** Microsegmentation and traffic control

**Kubernetes Networking Components:**
- **CNI (Container Network Interface):** Manages pod networking (Calico, Flannel, Weave)
- **kube-proxy:** Handles service traffic routing and load balancing
- **CoreDNS:** Provides DNS-based service discovery
- **Ingress Controllers:** Manage external access to services
- **Network Policies:** Control traffic flow between pods

**Real-world Example:** Spotify runs 1000+ microservices with reliable service discovery through Kubernetes networking  
**VelocityTech Connection:** Proper networking would eliminate service communication failures and provide consistent connectivity

### Services, Ingress Controllers, and Load Balancing
**The Problem it Solves:** Pod IP instability, manual load balancing, external access complexity, traffic routing challenges  
**How it Works:** Kubernetes Services provide stable networking endpoints while Ingress controllers manage external traffic routing

**Service Types and Use Cases:**
```yaml
# ClusterIP - Internal service communication
apiVersion: v1
kind: Service
metadata:
  name: payment-service
spec:
  type: ClusterIP
  selector:
    app: payment-gateway
  ports:
  - port: 80
    targetPort: 3000
---
# NodePort - External access via node ports
apiVersion: v1
kind: Service
metadata:
  name: payment-external
spec:
  type: NodePort
  selector:
    app: payment-gateway
  ports:
  - port: 80
    targetPort: 3000
    nodePort: 30080
---
# LoadBalancer - Cloud provider load balancer
apiVersion: v1
kind: Service
metadata:
  name: payment-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: payment-gateway
  ports:
  - port: 80
    targetPort: 3000
```

**Advanced Service Features:**
```yaml
# Service with session affinity and custom load balancing
apiVersion: v1
kind: Service
metadata:
  name: payment-service-advanced
spec:
  type: ClusterIP
  selector:
    app: payment-gateway
  ports:
  - port: 80
    targetPort: 3000
  sessionAffinity: ClientIP
  sessionAffinityConfig:
    clientIP:
      timeoutSeconds: 3600
```

**Ingress Controllers for Advanced Routing:**
```yaml
# HTTP/HTTPS routing with path-based rules
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: velocitytech-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/rate-limit: "100"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  tls:
  - hosts:
    - api.velocitytech.com
    secretName: velocitytech-tls
  rules:
  - host: api.velocitytech.com
    http:
      paths:
      - path: /payments
        pathType: Prefix
        backend:
          service:
            name: payment-service
            port:
              number: 80
      - path: /auth
        pathType: Prefix
        backend:
          service:
            name: auth-service
            port:
              number: 80
      - path: /orders
        pathType: Prefix
        backend:
          service:
            name: order-service
            port:
              number: 80
```

**Load Balancing Strategies:**
- **Round Robin:** Default - distributes requests evenly
- **Least Connections:** Routes to pod with fewest active connections
- **IP Hash:** Consistent routing based on client IP
- **Weighted:** Different traffic percentages to different pods

**Real-world Example:** Netflix uses sophisticated Ingress configurations to route millions of requests across their microservices  
**VelocityTech Connection:** Proper service configuration would provide stable endpoints and eliminate connection failures

### DNS and Service Discovery Mechanisms
**The Problem it Solves:** Hardcoded service endpoints, service location complexity, dynamic service registration  
**How it Works:** Kubernetes DNS automatically registers services and provides name-based service discovery

**DNS Resolution Hierarchy:**
```
Service Discovery Resolution Order:
1. <service-name>                    → Same namespace
2. <service-name>.<namespace>        → Specific namespace  
3. <service-name>.<namespace>.svc    → Full service name
4. <service-name>.<namespace>.svc.cluster.local → FQDN

Examples:
- payment-service                    → payment-service.default.svc.cluster.local
- auth-service.production           → auth-service.production.svc.cluster.local
- database.data-tier.svc            → database.data-tier.svc.cluster.local
```

**Service Discovery Patterns:**
```yaml
# Environment-based service discovery
apiVersion: v1
kind: ConfigMap
metadata:
  name: service-config
data:
  PAYMENT_SERVICE_URL: "http://payment-service:80"
  AUTH_SERVICE_URL: "http://auth-service.auth:80"
  DATABASE_URL: "postgresql://postgres-service.data:5432"
---
# Application using service discovery
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  template:
    spec:
      containers:
      - name: order-service
        image: velocitytech/order-service:latest
        envFrom:
        - configMapRef:
            name: service-config
```

**Advanced DNS Configuration:**
```yaml
# Custom DNS configuration
apiVersion: v1
kind: Pod
metadata:
  name: custom-dns-pod
spec:
  dnsPolicy: "None"
  dnsConfig:
    nameservers:
    - 10.96.0.10  # CoreDNS service IP
    searches:
    - production.svc.cluster.local
    - svc.cluster.local
    - cluster.local
    options:
    - name: ndots
      value: "2"
```

**Service Discovery Best Practices:**
- **Use service names, not IPs:** Services provide stable DNS names
- **Namespace organization:** Group related services in namespaces
- **Health checks:** Services only route to healthy pods
- **Circuit breakers:** Handle service failures gracefully
- **Service mesh:** Advanced traffic management and observability

**Real-world Example:** Uber's service discovery handles millions of service-to-service calls with sub-millisecond resolution  
**VelocityTech Connection:** DNS-based discovery would eliminate hardcoded IPs and provide automatic service registration

**Key Takeaways:**
- Kubernetes networking provides reliable pod-to-pod communication
- Services offer stable endpoints regardless of pod lifecycle
- Ingress controllers manage external traffic with advanced routing
- DNS-based service discovery eliminates hardcoded service locations

---

## 🛠️ Lab Mission: "The Kubernetes Networking Mastery"
**Scenario:** VelocityTech's microservices can't communicate reliably, causing intermittent failures and user frustration  
**Your Mission:** Implement comprehensive Kubernetes networking with reliable service discovery and traffic management  
**Success Criteria:** All services communicate reliably with proper load balancing and external access

### Phase 1: Service Discovery and Internal Networking (75 minutes)
**Objective:** Establish reliable service-to-service communication using Kubernetes Services and DNS

**Steps:**
1. **Deploy Multi-Service Application Stack**
   ```bash
   # Create comprehensive application namespace
   kubectl create namespace velocitytech-prod
   kubectl config set-context --current --namespace=velocitytech-prod
   
   # Create authentication service
   cat > auth-service.yml << 'EOF'
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: auth-service
     labels:
       app: auth-service
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: auth-service
     template:
       metadata:
         labels:
           app: auth-service
           version: v1
       spec:
         containers:
         - name: auth-service
           image: nginx:alpine
           ports:
           - containerPort: 80
           env:
           - name: SERVICE_NAME
             value: "auth-service"
           - name: SERVICE_VERSION
             value: "v1.0"
           volumeMounts:
           - name: config
             mountPath: /etc/nginx/conf.d
         volumes:
         - name: config
           configMap:
             name: auth-service-config
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: auth-service
     labels:
       app: auth-service
   spec:
     type: ClusterIP
     selector:
       app: auth-service
     ports:
     - name: http
       port: 80
       targetPort: 80
   ---
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: auth-service-config
   data:
     default.conf: |
       server {
           listen 80;
           server_name auth-service;
           
           location /health {
               return 200 '{"service":"auth-service","status":"healthy","version":"v1.0"}';
               add_header Content-Type application/json;
           }
           
           location /auth {
               return 200 '{"service":"auth-service","action":"authenticate","timestamp":"'$(date -u +"%Y-%m-%dT%H:%M:%SZ")'"}';
               add_header Content-Type application/json;
           }
       }
   EOF
   
   # Create payment service
   cat > payment-service.yml << 'EOF'
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-service
     labels:
       app: payment-service
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: payment-service
     template:
       metadata:
         labels:
           app: payment-service
           version: v1
       spec:
         containers:
         - name: payment-service
           image: nginx:alpine
           ports:
           - containerPort: 80
           env:
           - name: SERVICE_NAME
             value: "payment-service"
           - name: AUTH_SERVICE_URL
             value: "http://auth-service:80"
           volumeMounts:
           - name: config
             mountPath: /etc/nginx/conf.d
         volumes:
         - name: config
           configMap:
             name: payment-service-config
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-service
     labels:
       app: payment-service
   spec:
     type: ClusterIP
     selector:
       app: payment-service
     ports:
     - name: http
       port: 80
       targetPort: 80
   ---
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: payment-service-config
   data:
     default.conf: |
       server {
           listen 80;
           server_name payment-service;
           
           location /health {
               return 200 '{"service":"payment-service","status":"healthy","auth_service":"http://auth-service:80"}';
               add_header Content-Type application/json;
           }
           
           location /payment {
               return 200 '{"service":"payment-service","action":"process_payment","auth_check":"verified"}';
               add_header Content-Type application/json;
           }
       }
   EOF
   
   # Create order service that depends on both auth and payment
   cat > order-service.yml << 'EOF'
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: order-service
     labels:
       app: order-service
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: order-service
     template:
       metadata:
         labels:
           app: order-service
           version: v1
       spec:
         containers:
         - name: order-service
           image: curlimages/curl:latest
           command: ["/bin/sh"]
           args:
           - -c
           - |
             echo "Starting order service..."
             while true; do
               echo "$(date): Order service running, testing dependencies..."
               curl -s http://auth-service/health || echo "Auth service unreachable"
               curl -s http://payment-service/health || echo "Payment service unreachable"
               sleep 30
             done
           env:
           - name: AUTH_SERVICE_URL
             value: "http://auth-service:80"
           - name: PAYMENT_SERVICE_URL
             value: "http://payment-service:80"
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: order-service
     labels:
       app: order-service
   spec:
     type: ClusterIP
     selector:
       app: order-service
     ports:
     - name: http
       port: 80
       targetPort: 80
   EOF
   ```

2. **Deploy Services and Test Basic Connectivity**
   ```bash
   # Deploy all services
   kubectl apply -f auth-service.yml
   kubectl apply -f payment-service.yml
   kubectl apply -f order-service.yml
   
   # Wait for deployments to be ready
   kubectl wait --for=condition=available --timeout=300s deployment/auth-service
   kubectl wait --for=condition=available --timeout=300s deployment/payment-service
   kubectl wait --for=condition=available --timeout=300s deployment/order-service
   
   # Verify all pods are running
   kubectl get pods -o wide
   
   # Test service discovery
   echo "=== Testing Service Discovery ==="
   kubectl run test-pod --image=curlimages/curl --rm -it --restart=Never -- \
     curl -s http://auth-service/health
   
   kubectl run test-pod --image=curlimages/curl --rm -it --restart=Never -- \
     curl -s http://payment-service/health
   
   # Test DNS resolution
   kubectl run test-pod --image=busybox --rm -it --restart=Never -- \
     nslookup auth-service
   
   kubectl run test-pod --image=busybox --rm -it --restart=Never -- \
     nslookup payment-service.velocitytech-prod.svc.cluster.local
   ```

3. **Implement Advanced Service Discovery Patterns**
   ```yaml
   # Create service discovery configuration
   cat > service-discovery-config.yml << 'EOF'
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: service-discovery-config
   data:
     # Service endpoints for internal communication
     services.yaml: |
       services:
         auth:
           url: "http://auth-service:80"
           health_endpoint: "/health"
           timeout: 5s
         payment:
           url: "http://payment-service:80"
           health_endpoint: "/health"
           timeout: 10s
         database:
           url: "postgresql://postgres-service:5432"
           health_endpoint: "/health"
           timeout: 15s
       
       discovery:
         dns_suffix: ".svc.cluster.local"
         namespace: "velocitytech-prod"
         refresh_interval: 30s
   EOF
   
   kubectl apply -f service-discovery-config.yml
   ```

4. **Test Service Communication and Load Balancing**
   ```bash
   # Test service-to-service communication
   echo "=== Testing Service Communication ==="
   
   # Check order service logs to see dependency checks
   kubectl logs -l app=order-service --tail=10
   
   # Test load balancing by scaling and watching traffic distribution
   kubectl scale deployment payment-service --replicas=5
   kubectl wait --for=condition=available --timeout=300s deployment/payment-service
   
   # Test load balancing with multiple requests
   for i in {1..10}; do
     kubectl run test-pod-$i --image=curlimages/curl --rm --restart=Never -- \
       curl -s http://payment-service/health &
   done
   wait
   
   # Check which pods handled the requests
   kubectl logs -l app=payment-service --tail=20
   
   # Test service discovery across namespaces
   kubectl create namespace external-services
   
   kubectl run test-cross-namespace --image=curlimages/curl --rm -it --restart=Never -n external-services -- \
     curl -s http://auth-service.velocitytech-prod:80/health
   ```

**Checkpoint:** All services communicate reliably using DNS-based service discovery

### Phase 2: External Access and Ingress Configuration (90 minutes)
**Objective:** Configure external access to services using Ingress controllers with advanced routing

**Steps:**
1. **Install and Configure Ingress Controller**
   ```bash
   # Install NGINX Ingress Controller
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
   
   # Wait for ingress controller to be ready
   kubectl wait --namespace ingress-nginx \
     --for=condition=ready pod \
     --selector=app.kubernetes.io/component=controller \
     --timeout=300s
   
   # Verify ingress controller is running
   kubectl get pods -n ingress-nginx
   kubectl get svc -n ingress-nginx
   
   # Get ingress controller external IP (or use localhost for local testing)
   INGRESS_IP=$(kubectl get svc ingress-nginx-controller -n ingress-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
   if [ -z "$INGRESS_IP" ]; then
     INGRESS_IP="localhost"
     echo "Using localhost for ingress testing"
   fi
   echo "Ingress IP: $INGRESS_IP"
   ```

2. **Create Advanced Ingress Configuration**
   ```yaml
   # Create comprehensive ingress with path-based routing
   cat > velocitytech-ingress.yml << 'EOF'
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: velocitytech-api-ingress
     namespace: velocitytech-prod
     annotations:
       nginx.ingress.kubernetes.io/rewrite-target: /$2
       nginx.ingress.kubernetes.io/use-regex: "true"
       nginx.ingress.kubernetes.io/rate-limit: "100"
       nginx.ingress.kubernetes.io/rate-limit-window: "1m"
       nginx.ingress.kubernetes.io/cors-allow-origin: "*"
       nginx.ingress.kubernetes.io/cors-allow-methods: "GET, POST, PUT, DELETE, OPTIONS"
       nginx.ingress.kubernetes.io/enable-cors: "true"
   spec:
     ingressClassName: nginx
     rules:
     - host: api.velocitytech.local
       http:
         paths:
         - path: /api/auth(/|$)(.*)
           pathType: Prefix
           backend:
             service:
               name: auth-service
               port:
                 number: 80
         - path: /api/payment(/|$)(.*)
           pathType: Prefix
           backend:
             service:
               name: payment-service
               port:
                 number: 80
         - path: /api/orders(/|$)(.*)
           pathType: Prefix
           backend:
             service:
               name: order-service
               port:
                 number: 80
         - path: /(.*)
           pathType: Prefix
           backend:
             service:
               name: auth-service
               port:
                 number: 80
   ---
   # Create separate ingress for admin interface
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: velocitytech-admin-ingress
     namespace: velocitytech-prod
     annotations:
       nginx.ingress.kubernetes.io/auth-type: basic
       nginx.ingress.kubernetes.io/auth-secret: admin-auth
       nginx.ingress.kubernetes.io/auth-realm: 'VelocityTech Admin Area'
   spec:
     ingressClassName: nginx
     rules:
     - host: admin.velocitytech.local
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: auth-service
               port:
                 number: 80
   EOF
   
   # Create basic auth secret for admin interface
   kubectl create secret generic admin-auth --from-literal=auth="admin:$(openssl passwd -6 -quiet admin123)"
   
   kubectl apply -f velocitytech-ingress.yml
   ```

3. **Configure Advanced Traffic Management**
   ```yaml
   # Create canary deployment ingress
   cat > canary-ingress.yml << 'EOF'
   # Production traffic (90%)
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: payment-service-prod
     namespace: velocitytech-prod
     annotations:
       nginx.ingress.kubernetes.io/canary: "false"
   spec:
     ingressClassName: nginx
     rules:
     - host: payment.velocitytech.local
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: payment-service
               port:
                 number: 80
   ---
   # Canary traffic (10%)
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: payment-service-canary
     namespace: velocitytech-prod
     annotations:
       nginx.ingress.kubernetes.io/canary: "true"
       nginx.ingress.kubernetes.io/canary-weight: "10"
   spec:
     ingressClassName: nginx
     rules:
     - host: payment.velocitytech.local
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: payment-service-canary
               port:
                 number: 80
   EOF
   
   # Create canary version of payment service
   kubectl patch deployment payment-service -p '{"spec":{"template":{"metadata":{"labels":{"version":"stable"}}}}}'
   
   cat > payment-service-canary.yml << 'EOF'
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: payment-service-canary
     labels:
       app: payment-service-canary
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: payment-service-canary
     template:
       metadata:
         labels:
           app: payment-service-canary
           version: canary
       spec:
         containers:
         - name: payment-service
           image: nginx:alpine
           ports:
           - containerPort: 80
           volumeMounts:
           - name: config
             mountPath: /etc/nginx/conf.d
         volumes:
         - name: config
           configMap:
             name: payment-service-canary-config
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-service-canary
     labels:
       app: payment-service-canary
   spec:
     type: ClusterIP
     selector:
       app: payment-service-canary
     ports:
     - name: http
       port: 80
       targetPort: 80
   ---
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: payment-service-canary-config
   data:
     default.conf: |
       server {
           listen 80;
           server_name payment-service-canary;
           
           location /health {
               return 200 '{"service":"payment-service","version":"canary-v1.1","status":"healthy"}';
               add_header Content-Type application/json;
           }
           
           location /payment {
               return 200 '{"service":"payment-service","version":"canary-v1.1","action":"process_payment","features":["enhanced_validation"]}';
               add_header Content-Type application/json;
           }
       }
   EOF
   
   kubectl apply -f payment-service-canary.yml
   kubectl apply -f canary-ingress.yml
   ```

4. **Test External Access and Routing**
   ```bash
   # Add hostnames to /etc/hosts for local testing (if using localhost)
   if [ "$INGRESS_IP" = "localhost" ]; then
     echo "127.0.0.1 api.velocitytech.local admin.velocitytech.local payment.velocitytech.local" | sudo tee -a /etc/hosts
   fi
   
   # Test basic ingress routing
   echo "=== Testing Ingress Routing ==="
   
   # Test API routing
   curl -H "Host: api.velocitytech.local" http://$INGRESS_IP/api/auth/health
   curl -H "Host: api.velocitytech.local" http://$INGRESS_IP/api/payment/health
   
   # Test admin interface (should require authentication)
   curl -H "Host: admin.velocitytech.local" http://$INGRESS_IP/health
   curl -u admin:admin123 -H "Host: admin.velocitytech.local" http://$INGRESS_IP/health
   
   # Test canary routing (should get mix of stable and canary responses)
   echo "=== Testing Canary Routing ==="
   for i in {1..20}; do
     response=$(curl -s -H "Host: payment.velocitytech.local" http://$INGRESS_IP/health)
     echo "Request $i: $response" | grep -o '"version":"[^"]*"'
   done
   
   # Monitor ingress controller logs
   kubectl logs -n ingress-nginx -l app.kubernetes.io/component=controller --tail=10
   ```

**Checkpoint:** External services accessible through Ingress with advanced routing and traffic management

### Phase 3: Network Policies and Micro-segmentation (45 minutes)
**Objective:** Implement network security through Kubernetes Network Policies

**Steps:**
1. **Create Network Security Policies**
   ```yaml
   # Default deny all traffic policy
   cat > network-policies.yml << 'EOF'
   # Default deny all ingress traffic
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: default-deny-ingress
     namespace: velocitytech-prod
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
   ---
   # Allow auth service to be accessed by payment and order services
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-auth-service-access
     namespace: velocitytech-prod
   spec:
     podSelector:
       matchLabels:
         app: auth-service
     policyTypes:
     - Ingress
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: payment-service
       - podSelector:
           matchLabels:
             app: order-service
       - namespaceSelector:
           matchLabels:
             name: ingress-nginx
       ports:
       - protocol: TCP
         port: 80
   ---
   # Allow payment service to be accessed by order service and external traffic
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-payment-service-access
     namespace: velocitytech-prod
   spec:
     podSelector:
       matchLabels:
         app: payment-service
     policyTypes:
     - Ingress
     - Egress
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: order-service
       - namespaceSelector:
           matchLabels:
             name: ingress-nginx
       ports:
       - protocol: TCP
         port: 80
     egress:
     - to:
       - podSelector:
           matchLabels:
             app: auth-service
       ports:
       - protocol: TCP
         port: 80
     - to: [] # Allow DNS resolution
       ports:
       - protocol: UDP
         port: 53
   ---
   # Allow order service egress to payment and auth services
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-order-service-egress
     namespace: velocitytech-prod
   spec:
     podSelector:
       matchLabels:
         app: order-service
     policyTypes:
     - Egress
     egress:
     - to:
       - podSelector:
           matchLabels:
             app: payment-service
       - podSelector:
           matchLabels:
             app: auth-service
       ports:
       - protocol: TCP
         port: 80
     - to: [] # Allow DNS resolution
       ports:
       - protocol: UDP
         port: 53
   ---
   # Allow ingress controller access to all services
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: allow-ingress-controller
     namespace: velocitytech-prod
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     ingress:
     - from:
       - namespaceSelector:
           matchLabels:
             name: ingress-nginx
   EOF
   
   # Label ingress-nginx namespace for network policies
   kubectl label namespace ingress-nginx name=ingress-nginx
   
   kubectl apply -f network-policies.yml
   ```

2. **Test Network Policy Enforcement**
   ```bash
   # Test that network policies are working
   echo "=== Testing Network Policy Enforcement ==="
   
   # This should work - order service accessing auth service (allowed)
   kubectl exec -it deployment/order-service -- curl -s --connect-timeout 5 http://auth-service/health
   
   # This should work - payment service accessing auth service (allowed)
   kubectl run test-payment --image=curlimages/curl --rm -it --restart=Never \
     --labels="app=payment-service" -- curl -s --connect-timeout 5 http://auth-service/health
   
   # This should fail - random pod accessing auth service (not allowed)
   kubectl run test-unauthorized --image=curlimages/curl --rm -it --restart=Never \
     --labels="app=unauthorized" -- curl -s --connect-timeout 5 http://auth-service/health || echo "Connection blocked by network policy ✅"
   
   # Test external access still works
   curl -H "Host: api.velocitytech.local" http://$INGRESS_IP/api/auth/health
   
   # View network policy status
   kubectl get networkpolicies
   kubectl describe networkpolicy allow-auth-service-access
   ```

3. **Create Monitoring and Troubleshooting Tools**
   ```bash
   # Create network debugging toolkit
   cat > network-debug-toolkit.yml << 'EOF'
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: network-debug-toolkit
     namespace: velocitytech-prod
     labels:
       app: network-debug
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: network-debug
     template:
       metadata:
         labels:
           app: network-debug
       spec:
         containers:
         - name: debug-tools
           image: nicolaka/netshoot:latest
           command: ["/bin/bash"]
           args: ["-c", "while true; do sleep 3600; done"]
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: network-debug-service
     namespace: velocitytech-prod
   spec:
     selector:
       app: network-debug
     ports:
     - port: 80
       targetPort: 80
   EOF
   
   kubectl apply -f network-debug-toolkit.yml
   kubectl wait --for=condition=available --timeout=300s deployment/network-debug-toolkit
   
   # Network troubleshooting commands
   echo "=== Network Troubleshooting Toolkit ==="
   
   # DNS resolution testing
   kubectl exec -it deployment/network-debug-toolkit -- nslookup auth-service
   kubectl exec -it deployment/network-debug-toolkit -- nslookup payment-service.velocitytech-prod.svc.cluster.local
   
   # Network connectivity testing
   kubectl exec -it deployment/network-debug-toolkit -- curl -v http://auth-service/health
   kubectl exec -it deployment/network-debug-toolkit -- nc -zv auth-service 80
   
   # Check routes and network configuration
   kubectl exec -it deployment/network-debug-toolkit -- ip route
   kubectl exec -it deployment/network-debug-toolkit -- iptables -L
   ```

4. **Service Mesh Preparation (Advanced)**
   ```yaml
   # Prepare for service mesh with service monitoring
   cat > service-monitoring.yml << 'EOF'
   # Add labels for future service mesh integration
   apiVersion: v1
   kind: Service
   metadata:
     name: auth-service-monitored
     namespace: velocitytech-prod
     labels:
       app: auth-service
       version: v1
       monitor: "true"
   spec:
     type: ClusterIP
     selector:
       app: auth-service
     ports:
     - name: http
       port: 80
       targetPort: 80
   ---
   # ServiceMonitor for Prometheus (if available)
   apiVersion: monitoring.coreos.com/v1
   kind: ServiceMonitor
   metadata:
     name: velocitytech-services
     namespace: velocitytech-prod
   spec:
     selector:
       matchLabels:
         monitor: "true"
     endpoints:
     - port: http
       path: /health
       interval: 30s
   EOF
   
   # Apply monitoring configuration (will fail if Prometheus operator not installed)
   kubectl apply -f service-monitoring.yml || echo "ServiceMonitor requires Prometheus operator"
   ```

**Checkpoint:** Network policies enforcing micro-segmentation while maintaining service functionality

### Validation & Testing
**How to verify everything works:**
1. **Service Discovery Test** - `All services can find each other using DNS names`
2. **Load Balancing Test** - `Traffic distributed evenly across multiple replicas`
3. **External Access Test** - `Ingress controller routes traffic correctly to services`
4. **Network Policy Test** - `Unauthorized traffic blocked, authorized traffic allowed`
5. **DNS Resolution Test** - `Service names resolve correctly across namespaces`

**Expected Results:**
- **Service Reliability:** 99.9% service-to-service communication success rate
- **DNS Resolution:** Sub-millisecond service name resolution
- **External Access:** Multiple routing rules working with authentication
- **Network Security:** Micro-segmentation preventing unauthorized communication
- **Load Balancing:** Even traffic distribution across service replicas

---

## 🔧 New Tools in Your Arsenal

### Kubernetes Service Types and DNS
- **Purpose:** Provide stable networking endpoints and automatic service discovery for dynamic pod environments
- **Key Components:**
  - ClusterIP: Internal service communication within cluster
  - NodePort: External access via node ports (development/testing)
  - LoadBalancer: Cloud provider integration for production traffic
  - DNS: Automatic service registration and name resolution
- **Pro Tips:** Use ClusterIP for internal services, DNS names instead of IPs, health checks for reliability
- **Common Pitfalls:** Don't use NodePort in production, avoid hardcoded IPs, ensure proper service selectors

### Ingress Controllers and Traffic Management
- **Purpose:** Manage external HTTP/HTTPS traffic routing with advanced features like SSL termination and path-based routing
- **Key Features:**
  - Path-based routing: Route requests based on URL paths
  - Host-based routing: Multiple domains to different services
  - SSL/TLS termination: HTTPS handling at ingress layer
  - Rate limiting: Protect services from traffic spikes
- **Pro Tips:** Use annotations for advanced features, implement proper SSL certificates, monitor ingress logs
- **Common Pitfalls:** Don't expose internal services unnecessarily, configure proper timeouts, use meaningful hostnames

### Network Policies and Microsegmentation
- **Purpose:** Implement network security by controlling traffic flow between pods and namespaces
- **Key Concepts:**
  - Default deny: Block all traffic by default, allow explicitly
  - Ingress policies: Control incoming traffic to pods
  - Egress policies: Control outgoing traffic from pods
  - Label selectors: Target specific pods or namespaces
- **Pro Tips:** Start with simple policies, use meaningful labels, test policies thoroughly
- **Common Pitfalls:** Don't forget DNS access, avoid overly complex policies, monitor policy effects

---

## 🔧 Common Issues & Solutions

### Issue #1: "Services can't communicate intermittently"
**Symptoms:** Service discovery works sometimes but fails randomly, connection timeouts occur sporadically
**Cause:** DNS caching issues, service endpoint updates lag, or network policy blocking traffic
**Solution:**
```bash
# Check service endpoints
kubectl get endpoints auth-service
kubectl describe service auth-service

# Test DNS resolution
kubectl exec -it deployment/order-service -- nslookup auth-service

# Check network policies
kubectl get networkpolicies
kubectl describe networkpolicy allow-auth-service-access

# Force DNS cache refresh
kubectl exec -it deployment/order-service -- \
  sh -c "echo 'nameserver 8.8.8.8' > /etc/resolv.conf && curl http://auth-service/health"
```
**Prevention:** Use proper health checks, implement retry logic, monitor DNS resolution times

### Issue #2: "Ingress returns 502/503 errors intermittently"
**Symptoms:** External requests fail with 502 Bad Gateway or 503 Service Unavailable errors
**Cause:** Service pods not ready, ingress controller configuration issues, or backend service problems
**Solution:**
```bash
# Check ingress controller logs
kubectl logs -n ingress-nginx -l app.kubernetes.io/component=controller

# Verify service endpoints
kubectl get endpoints payment-service

# Test backend service directly
kubectl run test-backend --image=curlimages/curl --rm -it --restart=Never -- \
  curl -v http://payment-service/health

# Check ingress configuration
kubectl describe ingress velocitytech-api-ingress
```
**Prevention:** Implement proper readiness probes, configure ingress timeouts, monitor backend service health

### Issue #3: "Network policies block legitimate traffic"
**Symptoms:** Services that should communicate can't connect, even with seemingly correct network policies
**Cause:** Incorrect label selectors, missing DNS traffic rules, or policy conflicts
**Solution:**
```bash
# Verify pod labels match policy selectors
kubectl get pods --show-labels
kubectl describe networkpolicy allow-auth-service-access

# Check if DNS traffic is allowed
kubectl exec -it deployment/order-service -- nslookup auth-service

# Test with policy temporarily removed
kubectl delete networkpolicy allow-auth-service-access
# Test connectivity
kubectl apply -f network-policies.yml

# Debug with network tools
kubectl exec -it deployment/network-debug-toolkit -- tcpdump -i any host auth-service
```
**Prevention:** Test policies in staging first, use clear labeling strategy, allow DNS traffic explicitly

---

## 🎭 Character Evolution This Week

### Legacy Luka's Networking Enlightenment
**Monday:** "Why is networking so complicated in Kubernetes? I just want services to talk to each other!"
**Tuesday:** "Service discovery through DNS is actually elegant. No more hardcoded IP addresses in my code."
**Wednesday:** "Ingress controllers handle all the routing complexity I used to manage manually."
**Thursday:** "Network policies provide security without me having to configure firewalls on every server."
**Friday:** "Kubernetes networking isn't complex - it's comprehensive. Everything I need is built-in."
**Key Quote:** "Kubernetes networking solved problems I didn't even know I had."

### Anxious Ana's Service Testing Mastery
**Monday:** "How am I supposed to test services when networking is unreliable and unpredictable?"
**Tuesday:** "DNS-based service discovery means I can test against consistent service names, not changing IPs."
**Wednesday:** "Ingress routing lets me test different API versions and traffic patterns easily."
**Thursday:** "Network policies help me test security scenarios and service isolation."
**Friday:** "Reliable networking made integration testing predictable and comprehensive."
**Key Quote:** "Solid networking is the foundation of effective testing. Now I can test with confidence."

### Firefighter Filip's Infrastructure Transformation
**Monday:** "Kubernetes networking is a black box. When things break, I don't know where to start debugging."
**Tuesday:** "Service discovery eliminates most of the networking issues I used to troubleshoot manually."
**Wednesday:** "Ingress controllers handle load balancing, SSL, and routing - no more manual configuration!"
**Thursday:** "Network policies prevent the security incidents I used to respond to at 3 AM."
**Friday:** "My networking emergencies disappeared. The system is self-healing and secure."
**Key Quote:** "Kubernetes networking transformed me from a network firefighter to a network architect."

### Manager Maja's Business Connectivity Vision
**Monday:** "Complex networking sounds expensive and risky. Will this impact our service reliability?"
**Tuesday:** "Service discovery improves reliability while reducing manual configuration overhead."
**Wednesday:** "Ingress controllers enable sophisticated traffic management for A/B testing and gradual rollouts."
**Thursday:** "Network security through policies provides compliance benefits with minimal operational overhead."
**Friday:** "Robust networking infrastructure became a competitive advantage - reliable, secure, and scalable."
**Key Quote:** "Kubernetes networking isn't just infrastructure - it's business enablement through reliable connectivity."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 10 Improvements:**

| Metric | Week 9 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 8 days | 6 days | 2 hours |
| Service Communication Reliability | 70% | 99.9% | 99.99% |
| Network-Related Incidents | 5/week | 0/week | 0/week |
| Service Discovery Resolution Time | Manual lookup | <1ms | <1ms |
| External Service Availability | 95% | 99.8% | 99.9% |
| Network Security Coverage | 20% | 95% | 98% |
| Traffic Routing Complexity | Manual | Automated | Automated |

**Key Improvements:**
- **Service Reliability:** 99.9% service-to-service communication success rate
- **Network Incidents:** Eliminated all network-related service failures
- **External Access:** 99.8% availability for external services through Ingress
- **Security Enhancement:** 95% network traffic now governed by security policies

---

## 🇭🇷 Croatian IT Market Reality
Kubernetes networking expertise is highly valued in Croatian companies operating microservices architectures. Organizations like Infobip, Rimac, and Span rely on sophisticated networking configurations to manage hundreds of services. Advanced Kubernetes networking skills are essential for senior platform engineer, cloud architect, and site reliability engineer roles. Many Croatian companies struggle with service communication reliability, making networking expertise a significant competitive advantage.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about Kubernetes networking?** How service discovery and DNS eliminate most manual networking configuration
2. **Which networking feature had the biggest impact on reliability?** Service discovery providing stable endpoints regardless of pod changes
3. **How did network policies change your view of security?** Security can be implemented at the network layer without impacting application code
4. **What questions do you still have?** How to troubleshoot complex networking issues in production environments

**Team Discussion Points:**
- How would each VelocityTech character feel about implementing network policies that might initially break existing connections?
- What resistance might we encounter from teams comfortable with traditional networking approaches?
- How would you convince skeptical management to invest in advanced networking features?

**Preparation for Next Week:**
Filip arrives Monday morning relieved about reliable networking but concerned: "The services are communicating perfectly now, but I have no visibility into what's actually happening! When performance degrades, I'm debugging blind. We need comprehensive monitoring and metrics to understand system behavior." The monitoring and observability revolution begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Kubernetes Networking Concepts" - Official Kubernetes documentation
- "Container Networking" by Michael Hausenblas - Deep dive into container networking

**Hands-on Practice:**
- Set up complex Ingress routing scenarios with multiple applications
- Practice network policy troubleshooting with deliberate misconfigurations
- Experiment with service mesh technologies like Istio or Linkerd

**Industry Insights:**
- "State of Kubernetes Networking" reports - Adoption trends and challenges
- CNCF networking landscape - Overview of networking tools and projects
- Croatian Kubernetes meetups - Network with local platform engineers

**Advanced Topics:**
- Service mesh architectures (Istio, Linkerd, Consul Connect)
- Multi-cluster networking and federation
- Advanced traffic management and circuit breakers
- Network observability and monitoring

**Croatian Companies Using Advanced Kubernetes Networking:**
- **Infobip:** Global communication platform with complex microservices networking
- **Rimac:** Automotive systems requiring high-reliability service communication
- **Span:** Web platform with sophisticated traffic routing and API management
- **Photomath:** Mobile backend services with global traffic distribution
- **Include:** Financial services with strict network security requirements

**Certification Path:**
- **Certified Kubernetes Administrator (CKA)** - Includes networking troubleshooting
- **Certified Kubernetes Networking Associate (CKNA)** - Specialized networking certification
- **Istio Certified Associate** - Service mesh networking expertise

---

*Next up: Week 11 - Monitoring & Metrics: "Gaining Visibility Into the System" →*

> *"In Kubernetes, networking isn't just about connectivity - it's about creating a reliable, secure, and observable communication fabric that enables microservices to work as a cohesive system."* - Kubernetes Networking Philosophy

# Week 11: Monitoring & Metrics - "Gaining Visibility Into the System" 👁️
**VelocityTech Transformation Week 11**

**Story Context:** Production issues discovered by angry customers, no visibility into system performance or user behavior  
**Focus:** Implementing comprehensive observability with Prometheus, Grafana, and intelligent alerting  
**Characters in Focus:** Filip's transformation from reactive firefighter to proactive system architect, Ana's need for performance insights, Maja's demand for business metrics

---

## 🔥 The Weekly Crisis: "The Great Visibility Blackout"
**Setting:** VelocityTech office, Friday 8:15 AM. Filip's phone hasn't stopped ringing since 6 AM.  
**The Problem:** Major performance degradation discovered when angry customers start calling - team had no idea anything was wrong

**Scene:** [VelocityTech main floor. Filip juggling three phones with increasingly frustrated customers. Ana staring at application logs trying to piece together what happened. Luka confused why his code worked fine yesterday. Maja dealing with escalating client complaints and potential contract cancellations.]

**FILIP** *(hanging up from his fourth angry customer call)*  
> "Payment processing has been failing for THREE HOURS and we had no idea!"  
> "Customer says they've been trying to buy something since 5 AM and keep getting timeouts!"  
> *(frantically checking server logs)* "Why didn't we know about this? Our monitoring shows everything is 'green'!"

**ANA** *(scrolling through endless application logs)*  
> "I'm seeing error patterns here, but I have no idea when they started or how widespread they are..."  
> "Error rate could be 1% or 50% - I can't tell from individual log entries!"  
> *(frustrated)* "How am I supposed to understand system behavior from text files?"

**LUKA** *(checking his local development environment)*  
> "But everything works perfectly in development! Response times are under 100ms!"  
> "Production must be different somehow, but I can't see what's happening..."  
> *(defensive)* "My code is fine - it's probably infrastructure!"

**MAJA** *(ending a tense call with the CEO)*  
> "Three major clients are threatening to cancel contracts because of service reliability!"  
> "They want SLA guarantees, uptime reports, performance metrics..."  
> *(stressed)* "I have no data to show them! I don't even know how often this happens!"

**FILIP** *(diving deeper into server metrics)*  
> "CPU usage is at 12%, memory at 30%, disk space is fine..."  
> "According to basic metrics, everything looks normal!"  
> *(confused)* "But clearly something is very wrong. What am I missing?"

**ANA** *(trying to understand user impact)*  
> "I need to know: How many users are affected? Which features are broken? Is it getting worse?"  
> "But all I have are scattered log entries and customer complaints!"

**LUKA** *(looking at database queries)*  
> "Maybe it's a slow database query? But which one? And why now?"  
> "I can see the queries running, but I can't see response times or patterns..."

**MARKO** *(entering, immediately recognizing the crisis pattern)*  
> "Let me guess - production is broken, customers are angry, and you discovered it from complaints, not monitoring?"  
> *(to you)* "This is exactly why we need proper observability. Ready to make the invisible visible?"

**FILIP** *(desperate)*  
> "I feel like I'm flying blind! We need to see what's actually happening in production!"  
> "Not just 'is the server running' but 'how well is it serving users'!"

**MAJA** *(calculating business impact)*  
> "We need real-time visibility into system health and user experience!"  
> "Something that tells us about problems before customers do!"

**[All eyes turn to you - the intern who might understand modern observability practices.]**

**ANA** *(pleading)*  
> "Please tell me there's a way to see what's really happening in our systems!"  
> "Something that shows trends, patterns, and user impact - not just server status!"

**[Your systems thinking activates. This isn't just about monitoring - it's about building comprehensive observability into the system.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Design comprehensive monitoring strategies** - Implement Prometheus metrics collection with custom application instrumentation and alerting rules
2. **Create actionable dashboards and visualizations** - Build Grafana dashboards that provide meaningful insights into system health and user experience
3. **Establish SLA/SLO frameworks** - Define service level objectives and create monitoring that tracks business-critical metrics

**Real-world Connection:** Observability expertise is crucial for senior roles in Croatian IT companies. Organizations like Infobip, Rimac, and Span depend on sophisticated monitoring to maintain service reliability at scale. Advanced monitoring skills are essential for SRE, platform engineering, and DevOps leadership positions.

---

## 📚 The Knowledge Foundation

### Observability Pillars: Metrics, Logs, and Traces
**The Problem it Solves:** Reactive problem discovery, poor system visibility, inability to understand user impact, no performance insights  
**How it Works:** Three complementary data sources provide complete system understanding

**The Three Pillars of Observability:**
```
┌─────────────────────────────────────────────────────────────┐
│                 Complete System Visibility                   │
├─────────────────────────────────────────────────────────────┤
│  Metrics          │    Logs           │    Traces           │
│  ┌─────────────┐   │   ┌─────────────┐ │   ┌─────────────┐   │
│  │What happened│   │   │Why it       │ │   │How it       │   │
│  │When         │   │   │happened     │ │   │happened     │   │
│  │How much     │   │   │Context &    │ │   │Request path │   │
│  │Aggregated   │   │   │Details      │ │   │Dependencies │   │
│  └─────────────┘   │   └─────────────┘ │   └─────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Metrics - The "What" and "When":**
- **Numerical data over time** - CPU usage, request count, response time
- **Aggregated views** - Trends, averages, percentiles
- **Efficient storage** - Can store years of data efficiently
- **Alerting foundation** - Define thresholds for automated notifications

**Logs - The "Why" and "Context":**
- **Detailed event records** - Application behavior, errors, user actions
- **Rich context** - Stack traces, user IDs, transaction details
- **Searchable history** - Debug specific incidents
- **Structured data** - JSON logs enable powerful queries

**Traces - The "How" and "Flow":**
- **Request journey** - Track requests across multiple services
- **Performance bottlenecks** - Identify slow components
- **Dependency mapping** - Understand service interactions
- **Root cause analysis** - Follow problems to their source

**Real-world Example:** Netflix uses all three pillars to maintain 99.99% uptime across thousands of microservices  
**VelocityTech Connection:** Complete observability would have detected the performance issue hours before customers complained

### Prometheus Architecture and Data Model
**The Problem it Solves:** Complex metric collection, vendor lock-in, expensive monitoring solutions, difficult custom metrics  
**How it Works:** Pull-based monitoring system that scrapes metrics from applications and infrastructure

**Prometheus Core Components:**
```
┌─────────────────────────────────────────────────────────┐
│                Prometheus Ecosystem                      │
├─────────────────────────────────────────────────────────┤
│  Applications     │  Prometheus      │  Visualization   │
│  ┌─────────────┐  │  ┌─────────────┐ │  ┌─────────────┐ │
│  │/metrics     │←─┤  │Time Series  │ │  │  Grafana    │ │
│  │endpoint     │  │  │Database     │ │  │Dashboards   │ │
│  │(your app)   │  │  │PromQL       │─┤→ │ Alerts      │ │
│  └─────────────┘  │  │Query Engine │ │  │  API        │ │
│  ┌─────────────┐  │  └─────────────┘ │  └─────────────┘ │
│  │Node         │←─┤       ↑          │                  │
│  │Exporter     │  │  ┌─────────────┐ │  ┌─────────────┐ │
│  │(system)     │  │  │Alertmanager │ │  │ Slack/Email │ │
│  └─────────────┘  │  │Rules &      │─┤→ │Notifications│ │
│                   │  │Routing      │ │  │  & Webhooks │ │
│                   │  └─────────────┘ │  └─────────────┘ │
└─────────────────────────────────────────────────────────┘
```

**Prometheus Data Model:**
- **Time series** - Metric name + labels + timestamp + value
- **Labels** - Key-value pairs for filtering and grouping
- **Metric types** - Counter, Gauge, Histogram, Summary

**Example Metrics:**
```
# Counter - always increasing
http_requests_total{method="GET", status="200", service="payment-gateway"} 1547

# Gauge - can go up and down  
memory_usage_bytes{instance="web-server-1"} 536870912

# Histogram - buckets of measurements
http_request_duration_seconds_bucket{le="0.1", service="api"} 1000
http_request_duration_seconds_bucket{le="0.5", service="api"} 1500
http_request_duration_seconds_bucket{le="1.0", service="api"} 1800
```

**PromQL Query Language:**
```
# Average response time over 5 minutes
rate(http_request_duration_seconds_sum[5m]) / rate(http_request_duration_seconds_count[5m])

# Error rate percentage
rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) * 100

# 95th percentile response time
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
```

**Real-world Example:** SoundCloud open-sourced Prometheus, and it's now used by thousands of companies including Google, Red Hat, and DigitalOcean  
**VelocityTech Connection:** Prometheus would provide comprehensive application and infrastructure metrics

### Grafana Dashboards and SLA/SLO/SLI Frameworks
**The Problem it Solves:** Raw metrics are hard to interpret, no business context, reactive alerting, unclear service quality  
**How it Works:** Visual dashboards show meaningful patterns, SLOs define service quality expectations

**Service Level Concepts:**
- **SLI (Service Level Indicator)** - What you measure (response time, error rate, availability)
- **SLO (Service Level Objective)** - Target values for SLIs (99.9% uptime, <200ms response)
- **SLA (Service Level Agreement)** - Business contracts with consequences (credits, penalties)

**Effective Dashboard Design:**
```
┌─────────────────────────────────────────────────────────┐
│                Executive Dashboard                       │
├─────────────────────────────────────────────────────────┤
│  SLO Status    │  Business Impact  │  User Experience   │
│  ┌───────────┐ │  ┌─────────────┐  │  ┌─────────────┐   │
│  │ 99.97%    │ │  │ $45K/hour   │  │  │ 180ms avg   │   │
│  │ Uptime    │ │  │ Revenue     │  │  │ Response    │   │
│  │ ✅ Target │ │  │ Impact      │  │  │ ⚠️ Target   │   │
│  └───────────┘ │  └─────────────┘  │  └─────────────┘   │
├─────────────────────────────────────────────────────────┤
│                Technical Dashboard                       │
├─────────────────────────────────────────────────────────┤
│  Error Rates   │  Response Times   │  System Health     │
│  [Graph]       │  [Histogram]      │  [Heatmap]        │
└─────────────────────────────────────────────────────────┘
```

**Golden Signals for Monitoring:**
1. **Latency** - How long requests take
2. **Traffic** - How many requests you're getting
3. **Errors** - Rate of failed requests
4. **Saturation** - How "full" your service is

**Alerting Best Practices:**
- **Alert on symptoms, not causes** - Alert on user impact, not CPU usage
- **Reduce noise** - Only alert on actionable problems
- **Escalation policies** - Route alerts to appropriate teams
- **Runbooks** - Document response procedures

**Real-world Example:** Google's SRE practices define error budgets - if you exceed error thresholds, you focus on reliability instead of new features  
**VelocityTech Connection:** Well-designed dashboards would give Maja business metrics and Filip proactive alerts

**Key Takeaways:**
- Observability requires metrics, logs, and traces working together
- Prometheus provides powerful, flexible metric collection and querying
- Good dashboards show business impact, not just technical metrics
- SLOs define service quality in business terms

---

## 🛠️ Lab Mission: "The Visibility Revolution"
**Scenario:** VelocityTech discovers critical performance issues from customer complaints instead of monitoring  
**Your Mission:** Implement comprehensive observability stack with Prometheus, Grafana, and intelligent alerting  
**Success Criteria:** Complete system visibility with proactive alerting, business dashboards, and sub-minute problem detection

### Phase 1: Prometheus and Metrics Foundation (90 minutes)
**Objective:** Deploy Prometheus monitoring stack and instrument applications with custom metrics

**Steps:**
1. **Deploy Prometheus Stack to Kubernetes**
   ```bash
   # Create monitoring namespace
   kubectl create namespace monitoring
   
   # Add Prometheus Helm repository
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   
   # Install Prometheus stack with Grafana
   helm install prometheus prometheus-community/kube-prometheus-stack \
     --namespace monitoring \
     --set prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues=false \
     --set prometheus.prometheusSpec.retention=30d \
     --set grafana.adminPassword=velocitytechpass \
     --set grafana.service.type=NodePort \
     --set grafana.service.nodePort=30080 \
     --set prometheus.service.type=NodePort \
     --set prometheus.service.nodePort=30090
   
   # Wait for deployment to be ready
   kubectl wait --for=condition=available --timeout=300s \
     deployment/prometheus-kube-prometheus-grafana -n monitoring
   
   # Verify Prometheus is running
   kubectl get pods -n monitoring
   
   # Get Grafana URL (minikube)
   echo "Grafana URL: $(minikube service prometheus-kube-prometheus-grafana --url -n monitoring)"
   echo "Username: admin"
   echo "Password: velocitytechpass"
   
   # Get Prometheus URL
   echo "Prometheus URL: $(minikube service prometheus-kube-prometheus-prometheus --url -n monitoring)"
   ```
   - Expected output: Prometheus and Grafana pods running successfully
   - Troubleshooting: If pods don't start, check resource limits with `kubectl describe pod`

2. **Instrument Payment Gateway with Custom Metrics**
   ```javascript
   // Add to payment-gateway application
   // src/metrics.js
   const prometheus = require('prom-client');
   
   // Create custom metrics
   const httpRequestDuration = new prometheus.Histogram({
     name: 'http_request_duration_seconds',
     help: 'Duration of HTTP requests in seconds',
     labelNames: ['method', 'route', 'status'],
     buckets: [0.1, 0.3, 0.5, 0.7, 1, 3, 5, 7, 10]
   });
   
   const httpRequestsTotal = new prometheus.Counter({
     name: 'http_requests_total',
     help: 'Total number of HTTP requests',
     labelNames: ['method', 'route', 'status']
   });
   
   const paymentProcessingDuration = new prometheus.Histogram({
     name: 'payment_processing_duration_seconds',
     help: 'Duration of payment processing in seconds',
     labelNames: ['payment_method', 'status'],
     buckets: [0.1, 0.5, 1, 2, 5, 10, 30]
   });
   
   const paymentProcessingTotal = new prometheus.Counter({
     name: 'payment_processing_total',
     help: 'Total number of payment processing attempts',
     labelNames: ['payment_method', 'status']
   });
   
   const activeUsers = new prometheus.Gauge({
     name: 'active_users_current',
     help: 'Current number of active users'
   });
   
   const databaseConnectionPool = new prometheus.Gauge({
     name: 'database_connections_active',
     help: 'Number of active database connections'
   });
   
   // Business metrics
   const revenueTotal = new prometheus.Counter({
     name: 'revenue_total_euros',
     help: 'Total revenue processed in euros',
     labelNames: ['currency']
   });
   
   const orderValue = new prometheus.Histogram({
     name: 'order_value_euros',
     help: 'Distribution of order values in euros',
     buckets: [10, 25, 50, 100, 250, 500, 1000, 2500, 5000]
   });
   
   // Register all metrics
   prometheus.register.registerMetric(httpRequestDuration);
   prometheus.register.registerMetric(httpRequestsTotal);
   prometheus.register.registerMetric(paymentProcessingDuration);
   prometheus.register.registerMetric(paymentProcessingTotal);
   prometheus.register.registerMetric(activeUsers);
   prometheus.register.registerMetric(databaseConnectionPool);
   prometheus.register.registerMetric(revenueTotal);
   prometheus.register.registerMetric(orderValue);
   
   module.exports = {
     httpRequestDuration,
     httpRequestsTotal,
     paymentProcessingDuration,
     paymentProcessingTotal,
     activeUsers,
     databaseConnectionPool,
     revenueTotal,
     orderValue,
     register: prometheus.register
   };
   ```

3. **Add Metrics Middleware to Express Application**
   ```javascript
   // src/middleware/metrics.js
   const { httpRequestDuration, httpRequestsTotal } = require('../metrics');
   
   const metricsMiddleware = (req, res, next) => {
     const start = Date.now();
     
     // Track request start
     res.on('finish', () => {
       const duration = (Date.now() - start) / 1000;
       const route = req.route ? req.route.path : req.path;
       
       // Record metrics
       httpRequestDuration
         .labels(req.method, route, res.statusCode.toString())
         .observe(duration);
       
       httpRequestsTotal
         .labels(req.method, route, res.statusCode.toString())
         .inc();
     });
     
     next();
   };
   
   module.exports = metricsMiddleware;
   ```

   ```javascript
   // Update src/server.js to include metrics
   const express = require('express');
   const metricsMiddleware = require('./middleware/metrics');
   const { register } = require('./metrics');
   
   const app = express();
   
   // Add metrics middleware
   app.use(metricsMiddleware);
   
   // Metrics endpoint for Prometheus
   app.get('/metrics', async (req, res) => {
     res.set('Content-Type', register.contentType);
     res.end(await register.metrics());
   });
   
   // Health check endpoint
   app.get('/health', (req, res) => {
     res.json({ status: 'healthy', timestamp: new Date().toISOString() });
   });
   
   // Payment processing endpoint with metrics
   app.post('/api/payments/process', async (req, res) => {
     const start = Date.now();
     
     try {
       const { paymentProcessingDuration, paymentProcessingTotal, revenueTotal, orderValue } = require('./metrics');
       
       // Simulate payment processing
       const paymentResult = await processPayment(req.body);
       
       if (paymentResult.success) {
         // Record successful payment metrics
         paymentProcessingTotal.labels(paymentResult.method, 'success').inc();
         revenueTotal.labels('EUR').inc(paymentResult.amount);
         orderValue.observe(paymentResult.amount);
         
         const duration = (Date.now() - start) / 1000;
         paymentProcessingDuration.labels(paymentResult.method, 'success').observe(duration);
         
         res.json(paymentResult);
       } else {
         // Record failed payment metrics
         paymentProcessingTotal.labels(req.body.method || 'unknown', 'failure').inc();
         
         const duration = (Date.now() - start) / 1000;
         paymentProcessingDuration.labels(req.body.method || 'unknown', 'failure').observe(duration);
         
         res.status(400).json(paymentResult);
       }
     } catch (error) {
       // Record error metrics
       const { paymentProcessingTotal } = require('./metrics');
       paymentProcessingTotal.labels('unknown', 'error').inc();
       
       res.status(500).json({ success: false, error: error.message });
     }
   });
   
   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => {
     console.log(`Payment Gateway running on port ${PORT}`);
     console.log(`Metrics available at http://localhost:${PORT}/metrics`);
   });
   ```

4. **Create ServiceMonitor for Prometheus Discovery**
   ```yaml
   # Create monitoring/servicemonitor.yml
   apiVersion: monitoring.coreos.com/v1
   kind: ServiceMonitor
   metadata:
     name: payment-gateway-metrics
     namespace: monitoring
     labels:
       app: payment-gateway
   spec:
     selector:
       matchLabels:
         app: payment-gateway
     endpoints:
     - port: http
       path: /metrics
       interval: 30s
   ---
   # Update payment gateway service to include metrics labels
   apiVersion: v1
   kind: Service
   metadata:
     name: payment-gateway-service
     namespace: velocitytech
     labels:
       app: payment-gateway
   spec:
     selector:
       app: payment-gateway
     ports:
     - name: http
       port: 80
       targetPort: 3000
   ```

5. **Deploy Updated Application and Verify Metrics**
   ```bash
   # Update package.json dependencies
   npm install prom-client
   
   # Build and deploy updated application
   docker build -t velocitytech/payment-gateway:monitored .
   kubectl set image deployment/payment-gateway-deployment \
     payment-gateway=velocitytech/payment-gateway:monitored -n velocitytech
   
   # Apply ServiceMonitor
   kubectl apply -f monitoring/servicemonitor.yml
   
   # Wait for rollout
   kubectl rollout status deployment/payment-gateway-deployment -n velocitytech
   
   # Test metrics endpoint
   GATEWAY_URL=$(minikube service payment-gateway-service --url -n velocitytech)
   curl $GATEWAY_URL/metrics
   
   # Generate some test traffic
   for i in {1..50}; do
     curl -X POST $GATEWAY_URL/api/payments/process \
       -H "Content-Type: application/json" \
       -d '{
         "amount": '$((RANDOM % 500 + 10))',
         "method": "credit_card",
         "cardNumber": "4111111111111111"
       }' &
   done
   wait
   
   # Check Prometheus targets
   echo "Check Prometheus targets at: $(minikube service prometheus-kube-prometheus-prometheus --url -n monitoring)/targets"
   ```

**Checkpoint:** Prometheus collecting custom metrics from instrumented payment gateway

### Phase 2: Grafana Dashboards and Visualization (90 minutes)
**Objective:** Create comprehensive dashboards showing system health, business metrics, and user experience

**Steps:**
1. **Access Grafana and Import Basic Dashboards**
   ```bash
   # Get Grafana URL and login
   GRAFANA_URL=$(minikube service prometheus-kube-prometheus-grafana --url -n monitoring)
   echo "Grafana URL: $GRAFANA_URL"
   echo "Username: admin"
   echo "Password: velocitytechpass"
   
   # Open Grafana in browser
   # Login and verify Prometheus data source is configured
   # Go to Explore tab and test queries:
   # - up
   # - rate(http_requests_total[5m])
   # - histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
   ```

2. **Create Executive Business Dashboard**
   ```json
   // Create dashboard via Grafana UI or import this JSON
   {
     "dashboard": {
       "title": "VelocityTech Executive Dashboard",
       "tags": ["business", "executive"],
       "panels": [
         {
           "title": "Revenue Today",
           "type": "stat",
           "targets": [
             {
               "expr": "increase(revenue_total_euros[24h])",
               "legendFormat": "Revenue (EUR)"
             }
           ],
           "fieldConfig": {
             "defaults": {
               "unit": "currencyEUR",
               "color": {
                 "mode": "thresholds"
               },
               "thresholds": {
                 "steps": [
                   {"color": "red", "value": 0},
                   {"color": "yellow", "value": 5000},
                   {"color": "green", "value": 10000}
                 ]
               }
             }
           }
         },
         {
           "title": "Service Availability (SLO: 99.9%)",
           "type": "stat",
           "targets": [
             {
               "expr": "(1 - (rate(http_requests_total{status=~\"5..\"}[24h]) / rate(http_requests_total[24h]))) * 100",
               "legendFormat": "Availability %"
             }
           ],
           "fieldConfig": {
             "defaults": {
               "unit": "percent",
               "min": 99.5,
               "max": 100,
               "thresholds": {
                 "steps": [
                   {"color": "red", "value": 99.5},
                   {"color": "yellow", "value": 99.9},
                   {"color": "green", "value": 99.95}
                 ]
               }
             }
           }
         },
         {
           "title": "Average Response Time (SLO: <200ms)",
           "type": "stat",
           "targets": [
             {
               "expr": "histogram_quantile(0.50, rate(http_request_duration_seconds_bucket[5m])) * 1000",
               "legendFormat": "Avg Response Time"
             }
           ],
           "fieldConfig": {
             "defaults": {
               "unit": "ms",
               "thresholds": {
                 "steps": [
                   {"color": "green", "value": 0},
                   {"color": "yellow", "value": 200},
                   {"color": "red", "value": 500}
                 ]
               }
             }
           }
         }
       ]
     }
   }
   ```

3. **Create Technical Operations Dashboard**
   ```bash
   # Create comprehensive technical dashboard with panels for:
   # 1. Request Rate and Error Rate
   # 2. Response Time Percentiles  
   # 3. Payment Processing Metrics
   # 4. System Resource Usage
   # 5. Database Performance
   
   # Example queries to use in Grafana panels:
   
   # Request rate
   rate(http_requests_total[5m])
   
   # Error rate percentage
   (rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m])) * 100
   
   # Response time percentiles
   histogram_quantile(0.50, rate(http_request_duration_seconds_bucket[5m])) * 1000
   histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) * 1000
   histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m])) * 1000
   
   # Payment success rate
   rate(payment_processing_total{status="success"}[5m]) / rate(payment_processing_total[5m]) * 100
   
   # Payment processing time
   histogram_quantile(0.95, rate(payment_processing_duration_seconds_bucket[5m]))
   
   # Active users
   active_users_current
   
   # Database connections
   database_connections_active
   ```

4. **Create SLO Monitoring Dashboard**
   ```json
   // SLO Dashboard focusing on service level objectives
   {
     "dashboard": {
       "title": "VelocityTech SLO Monitoring",
       "panels": [
         {
           "title": "Error Budget Remaining (99.9% SLO)",
           "type": "stat",
           "targets": [
             {
               "expr": "((0.999 - (1 - rate(http_requests_total{status=~\"5..\"}[30d]) / rate(http_requests_total[30d]))) / 0.001) * 100",
               "legendFormat": "Error Budget %"
             }
           ],
           "fieldConfig": {
             "defaults": {
               "unit": "percent",
               "thresholds": {
                 "steps": [
                   {"color": "red", "value": 0},
                   {"color": "yellow", "value": 25},
                   {"color": "green", "value": 50}
                 ]
               }
             }
           }
         },
         {
           "title": "SLO Compliance Trend",
           "type": "graph",
           "targets": [
             {
               "expr": "(1 - (rate(http_requests_total{status=~\"5..\"}[1h]) / rate(http_requests_total[1h]))) * 100",
               "legendFormat": "Hourly Availability %"
             }
           ],
           "yAxes": [
             {
               "min": 99.5,
               "max": 100,
               "unit": "percent"
             }
           ],
           "thresholds": [
             {
               "value": 99.9,
               "colorMode": "critical",
               "op": "lt"
             }
           ]
         }
       ]
     }
   }
   ```

5. **Test Dashboard Functionality**
   ```bash
   # Generate realistic traffic patterns to test dashboards
   
   # Create load testing script
   cat > scripts/generate-traffic.sh << 'EOF'
   #!/bin/bash
   
   GATEWAY_URL=$(minikube service payment-gateway-service --url -n velocitytech)
   
   echo "Generating traffic to $GATEWAY_URL"
   
   # Generate successful payments
   for i in {1..100}; do
     curl -s -X POST $GATEWAY_URL/api/payments/process \
       -H "Content-Type: application/json" \
       -d '{
         "amount": '$((RANDOM % 500 + 10))',
         "method": "credit_card",
         "cardNumber": "4111111111111111"
       }' > /dev/null &
     
     # Add some delay
     sleep 0.1
   done
   
   # Generate some failures
   for i in {1..10}; do
     curl -s -X POST $GATEWAY_URL/api/payments/process \
       -H "Content-Type: application/json" \
       -d '{
         "amount": -100,
         "method": "credit_card",
         "cardNumber": "invalid"
       }' > /dev/null &
   done
   
   wait
   echo "Traffic generation completed"
   EOF
   
   chmod +x scripts/generate-traffic.sh
   
   # Run traffic generation
   ./scripts/generate-traffic.sh
   
   # Verify metrics are updating in Grafana dashboards
   echo "Check your dashboards in Grafana: $GRAFANA_URL"
   ```

**Checkpoint:** Complete Grafana dashboards showing business and technical metrics with real-time data

### Phase 3: Intelligent Alerting and Notifications (60 minutes)
**Objective:** Configure smart alerting rules that notify the team of actual problems before they impact users

**Steps:**
1. **Configure Alertmanager Rules**
   ```yaml
   # Create monitoring/alert-rules.yml
   apiVersion: monitoring.coreos.com/v1
   kind: PrometheusRule
   metadata:
     name: velocitytech-alerts
     namespace: monitoring
     labels:
       app: kube-prometheus-stack
       release: prometheus
   spec:
     groups:
     - name: velocitytech.payment.rules
       rules:
       - alert: HighErrorRate
         expr: (rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m])) * 100 > 5
         for: 2m
         labels:
           severity: critical
           team: backend
         annotations:
           summary: "High error rate detected"
           description: "Error rate is {{ $value }}% for {{ $labels.instance }}"
           runbook_url: "https://wiki.velocitytech.com/runbooks/high-error-rate"
       
       - alert: HighResponseTime
         expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 0.5
         for: 5m
         labels:
           severity: warning
           team: backend
         annotations:
           summary: "High response time detected"
           description: "95th percentile response time is {{ $value }}s"
       
       - alert: PaymentProcessingFailures
         expr: rate(payment_processing_total{status="failure"}[5m]) > 0.1
         for: 1m
         labels:
           severity: critical
           team: payments
         annotations:
           summary: "Payment processing failures detected"
           description: "Payment failure rate: {{ $value }} failures/second"
       
       - alert: SLOViolation
         expr: (1 - (rate(http_requests_total{status=~"5.."}[1h]) / rate(http_requests_total[1h]))) * 100 < 99.9
         for: 0m
         labels:
           severity: critical
           team: sre
         annotations:
           summary: "SLO violation - availability below 99.9%"
           description: "Current availability: {{ $value }}%"
       
       - alert: ErrorBudgetExhausted
         expr: ((0.999 - (1 - rate(http_requests_total{status=~"5.."}[30d]) / rate(http_requests_total[30d]))) / 0.001) * 100 < 10
         for: 0m
         labels:
           severity: warning
           team: sre
         annotations:
           summary: "Error budget critically low"
           description: "Only {{ $value }}% of error budget remaining"
       
       - alert: ServiceDown
         expr: up{job="payment-gateway-metrics"} == 0
         for: 1m
         labels:
           severity: critical
           team: platform
         annotations:
           summary: "Payment gateway service is down"
           description: "Payment gateway has been down for more than 1 minute"
   ```

2. **Configure Slack Notifications**
   ```yaml
   # Update Alertmanager configuration
   # Create monitoring/alertmanager-config.yml
   apiVersion: v1
   kind: Secret
   metadata:
     name: alertmanager-kube-prometheus-alertmanager
     namespace: monitoring
   stringData:
     alertmanager.yml: |
       global:
         slack_api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
       
       route:
         group_by: ['alertname', 'team']
         group_wait: 10s
         group_interval: 10s
         repeat_interval: 1h
         receiver: 'web.hook'
         routes:
         - match:
             severity: critical
           receiver: 'critical-alerts'
         - match:
             team: payments
           receiver: 'payments-team'
         - match:
             severity: warning
           receiver: 'warning-alerts'
       
       receivers:
       - name: 'web.hook'
         slack_configs:
         - channel: '#alerts'
           title: 'VelocityTech Alert'
           text: '{{ range .Alerts }}{{ .Annotations.summary }}\n{{ .Annotations.description }}{{ end }}'
       
       - name: 'critical-alerts'
         slack_configs:
         - channel: '#critical-alerts'
           title: '🚨 CRITICAL: {{ .GroupLabels.alertname }}'
           text: |
             {{ range .Alerts }}
             *Alert:* {{ .Annotations.summary }}
             *Description:* {{ .Annotations.description }}
             *Severity:* {{ .Labels.severity }}
             *Team:* {{ .Labels.team }}
             {{ if .Annotations.runbook_url }}*Runbook:* {{ .Annotations.runbook_url }}{{ end }}
             {{ end }}
           send_resolved: true
       
       - name: 'payments-team'
         slack_configs:
         - channel: '#payments-alerts'
           title: '💳 Payment System Alert'
           text: '{{ range .Alerts }}{{ .Annotations.summary }}\n{{ .Annotations.description }}{{ end }}'
       
       - name: 'warning-alerts'
         slack_configs:
         - channel: '#warnings'
           title: '⚠️ Warning: {{ .GroupLabels.alertname }}'
           text: '{{ range .Alerts }}{{ .Annotations.summary }}\n{{ .Annotations.description }}{{ end }}'
   ```

3. **Test Alerting Rules**
   ```bash
   # Apply alert rules
   kubectl apply -f monitoring/alert-rules.yml
   
   # Update Alertmanager configuration (if using custom config)
   # kubectl apply -f monitoring/alertmanager-config.yml
   
   # Trigger test alerts by generating errors
   GATEWAY_URL=$(minikube service payment-gateway-service --url -n velocitytech)
   
   # Generate high error rate
   for i in {1..50}; do
     curl -s -X POST $GATEWAY_URL/api/payments/process \
       -H "Content-Type: application/json" \
       -d '{"amount": -100, "method": "invalid"}' > /dev/null &
   done
   
   # Generate high response time (if you have a slow endpoint)
   for i in {1..20}; do
     curl -s "$GATEWAY_URL/api/slow-endpoint" > /dev/null &
   done
   
   # Check alerts in Prometheus
   PROMETHEUS_URL=$(minikube service prometheus-kube-prometheus-prometheus --url -n monitoring)
   echo "Check firing alerts at: $PROMETHEUS_URL/alerts"
   
   # Check Alertmanager
   ALERTMANAGER_URL=$(minikube service prometheus-kube-prometheus-alertmanager --url -n monitoring)
   echo "Check Alertmanager at: $ALERTMANAGER_URL"
   ```

4. **Create Runbook Documentation**
   ```markdown
   # Create docs/runbooks/high-error-rate.md
   # High Error Rate Runbook
   
   ## Symptoms
   - Error rate > 5% for more than 2 minutes
   - Users reporting service unavailability
   - Increased support tickets
   
   ## Immediate Actions
   1. Check service health: `kubectl get pods -n velocitytech`
   2. View recent logs: `kubectl logs -l app=payment-gateway -n velocitytech --tail=100`
   3. Check resource usage: `kubectl top pods -n velocitytech`
   4. Verify external dependencies (database, payment processor)
   
   ## Investigation Steps
   1. **Check Grafana dashboards** for traffic patterns
   2. **Review error logs** for specific error patterns
   3. **Check deployment history** for recent changes
   4. **Verify database connectivity** and performance
   5. **Check payment processor status** and API limits
   
   ## Resolution Steps
   1. **If recent deployment**: Consider rollback
      ```bash
      kubectl rollout undo deployment/payment-gateway-deployment -n velocitytech
      ```
   2. **If resource issues**: Scale up pods
      ```bash
      kubectl scale deployment/payment-gateway-deployment --replicas=5 -n velocitytech
      ```
   3. **If database issues**: Check connection pool settings
   4. **If external API issues**: Implement circuit breaker
   
   ## Prevention
   - Implement proper error handling and retries
   - Add circuit breakers for external dependencies
   - Monitor deployment impact with canary releases
   - Regular load testing to identify capacity limits
   
   ## Escalation
   - If error rate > 20%: Page on-call engineer immediately
   - If revenue impact > €1000/hour: Escalate to management
   - If external payment processor down: Contact vendor support
   ```

**Checkpoint:** Comprehensive alerting system with intelligent rules and notification routing

### Validation & Testing
**How to verify everything works:**
1. **Metrics Collection Test** - `Prometheus scraping custom application metrics successfully`
2. **Dashboard Functionality Test** - `Grafana shows real-time business and technical metrics`
3. **Alerting Test** - `Alerts trigger correctly and notifications reach appropriate channels`
4. **SLO Monitoring Test** - `Service level objectives tracked and error budgets calculated`
5. **End-to-End Test** - `Can detect and respond to issues in under 2 minutes`

**Expected Results:**
- **Visibility:** Complete system observability with business and technical metrics
- **Proactive Alerting:** Issues detected before customer complaints
- **Response Time:** Problem detection in under 1 minute
- **Dashboard Coverage:** Executive, technical, and SLO monitoring views
- **Alert Quality:** Low false positive rate, actionable notifications only

---

## 🔧 New Tools in Your Arsenal

### Prometheus Monitoring Platform
- **Purpose:** Collect, store, and query time-series metrics from applications and infrastructure
- **Key Components:**
  - PromQL query language for powerful metric analysis
  - Pull-based metric collection with service discovery
  - Time-series database optimized for monitoring data
- **Pro Tips:** Use histogram metrics for latency, counter metrics for events, gauge metrics for current values
- **Common Pitfalls:** Don't create too many high-cardinality labels, avoid storing logs in metrics

### Grafana Visualization and Dashboards
- **Purpose:** Create visual dashboards and alerts from multiple data sources including Prometheus
- **Key Features:**
  - Rich visualization options (graphs, stats, heatmaps, tables)
  - Templating for dynamic dashboards
  - Alert management with notification channels
- **Pro Tips:** Design dashboards for your audience, use consistent color schemes, focus on actionable metrics
- **Common Pitfalls:** Don't create dashboard sprawl, avoid too many metrics on one panel

### Service Level Objectives (SLO) Framework
- **Purpose:** Define and monitor service quality in business terms with error budgets
- **Key Concepts:**
  - SLI: What you measure (latency, availability, throughput)
  - SLO: Targets for SLIs (99.9% uptime, <200ms response time)
  - Error Budget: How much failure is acceptable
- **Pro Tips:** Start with simple SLOs, align with business impact, use error budgets to balance reliability and feature velocity
- **Common Pitfalls:** Don't set unrealistic SLOs, avoid too many SLIs, ensure SLOs are measurable

---

## 🔧 Common Issues & Solutions

### Issue #1: "High cardinality metrics causing performance problems"
**Symptoms:** Prometheus consuming excessive memory/disk space, slow queries, ingestion delays
**Cause:** Metrics with too many unique label combinations (like user IDs, IP addresses)
**Solution:**
```javascript
// Bad: Using user ID as label (high cardinality)
requestCounter.labels(userId, method, endpoint).inc();

// Good: Use fixed labels only
requestCounter.labels(method, endpoint).inc();

// Track user count separately with gauge
activeUsers.set(getCurrentUserCount());
```
**Prevention:** Limit labels to low-cardinality values, use separate aggregation for high-cardinality data

### Issue #2: "Too many false positive alerts disrupting team"
**Symptoms:** Alert fatigue, team ignoring notifications, alerts for non-actionable issues
**Cause:** Alerts based on symptoms rather than user impact, no alert tuning
**Solution:**
```yaml
# Bad: Alert on resource usage
- alert: HighCPU
  expr: cpu_usage > 80
  
# Good: Alert on user impact
- alert: HighLatency
  expr: histogram_quantile(0.95, http_request_duration_seconds_bucket) > 0.5
  for: 5m  # Add duration to avoid noise
```
**Prevention:** Alert on user-visible symptoms, tune thresholds based on historical data, use appropriate alert durations

### Issue #3: "Dashboards are confusing and not actionable"
**Symptoms:** Team doesn't use dashboards, too much information, unclear what to do when issues appear
**Cause:** Dashboard design focused on data availability rather than user needs
**Solution:**
```
# Create role-specific dashboards:
Executive Dashboard: Business metrics, SLO status, revenue impact
Operations Dashboard: System health, error rates, deployment status  
Developer Dashboard: Application performance, error details, resource usage

# Use consistent design patterns:
- Traffic light colors (green/yellow/red)
- Clear thresholds and baselines
- Actionable titles and descriptions
```
**Prevention:** Design dashboards for specific audiences, include context and thresholds, regularly review and update

---

## 🎭 Character Evolution This Week

### Legacy Luka's Observability Awakening
**Monday:** "More monitoring tools? We already have basic server monitoring. Isn't that enough?"
**Tuesday:** "These custom metrics show exactly how my code is performing in production. I've never had this visibility before."
**Wednesday:** "I can see which features are used most and how they perform. This changes how I prioritize development."
**Thursday:** "Debugging production issues is so much easier when I can see the actual data instead of guessing."
**Friday:** "I'm writing better code because I can see its real-world impact. Metrics are like unit tests for production."
**Key Quote:** "Observability turned my code from a black box into a glass house. I can see everything that's happening."

### Anxious Ana's Testing Revolution
**Monday:** "Another system to learn? How many monitoring tools do we need?"
**Tuesday:** "I can see exactly how the system behaves under load. My performance tests finally make sense!"
**Wednesday:** "These dashboards show user experience metrics I could never test manually."
**Thursday:** "I can validate that my testing scenarios match real user patterns. This is incredibly valuable!"
**Friday:** "Monitoring is like having continuous testing running in production. I can spot issues before users do."
**Key Quote:** "Observability extended my testing capabilities into production. I'm not just testing features - I'm validating user experience."

### Firefighter Filip's Proactive Transformation
**Monday:** "Great, now I have even more systems to monitor and maintain. More complexity, more potential failures."
**Tuesday:** "Wait... I got an alert about a problem BEFORE customers called? This is new."
**Wednesday:** "I can see problems forming before they become critical. No more surprises at 3 AM!"
**Thursday:** "The runbooks linked to alerts help me resolve issues faster than ever. Structured response works."
**Friday:** "I went from fighting fires to preventing them. I'm a systems architect now, not just a firefighter."
**Key Quote:** "Observability turned me from reactive to proactive. I prevent problems instead of just fixing them."

### Manager Maja's Strategic Visibility
**Monday:** "How much is this monitoring infrastructure going to cost us? We need to prove ROI."
**Tuesday:** "I can see real business metrics now - revenue impact, user satisfaction, service quality."
**Wednesday:** "These SLO dashboards give me data to make informed decisions about reliability vs. feature development."
**Thursday:** "Client conversations are completely different when I have concrete performance data to share."
**Friday:** "Observability isn't just operational - it's strategic. I can make data-driven business decisions now."
**Key Quote:** "Monitoring transformed from IT overhead to business intelligence. Now I have visibility into what actually matters."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 11 Improvements:**

| Metric | Week 10 State | After This Week | Target (Week 15) |
|--------|--------------|------------------|------------------|
| Lead Time | 6 days | 4 days | 2 hours |
| Mean Time to Detection | 4 hours | 2 minutes | <1 minute |
| Mean Time to Recovery | 5 minutes | 3 minutes | <1 minute |
| Service Availability | 99.8% | 99.9% | 99.95% |
| Alert Response Time | Manual discovery | 30 seconds | <15 seconds |
| False Positive Rate | N/A | 5% | <2% |
| Business Visibility | 0% | 95% | 100% |

**Key Improvements:**
- **Proactive Detection:** 120x faster problem detection (4 hours → 2 minutes)
- **System Visibility:** Complete observability with business and technical metrics
- **Service Reliability:** 99.9% availability with proactive issue resolution
- **Team Productivity:** Elimination of surprise outages and customer-reported issues

---

## 🇭🇷 Croatian IT Market Reality
Observability and monitoring expertise is highly valued in Croatian IT companies. Organizations like Infobip, Rimac, and Span depend on sophisticated monitoring to maintain service reliability at scale. Site Reliability Engineering (SRE) and Platform Engineering roles focusing on observability command premium salaries (40-60% higher than traditional operations roles). Many Croatian companies still lack proper monitoring, creating high demand for professionals who can implement comprehensive observability strategies. Your monitoring expertise positions you for senior DevOps, SRE, and platform engineering positions.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about observability implementation?** How comprehensive monitoring changes team behavior from reactive to proactive
2. **Which observability pillar had the biggest impact?** Custom metrics providing business visibility alongside technical insights
3. **How did monitoring change your view of system reliability?** From hoping nothing breaks to knowing exactly what's happening
4. **What questions do you still have?** How to scale observability practices across multiple teams and services

**Team Discussion Points:**
- How would each VelocityTech character feel about comprehensive monitoring becoming mandatory for all services?
- What resistance might we encounter from teams comfortable with basic monitoring approaches?
- How would you convince skeptical management to invest in advanced observability infrastructure?

**Preparation for Next Week:**
Ana arrives Monday morning excited about the comprehensive monitoring but concerned: "We can see everything that's happening now, but when issues occur, I'm still hunting through dozens of different log files across multiple services. The metrics tell us what's wrong, but finding the detailed context in logs is still time-consuming. We need a better way to search and correlate all our log data!" The centralized logging and log analysis journey begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Site Reliability Engineering" by Google - Chapters on monitoring and alerting
- "Observability Engineering" by Charity Majors - Modern observability practices

**Hands-on Practice:**
- Implement Prometheus monitoring for your personal projects
- Practice PromQL queries with the Prometheus query builder
- Create Grafana dashboards for different stakeholder audiences

**Industry Insights:**
- "State of Observability" reports - Industry trends and adoption patterns
- Prometheus and Grafana community surveys - Usage patterns and best practices
- Croatian SRE meetups - Network with local reliability engineers

**Advanced Topics:**
- OpenTelemetry for distributed tracing and unified observability
- Advanced PromQL for complex query analysis
- Alerting best practices and escalation policies
- Cost optimization for large-scale monitoring deployments

**Croatian Companies Leading in Observability:**
- **Infobip:** Comprehensive monitoring across global communication platform
- **Rimac:** Automotive systems monitoring with real-time performance tracking
- **Span:** Web platform observability with business intelligence integration
- **Photomath:** Mobile backend monitoring at scale with millions of users
- **Include:** Financial services monitoring with compliance and audit requirements

**Certification Path:**
- **Prometheus Certified Associate** - Monitoring and alerting expertise
- **Grafana Certified Professional** - Dashboard and visualization skills
- **Google Cloud Professional Cloud Architect** - Includes observability design patterns
- **Site Reliability Engineering** certifications from major cloud providers

---

*Next up: Week 12 - Centralized Logging: "Finding Needles in Haystacks" →*

> *"You can't improve what you can't measure. You can't measure what you can't see. And you can't see what you don't monitor. Observability makes the invisible visible, the unknown knowable, and the unpredictable predictable."* - Observability Philosophy

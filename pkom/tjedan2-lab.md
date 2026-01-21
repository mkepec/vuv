# Week 2 Lab Guide: Operation Dark Network
## The Command Center - Visualization & Dashboarding

**Course:** Network Design (Projektiranje komunikacijskih mreža)
**Lab Duration:** Week 2 of 3
**Estimated Time:** 4-6 hours (2 lab sessions)
**Learning Outcomes:** LO6, LO7

# Vodič za laboratorijske vježbe (Tjedan 2): Operacija Dark Network
## Zapovjedni centar – Vizualizacija i nadzorne ploče (Dashboards)
Kolegij: Projektiranje komunikacijskih mreža 
Trajanje vježbe: Tjedan 2 od 3 
Predviđeno vrijeme: 4-6 sati (2 laboratorijska termina) 
Ishodi učenja: IU6, IU7

---

## Table of Contents

1. [Overview & Objectives](#overview--objectives)
2. [Prerequisites](#prerequisites)
3. [Mission 1: Deploy Grafana](#mission-1-deploy-grafana)
4. [Mission 2: Connect Data Source](#mission-2-connect-data-source)
5. [Mission 3: Import Community Dashboards](#mission-3-import-community-dashboards)
6. [Mission 4: Understanding Dashboard Components](#mission-4-understanding-dashboard-components)
7. [Mission 5: Customize for Your Network](#mission-5-customize-for-your-network)
8. [Mission 6: The Executive Dashboard](#mission-6-the-executive-dashboard)
9. [Troubleshooting Guide](#troubleshooting-guide)
10. [Submission Requirements](#submission-requirements)

---

## Pregled i ciljevi

### Što gradite
Ovog tjedna transformirat ćete sirove metričke podatke koje ste prikupili prošlog tjedna u **vizualne nadzorne ploče** koje svatko može razumjeti.

Dodat ćete **Grafanu** u svoj monitoring "stack":
- **Grafana** – Platforma za vizualizaciju i izradu nadzornih ploča.
- **Povezivanje** s vašim postojećim Prometheus podacima.
- **Gotove nadzorne ploče** iz Grafana zajednice.
- **Prilagođeni prikazi** skrojeni prema vašoj mreži.

### Ciljevi učenja

Nakon završetka ove vježbe, moći ćete:
- ✅ Implementirati Grafanu koristeći Docker Compose.
- ✅ Konfigurirati Prometheus kao izvor podataka (Data Source) u Grafani.
- ✅ Importirati i koristiti nadzorne ploče koje je izradila zajednica.
- ✅ Razumjeti komponente nadzorne ploče (paneli, upiti, varijable).
- ✅ Prilagoditi nadzorne ploče za specifične slučajeve korištenja.
- ✅ Primijeniti FCAPS koncepte (Upravljanje kvarovima i performansama).
- ✅ Kreirati vizualizacije prilagođene menadžmentu (executive-friendly).

### Šira slika (Arhitektura)

```
┌─────────────────────────────────────────────────────────────────┐
│                    YOUR MONITORING VM                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Prometheus  │  │SNMP Exporter │  │Node Exporter │          │
│  │(Port 9090)   │  │(Port 9116)   │  │(Port 9100)   │          │
│  └──────┬───────┘  └──────────────┘  └──────────────┘          │
│         │                                                        │
│         │ Data Source Connection                                │
│         ▼                                                        │
│  ┌──────────────────────────────────────────┐                   │
│  │          Grafana (Port 3000)             │                   │
│  │  ┌────────────┐  ┌────────────┐         │                   │
│  │  │ Windows    │  │   Cisco    │         │                   │
│  │  │ Dashboard  │  │ Dashboard  │         │                   │
│  │  └────────────┘  └────────────┘         │                   │
│  │  ┌────────────┐  ┌────────────┐         │                   │
│  │  │   Server   │  │ Executive  │         │                   │
│  │  │ Dashboard  │  │ Overview   │         │                   │
│  │  └────────────┘  └────────────┘         │                   │
│  └──────────────────────────────────────────┘                   │
└─────────────────────────────────────────────────────────────────┘
                            │
                            │ Beautiful Graphs!
                            ▼
                      [CEO is happy]
```

---

## Preduvjeti

### Što vam je potrebno iz 1. tjedna

- ✅ Pokrenut Prometheus koji prikuplja podatke s vašeg servera, Cisco switcha i Windows radnih stanica u učionici.
- ✅ Svi konfigurirani ciljevi (targets) moraju imati status "UP" u Prometheusu.
- ✅ Pristup vašoj monitoring VM.

### Provjera postavki iz 1. tjedna

Prije početka, potvrdite da sve iz prošlog tjedna i dalje radi:

```bash
# SSH na vašu VM
ssh root@10.10.50.10

# Navigirajte u direktorij za monitoring
cd /opt/monitoring

# Provjerite rade li kontejneri
docker-compose ps
```

**✅ Checkpoint:** Trebali biste vidjeti 3 kontejnera: `prometheus`, `snmp_exporter`, `node_exporter` – svi moraju biti "Up".

**Provjera Prometheus targeta:**
Otvorite `http://IP_adresa:9090/targets` u pregledniku.

**✅ Checkpoint:** Svi vaši ciljevi (`prometheus`, `monitoring-server`, `cisco-2950` i `windows-workstations`) trebaju biti "UP".

**Ako je bilo što iz 1. tjedna neispravno, popravite to prije nastavka!**

---

## Misija 1: Implementacija Grafane

**Cilj:** Dodati Grafanu u vaš Docker Compose "stack".

### Korak 1.1: Ažuriranje Docker Compose konfiguracije

Uredite postojeću `docker-compose.yml` datoteku:

```bash
cd /opt/monitoring
nano docker-compose.yml
```

**Dodajte grafana servis** u sekciju services: (zadržite sve postojeće servise):

```yaml
  grafana:
    image: grafana/grafana-oss:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=VUV_Monitor_2026
    volumes:
      - grafana_data:/var/lib/grafana
    networks:
      - monitoring
    depends_on:
      - prometheus
```

**Dodajte Grafana volumen** u sekciju `volumes`: na dnu:

```yaml
volumes:
  prometheus_data:
  grafana_data:  # Dodajte ovu liniju
```

**Spremite i izađite** (Ctrl+X, Y, Enter)

### Step 1.2: Deploy Grafana

```bash
# Pull the Grafana image and start the service
# The '-d' flag ensures it runs in the background
docker-compose up -d grafana

# Wait a few moments for it to start
sleep 10

# Check it's running
docker-compose ps grafana
```

**✅ Checkpoint:** Grafana should show status "Up".

### Step 1.3: Access Grafana Web Interface

From your laptop's web browser, navigate to:

```
http://10.10.50.10:3000
```

**Login credentials:**
- **Username:** `admin`
- **Password:** `VUV_Monitor_2026` (as defined in `docker-compose.yml`)

You may be prompted to change your password. You can skip this for the lab.

**✅ Checkpoint:** You should see the Grafana welcome page!

**📸 Screenshot:** Capture the Grafana welcome/home page after login.

---

## Mission 2: Connect Data Source

**Objective:** Configure Grafana to read data from Prometheus.

Grafana doesn't store data; it queries other systems called **Data Sources**. Let's connect it to Prometheus.

### Step 2.1: Add Prometheus Data Source

In the Grafana web interface:

1. Click the **☰ menu** (top left) and go to **Connections**.
2. Click **Add new connection**.
3. In the search box, type `Prometheus` and select it.
4. Configure the settings:
   - **Prometheus server URL:** `http://prometheus:9090`
5. Click **Save & Test** at the bottom.

**Why `http://prometheus:9090`?**
Your Grafana and Prometheus containers are on the same Docker network. Docker's internal DNS allows them to find each other using their service names (`prometheus` and `grafana`).

**✅ Checkpoint:** You should see a green notification: "Successfully queried the Prometheus API."

### Step 2.2: Verify with an Exploration

Let's test the connection by running a query.

1. Click **Explore** (the compass icon in the left sidebar).
2. At the top, ensure `Prometheus` is selected as the data source.
3. In the **Metric** field, type `up`.
4. Click **Run query** (top right).

**✅ Checkpoint:** You should see a graph showing the status of your monitoring targets from Week 1 (a value of `1` means the target is up).

**📸 Screenshot:** Capture the Explore view showing the `up` metric with data.

---

## Mission 3: Import Community Dashboards

**Objective:** Import pre-built dashboards from the Grafana community.

Why build from scratch when you can use the work of others? The Grafana community maintains a library of thousands of free dashboards at **grafana.com/dashboards**. Each has a unique ID.

### Step 3.1: Import Node Exporter Dashboard

This dashboard visualizes metrics from the `node_exporter` running on your monitoring server.

1. In Grafana, click **Dashboards** (four squares icon in the left sidebar).
2. Click **New** → **Import**.
3. In the "Import via grafana.com" field, enter the ID: **`1860`** and click **Load**.
4. On the next screen, at the bottom, select your `Prometheus` data source.
5. Click **Import**.

**✅ Checkpoint:** You should see a beautiful dashboard with CPU, memory, disk, and network stats for your main monitoring server.

### Step 3.2: Import SNMP Dashboard for Cisco Switch

This dashboard is generic for SNMP-enabled devices and works well for your Cisco switch.

1. Go back to **Dashboards** → **New** → **Import**.
2. Enter dashboard ID: **`11169`** and click **Load**.
3. Select your `Prometheus` data source.
4. Click **Import**.

**✅ Checkpoint:** You should see interface statistics. Use the dropdowns at the top to select the IP address of your Cisco switch (`10.10.50.Z`).

### Step 3.3: Import Windows Exporter Dashboard

Now, let's visualize the metrics from all the classroom workstations you added in Week 1.

1. Go to **Dashboards** → **New** → **Import**.
2. Enter dashboard ID: **`14694`** and click **Load**.
3. Name the dashboard `Windows Workstations Classroom`.
4. Select your `Prometheus` data source.
5. Click **Import**.

**✅ Checkpoint:** You should now have a comprehensive dashboard showing the status of all Windows workstations in the classroom. You can switch between different workstations using the `instance` dropdown at the top.

### Step 3.4: A Note on "No Data"

You might have imported a dashboard for a device you haven't configured yet (like the MikroTik router). In this case, many panels will show **"No data"**. This is expected! A dashboard is only as good as the data it receives.

**📸 Screenshot:** Capture the **Windows Workstations Classroom** dashboard showing live data from your network.

---

## Mission 4: Understanding Dashboard Components

**Objective:** Learn how dashboards are structured so you can customize them.

Open the **Node Exporter Full** dashboard (`1860`) and click the **gear icon** (⚙️) at the top right, then select **Edit**. This allows you to see how it's built.

- **Variables:** The dropdowns at the top. They let you filter the data shown in the panels. (e.g., selecting a specific `instance` or `job`). They are defined in Dashboard settings → Variables.
- **Panels:** Each individual graph, stat, or table is a panel.
- **Queries:** Inside each panel is a PromQL query that fetches the data.

Hover over any panel title, click the three dots (⋮), and select **Edit**. You can see the exact PromQL query being used.

**Spend 5 minutes exploring the panels in the dashboards you imported. Don't save any changes.**

---

## Mission 5: The Executive Dashboard

**Objective:** Create a simple, high-level dashboard for a non-technical audience (like the CEO).

Remember the "CEO Test": Can they look at the screen for 3 seconds and know if the network is healthy?

### Step 5.1: Create a New Dashboard

1. Go to **Dashboards**.
2. Click **New** → **New Dashboard**.
3. Click **Add visualization**.

### Step 5.2: Panel 1 - Overall Network Health

Let's create a single stat showing what percentage of our infrastructure is online.

1. In the query editor, enter:
   ```promql
   (sum(up) / count(up)) * 100
   ```
2. On the right side, under **Visualization**, select **Stat**.
3. Customize the panel:
   - **Panel options** → Title: `System Health Uptime`
   - **Standard options** → Unit: `Percent (0-100)`
   - **Thresholds:** Keep the defaults (Base: Green). This will show the value in green.
4. Click **Apply** in the top right.

### Step 5.3: Panel 2 - Status of Core Services

Let's show the status of each major group of targets.

1. Click **Add** → **Visualization** on the dashboard.
2. Select the **Table** visualization.
3. Enter the following query:
   ```promql
   up
   ```
4. On the right, go to the **Transform** tab.
5. Add two **"Group by"** transformations:
   - Group by `job` (select `Calculate` → `count`)
   - Group by `job` again (select `Calculate` → `sum`)
6. This gives you a count of total targets and how many are up. Now let's make it pretty.
7. Go to the **Overrides** tab.
   - Click **Add field override** → **Fields with name** → select `sum`.
   - Add override property → **Cell display mode** → `Color background`.
   - Add another override property → **Thresholds** → `Add threshold`.
     - Set it so the color is **Green** when the value is high (e.g., `1`).
8. Click **Apply**.

You now have a table that shows each job (`cisco-2950`, `windows-workstations`, etc.) and how many targets in that job are online.

### Step 5.4: Save and Organize

1. Drag your panels to arrange them neatly.
2. Click the **Save** icon (floppy disk) at the top right.
3. Name your dashboard `Executive Dashboard - Vertex Solutions`.
4. Click **Save**.

**📸 Screenshot:** Capture your complete Executive Dashboard.

---

## Mission 6: Understanding FCAPS

**Objective:** Map your dashboards to network management principles.

**FCAPS** is a network management framework: **F**ault, **C**onfiguration, **A**ccounting, **P**erformance, **S**ecurity.

In your Executive Dashboard:
- The **System Health** panel is for **Fault Management** (is something down?).
- The **Core Services** table is also for **Fault Management**.
- If you were to add a panel showing `rate(ifHCInOctets[5m])`, that would be **Performance Management**.

**📝 Document:** Think about which FCAPS categories your imported dashboards cover. For example, the Windows Exporter dashboard covers **Performance** (CPU, memory) and **Faults** (is the machine up?).

---

## Troubleshooting Guide

### Problem: Grafana shows "No data" on dashboards.

1.  **Check Time Range:** Is the time range at the top right of the dashboard (e.g., "Last 6 hours") appropriate? If you just started collecting data, try a smaller range like "Last 5 minutes".
2.  **Check Prometheus:** Does the query work in Prometheus itself? Copy the query from the Grafana panel and run it in the Prometheus UI (`http://10.10.50.10:9090`).
3.  **Check Labels:** Community dashboards often use generic labels. Your job might be named `cisco-2950`, but the dashboard expects `job="snmp"`. You may need to edit the panel query to match your job name.

### Problem: Grafana login fails.

If you forget the password (`VUV_Monitor_2026`), you can reset it via the command line:
```bash
docker exec -it grafana grafana-cli admin reset-admin-password YOUR_NEW_PASSWORD
```

---

## Submission Requirements

### Required Deliverables

Create a single PDF document containing:

#### 1. Cover Page
- Your name and student ID, Lab title: "Week 2 - Visualization & Dashboarding", Date.

#### 2. Deployment Documentation
- Screenshot of Grafana home page after login.
- Screenshot of the Prometheus data source configuration showing it's working.
- Output of `docker-compose ps` showing all running containers.

#### 3. Community Dashboards
- Screenshot of the **Node Exporter Full** dashboard (`1860`).
- Screenshot of the **Windows Workstations** dashboard (`14694`), showing data for one of the classroom PCs.

#### 4. Executive Dashboard
- A full screenshot of your custom `Executive Dashboard - Vertex Solutions`.
- A brief explanation of what each panel shows.

#### 5. Reflection (500 words max)
- How does visualization change the value of monitoring data?
- What is the "CEO Test" and why is it important for a network engineer to consider?
- Which Grafana panel or feature did you find most useful and why?

### Submission Deadline

**Due:** End of Week 2 lab session (Friday, 23:59)

**Submit to:** [Your course management system / email]

---

## What's Next?

**Week 3 Preview:** Now you can *see* your data. But what if you're not looking? Next week, you'll configure **Alertmanager** to proactively notify you when something goes wrong. You will transform from a reactive to a **proactive** administrator.

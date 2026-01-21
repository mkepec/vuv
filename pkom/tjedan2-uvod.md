# Week 2: The Executive Briefing

## Mission Update from Management

**Date:** Monday, 8:15 AM (Week 2)
**From:** Sarah Chen, IT Manager
**To:** Network Administrator
**Subject:** Great work + New challenge

---

### Last Week's Win

First, let me say: **excellent work** getting the monitoring infrastructure online.

I logged into Prometheus over the weekend (yes, I work weekends, don't judge), and I can see our devices are being monitored. The `up` query shows everything is green. The SNMP metrics are flowing.

From a technical perspective, this is a huge step forward.

But...

---

### The Problem

I showed Prometheus to the CEO this morning.

I pulled up the web interface, typed in a query like you showed me: `rate(ifHCInOctets[5m]) * 8 / 1000000`

The screen displayed:
```
ifHCInOctets{ifDescr="ether1",ifIndex="5",instance="10.10.50.1",job="mikrotik-rb3011"} 87.234567
ifHCInOctets{ifDescr="ether2",ifIndex="6",instance="10.10.50.1",job="mikrotik-rb3011"} 12.891234
...
```

She looked at me like I was speaking Klingon.

Her exact words: **"Sarah, I need a dashboard, not a spreadsheet from 1995."**

She's right.

---

### What We Need

Here's what happened next. She pulled out her phone and showed me her **fitness app**.

"Look," she said, "I can see my steps, my heart rate, my sleep quality. All on one screen. All in graphs I can understand. Why can't I see my network like this?"

She wants:
- **Visual dashboards** that make sense to non-technical people
- **Graphs that show trends** (not just numbers)
- **Status indicators** (green = good, red = bad)
- **A single screen** she can glance at during meetings

In other words: She wants what you built last week, but **wrapped in a interface a human can understand**.

---

### The Ask

This week, I need you to deploy **Grafana**.

From what I've read, Grafana is the "visualization layer" for Prometheus. It takes those ugly metric queries and turns them into beautiful, understandable dashboards.

Your mission:
1. Deploy Grafana on the monitoring server
2. Connect it to Prometheus
3. Build dashboards that show the health and performance of our network
4. Make it look professional enough to show the CEO

---

### Success Criteria

By the end of this week, I want to be able to:

1. **Open a web browser** to a dashboard URL
2. **See at a glance** if our network is healthy
3. **Understand trends** without knowing what "ifHCInOctets" means
4. **Show this to the CEO** without being embarrassed

**The "CEO Test":** If I can open the dashboard and answer "Is the warehouse switch working?" in 3 seconds, you've succeeded.

---

### What You Have

Last week you built the foundation. This week you're building the interface.

You have:
- Working Prometheus installation
- Metrics flowing from all devices
- Knowledge of PromQL (from last week's queries)
- Access to the same monitoring server
- **This lab guide** for Week 2

What you're adding:
- Grafana (the visualization tool)
- Pre-built dashboards from the community
- Custom panels tailored to our needs

---

### A Note on Community Dashboards

The good news: You don't have to build everything from scratch.

The Grafana community has created **thousands of pre-built dashboards** for common devices. There are dashboards specifically for:
- MikroTik routers
- Cisco switches
- SNMP-enabled devices
- Linux servers

Your job is to:
1. Find the right dashboards
2. Import them into Grafana
3. Customize them for our specific needs
4. Make sure they actually show our data

Think of it like installing a theme on a website. The hard work is done; you just need to configure it.

---

### Timeline

**This Week (Week 2):**
- Deploy Grafana
- Connect to Prometheus
- Import community dashboards
- Verify data visualization

**Next Week (Week 3):**
- Build custom dashboards from scratch
- Add alerting
- Make it production-ready

---

### The Bigger Picture

Last week, you gave us **visibility**.

This week, you're giving us **understanding**.

Next week, you'll give us **action** (alerts).

We're transforming from a company that finds out about problems when customers call, to a company that sees problems before they impact anyone.

That's a big deal.

---

## Real Talk

I know "make it pretty" might sound less exciting than the technical work you did last week. But honestly? This is where the value shows up.

A network engineer who can bridge the gap between `rate(ifHCInOctets[5m])` and "here's a dashboard that shows our warehouse traffic is about to max out the connection" is **invaluable**.

You're not just making graphs. You're translating technical reality into business insight.

That's the skill that gets you promoted.

---

### One More Thing

The CFO heard about this project. She asked: **"Can we see our network costs on a dashboard too?"**

Not this week. But it's coming. Once we have monitoring and visualization working, people are going to want metrics on everything.

You're building something that's going to expand. Do it right.

---

## Your Assignment

Head over to the **Week 2 Lab Guide** and start with "Mission 1: Deploy Grafana."

You know how to use Docker Compose from last week. This should feel familiar.

By Friday, I want to see a dashboard I can show the CEO.

**Make it happen.**

Good luck,

**Sarah Chen**
IT Manager, Vertex Solutions
sarah.chen@vertex-solutions.local
Ext. 2847

---

*P.S. - The CEO actually used the word "dashboard" 14 times in a 5-minute conversation. She REALLY wants this.*

*P.P.S. - If this works, there's budget approval for better hardware next quarter. Just saying.*

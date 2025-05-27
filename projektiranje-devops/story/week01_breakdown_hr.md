# Week 1: Welcome to VelocityTech - Current State Assessment 🏗️
**VelocityTech Transformation Week 1**

**Story Context:** Your first day at VelocityTech Solutions - witness the Friday evening deployment disaster and begin documenting the chaos  
**Focus:** Establishing baseline metrics and understanding current pain points  
**Characters in Focus:** Meeting all team members and their daily frustrations

---

## 🔥 The Weekly Crisis: "Friday Evening Production Meltdown"
**Setting:** VelocityTech office, Friday 4:30 PM. Most people planning weekend escapes.  
**The Problem:** Critical customer-facing bug discovered just as everyone's leaving

**Scene:** [VelocityTech main development floor. Coffee cups abandoned, coats half-on.]

**MAJA** *(frantically refreshing monitoring dashboard)*  
> "Customer portal is down! We've got 500 users locked out of their accounts!"  
> "The client is calling every 10 minutes. They want answers NOW."

**FILIP** *(already pulling up server logs)*  
> "Here we go again... Let me guess, it's the database connection timeout?"  
> *(sighs heavily)* "Why does this always happen on Friday afternoons?"

**LUKA** *(defensively, still in his coat)*  
> "My code worked fine in development! It passed all the manual tests yesterday."  
> "Maybe it's an environment issue? The servers are different..."

**ANA** *(looking overwhelmed)*  
> "I tested the login flow, but I can't test every possible user scenario."  
> "There are thousands of edge cases! I'm just one person!"

**MARKO** *(your mentor, calmly assessing the situation)*  
> "Right. Filip, what's our usual recovery time for issues like this?"  
> "Ana, how long does it typically take to verify a fix?"

**FILIP** *(checking his phone for previous incidents)*  
> "Last three similar issues... 2 hours, 4 hours, and 6 hours to resolve."  
> "Plus another hour to verify it's actually fixed."

**ANA**  
> "If I have to manually test everything again, that's at least 3 hours of scenarios."  
> "Assuming I don't find more bugs..."

**MAJA** *(calculating in her head)*  
> "So we're looking at potentially 7+ hours? It's Friday evening!"  
> *(to you)* "You're our new intern, right? What do you see that we're missing?"

**[Everyone turns to you - the fresh perspective they hope will somehow make sense of this recurring nightmare.]**

**MARKO** *(to you quietly)*  
> "This happens every 2-3 weeks. There's a pattern here, but they can't see it."  
> "What would you measure to understand why this keeps happening?"

**[Your systems thinking kicks in. This isn't just bad luck - this is a symptom of deeper problems.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Measure current state using DORA metrics** - Calculate lead time, deployment frequency, MTTR, and change failure rate for any development team
2. **Create value stream maps that reveal hidden waste** - Identify bottlenecks, waiting time, and handoff problems in software delivery
3. **Establish measurement systems** - Set up tracking that enables data-driven transformation decisions

**Real-world Connection:** These assessment skills are exactly what DevOps consultants use when entering Croatian companies like Infobip, Rimac, or Span. Measuring current state is always the first step in any transformation project.

---

## 📚 The Knowledge Foundation

### DevOps Principles and the CALMS Framework
**The Problem it Solves:** Teams working in silos, manual processes causing delays and errors  
**How it Works:** CALMS provides a framework for cultural and technical transformation:
- **C**ulture - Breaking down silos between development and operations
- **A**utomation - Eliminating repetitive manual tasks
- **L**ean - Focusing on value, eliminating waste
- **M**easurement - Making decisions based on data, not assumptions
- **S**haring - Knowledge sharing and collective responsibility

**Real-world Example:** Netflix transformed from DVD shipping to streaming by applying these principles  
**VelocityTech Connection:** Friday's crisis shows lack of shared responsibility - developers blame environment, ops blame code, QA overwhelmed by manual work

### DORA Metrics - The Four Keys to DevOps Performance
**The Problem it Solves:** No objective way to measure development team performance and improvement  
**How it Works:** Four metrics that correlate with organizational performance:

**Lead Time for Changes**
- Time from code commit to production deployment
- VelocityTech current: ~30 days (based on today's observation)
- World-class target: Less than 1 hour

**Deployment Frequency**
- How often we release to production
- VelocityTech current: Monthly releases (with pain)
- World-class target: On-demand deployments

**Mean Time to Recovery (MTTR)**
- How quickly we recover from production failures
- VelocityTech current: 3-4 hours (today's crisis proves this)
- World-class target: Less than 1 hour

**Change Failure Rate**
- Percentage of deployments causing production problems
- VelocityTech current: ~30% (estimate from team discussion)
- World-class target: 0-15%

**Real-world Example:** Google, Amazon, and Microsoft use these metrics to drive improvements  
**VelocityTech Connection:** Today's incident is a perfect example of high MTTR and likely high change failure rate

### Value Stream Mapping Fundamentals
**The Problem it Solves:** Hidden waste and bottlenecks in development processes  
**How it Works:** Visual representation showing how work flows from idea to customer value

**Types of Work:**
- **Value-adding** - Customer pays for this (writing features)
- **Non-value-adding but necessary** - Compliance, security reviews
- **Pure waste** - Waiting, rework, handoffs without purpose

**Common Waste Patterns:**
- **Waiting** - Code waits for approval, testing, deployment
- **Extra processing** - Manual steps that could be automated
- **Defects** - Bugs found late in the process
- **Transport** - Moving code between environments

**Real-world Example:** Toyota's manufacturing principles applied to software development  
**VelocityTech Connection:** Friday's issue shows waste - manual testing delays, environment inconsistencies, long feedback loops

**Key Takeaways:**
- Measurement reveals problems that feelings hide
- DORA metrics provide objective improvement targets
- Value stream mapping shows where time actually goes
- Current state assessment must be honest and comprehensive

---

## 🛠️ Lab Mission: "The VelocityTech Diagnostic"
**Scenario:** You're the new intern tasked with understanding why VelocityTech has recurring production crises  
**Your Mission:** Establish baseline metrics and create a value stream map showing current reality  
**Success Criteria:** Quantified current state with identified improvement opportunities

### Phase 1: DORA Metrics Baseline (45 minutes)
**Objective:** Establish measurable baseline for VelocityTech's current performance

**Steps:**
1. **Lead Time Analysis**
   ```bash
   # Access VelocityTech's Jira/project tracking system
   # Find last 10 completed features
   # Record: Request date → Code complete → Production date
   # Calculate average lead time
   
   echo "Feature 1: 35 days from request to production"
   echo "Feature 2: 28 days from request to production"
   echo "Average lead time: 31.5 days"
   ```
   - Expected output: Average lead time of 25-35 days
   - Troubleshooting: If data is incomplete, interview team members for estimates

2. **Deployment Frequency Tracking**
   ```bash
   # Review Git history for production tags
   git log --oneline --grep="PROD\|RELEASE\|DEPLOY" --since="6 months ago"
   
   # Count production deployments per month
   # Document deployment success/failure
   ```
   - Expected output: 1-2 deployments per month
   - Troubleshooting: Check multiple branches if using Git Flow

3. **MTTR Calculation**
   ```bash
   # Review incident tracking system
   # Document last 10 production incidents:
   # - Detection time
   # - Resolution start time  
   # - Resolution completion time
   
   echo "Incident 1: 3.5 hours MTTR"
   echo "Incident 2: 4.2 hours MTTR"
   echo "Average MTTR: 3.8 hours"
   ```
   - Expected output: 2-6 hours average MTTR
   - Troubleshooting: If no formal incident tracking, use email/Slack history

**Checkpoint:** You should have baseline numbers for all four DORA metrics

### Phase 2: Value Stream Mapping Workshop (90 minutes)
**Objective:** Map the complete flow from feature request to production deployment

**Steps:**
1. **Stakeholder Interviews**
   ```
   Interview each character (5 minutes each):
   
   Luka (Developer):
   "Walk me through what happens when you get a new feature request"
   Document: Steps, waiting times, handoffs
   
   Ana (QA):
   "How does code reach you for testing?"
   Document: Testing process, approval gates, bottlenecks
   
   Filip (Operations):
   "What's involved in deploying to production?"
   Document: Deployment steps, environment preparation, monitoring
   
   Maja (Manager):
   "How do you track progress and communicate with clients?"
   Document: Reporting processes, status updates, approval chains
   ```

2. **Process Flow Mapping**
   ```
   Current State Map:
   
   [Feature Request] → [Requirements Analysis] → [Development] → [Code Review]
         ↓ 3 days          ↓ 2 days           ↓ 10 days     ↓ 2 days
   
   [QA Testing] → [UAT] → [Deployment Prep] → [Production Deploy]
     ↓ 5 days    ↓ 3 days    ↓ 4 days          ↓ 2 days
   
   Total: ~31 days (only 12 days adding value)
   ```

3. **Waste Identification**
   ```markdown
   Red (Pure Waste):
   - 3-day wait for requirement clarification
   - 2-day wait for code review availability
   - 4-day environment preparation delays
   
   Yellow (Necessary but Non-Value):
   - Manual UAT process (security requirement)
   - Compliance documentation reviews
   
   Green (Value-Adding):
   - Actual feature development
   - Bug fixing and quality improvements
   ```

**Checkpoint:** Complete value stream map showing 60%+ waste/waiting time

### Phase 3: Pain Point Documentation (45 minutes)
**Objective:** Document and quantify the most impactful problems

**Steps:**
1. **Technical Pain Points**
   ```markdown
   ## Pain Point: Environment Inconsistencies
   **Impact:** High
   **Frequency:** Weekly
   **Affects:** Luka (development), Ana (testing), Filip (operations)
   **Current Cost:** 8 hours/week troubleshooting
   **Root Cause:** Manual environment setup, no configuration management
   **Potential Solution:** Infrastructure as Code, containerization
   ```

2. **Process Pain Points**
   ```markdown
   ## Pain Point: Manual Testing Bottleneck
   **Impact:** High
   **Frequency:** Every release
   **Affects:** Ana (overwhelmed), entire team (delays)
   **Current Cost:** 5 days per release cycle
   **Root Cause:** No test automation, single point of failure
   **Potential Solution:** Automated testing pipeline
   ```

3. **Cultural Pain Points**
   ```markdown
   ## Pain Point: Blame Culture During Incidents
   **Impact:** Medium
   **Frequency:** Every incident
   **Affects:** Team morale and collaboration
   **Current Cost:** Reduced knowledge sharing, defensive behavior
   **Root Cause:** Lack of blameless post-mortems
   **Potential Solution:** Incident review process focused on systems improvement
   ```

### Validation & Testing
**How to verify everything works:**
1. **Metrics Validation** - `Cross-check numbers with team members for accuracy`
2. **Process Validation** - `Walk through mapped process with each stakeholder`
3. **Pain Point Validation** - `Confirm impact assessments with affected team members`

**Expected Results:**
- **Lead Time:** 25-35 days measured and documented
- **Deployment Frequency:** 1-2 per month confirmed
- **MTTR:** 3-6 hours average with incident examples
- **Change Failure Rate:** 20-40% estimated with supporting data
- **Value Stream Map:** Shows >50% waste/waiting time
- **Pain Points:** 8-12 documented issues with quantified impact

---

## 🔧 New Tools in Your Arsenal
### Excel/Google Sheets for Metrics Tracking
- **Purpose:** Collect and analyze baseline performance data
- **Key Functions:**
  - `=AVERAGE(range)` - Calculate mean values for metrics
  - `=COUNTIF(range,criteria)` - Count deployment successes/failures
- **Pro Tips:** Use pivot tables for incident analysis by type
- **Common Pitfalls:** Ensure consistent date formats when calculating lead times

### Value Stream Mapping Templates
- **Purpose:** Visualize current state processes and identify waste
- **Key Components:**
  - Process boxes (activities)
  - Data boxes (time, quality metrics)
  - Flow arrows (handoffs)
- **Pro Tips:** Use different colors for value-add vs waste activities
- **Common Pitfalls:** Don't map ideal process - map what actually happens

### Interview Techniques for Process Discovery
- **Purpose:** Gather accurate information about current state
- **Key Questions:**
  - "Walk me through your typical..."
  - "What happens when..."
  - "How long does X usually take?"
- **Pro Tips:** Ask for specific recent examples, not generalizations
- **Common Pitfalls:** Don't lead witnesses - let them describe their reality

---

## 🔧 Common Issues & Solutions

### Issue #1: "Team members give inconsistent time estimates"
**Symptoms:** Different people report different lead times for same process
**Cause:** People estimate ideal case vs reality, or measure different things
**Solution:** 
```bash
# Use specific recent examples
"Tell me about the last feature you completed - what were the actual dates?"
# Cross-reference with project tracking tools
# Document both best-case and typical scenarios
```
**Prevention:** Always ask for specific examples with dates

### Issue #2: "Cannot access historical deployment data"
**Symptoms:** No clear record of when things were deployed to production
**Cause:** Informal deployment process, no tagging strategy
**Solution:**
```bash
# Check multiple sources:
git log --grep="prod\|release\|deploy" --since="6.months.ago"
# Email searches for deployment announcements
# Jira/ticket system for release records
# Interview team members for major release dates
```
**Prevention:** Establish deployment tracking from now forward

### Issue #3: "Value stream map looks too complex"
**Symptoms:** Map has too many branches and decision points
**Cause:** Trying to map every possible scenario instead of typical flow
**Solution:**
```markdown
Focus on the "happy path" - most common scenario
Map exceptions separately as variants
Ask: "What happens 80% of the time?"
Use swimlanes for different roles/teams
```
**Prevention:** Start with simple linear flow, add complexity gradually

---

## 🎭 Character Evolution This Week

### Legacy Luka's Journey
**Monday:** "Why do we need to measure everything? We know what the problems are."
**Wednesday:** "I never realized my code sits waiting for review for 2 days..."
**Friday:** "These numbers explain why my estimates are always wrong."
**Key Quote:** "I'm starting to see that my 'quick fixes' might be creating more problems."

### Anxious Ana's Discovery
**Monday:** "More documentation and meetings? I barely have time to test!"
**Wednesday:** "This shows I'm not the bottleneck I thought I was..."
**Friday:** "If we could automate the repetitive tests, I could focus on the complex scenarios."
**Key Quote:** "Maybe automation could be my friend, not my replacement."

### Firefighter Filip's Transformation
**Monday:** "We don't need metrics, we need better monitoring tools."
**Wednesday:** "These incident patterns... they're actually predictable."
**Friday:** "If we can predict these problems, maybe we can prevent them."
**Key Quote:** "I'm tired of being reactive. Let's try being proactive for once."

### Manager Maja's Realization
**Monday:** "How long will this assessment take? We have deadlines!"
**Wednesday:** "These metrics explain why our project estimates are always wrong."
**Friday:** "This data could help me give realistic commitments to clients."
**Key Quote:** "Maybe if we measure what we do, we can actually improve how we do it."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 1 Improvements:**

| Metric | Before This Week | After This Week | Target (Week 15) |
|--------|------------------|------------------|------------------|
| Lead Time | Unknown/Estimated | 30 days (measured) | 2 hours |
| Deployment Frequency | Unknown | Monthly (documented) | On-demand |
| MTTR | Unknown | 4 hours (baseline) | 5 minutes |
| Change Failure Rate | Unknown | 30% (measured) | 2% |
| Team Satisfaction | Unknown | 3/10 (baseline) | 9/10 |
| Process Visibility | 0% | 85% (value stream mapped) | 100% |

**Key Improvements:**
- **Visibility:** From zero process documentation to complete value stream map
- **Measurement:** From gut feelings to objective baseline metrics
- **Problem identification:** From blame to systematic root cause analysis

---

## 🇭🇷 Croatian IT Market Reality
Many Croatian software companies still operate without proper metrics or process visibility. This assessment approach is exactly what DevOps consultants use when entering companies like Infobip, Rimac, or Span. Your skills in measuring current state make you immediately valuable - most companies know they have problems but can't quantify them.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about VelocityTech's current state?** The amount of waiting time vs actual work time
2. **Which character's perspective helped you understand problems better?** How each role sees different aspects of the same issues
3. **How did measuring change your view of the problems?** Transformed blame into systematic understanding
4. **What questions do you still have?** How to prioritize which problems to fix first

**Team Discussion Points:**
- How would each VelocityTech character feel about these measurements?
- What resistance might we encounter when presenting these findings?
- How would you explain this week's discoveries to a skeptical manager?

**Preparation for Next Week:**
Next week we'll witness what happens when code collaboration breaks down completely - Git crisis incoming!

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Accelerate" by Forsgren, Humble, Kim - DORA metrics research
- "The DevOps Handbook" Chapters 1-3 - Foundation concepts

**Hands-on Practice:**
- Apply value stream mapping to any process in your life
- Practice DORA metrics calculation with open source projects

**Industry Insights:**
- "State of DevOps Report" (annual) - Industry benchmarking data
- Croatian IT salary surveys - See how DevOps skills impact compensation

---

*Next up: Week 2 - Version Control Crisis →*

> *"You can't improve what you don't measure. And you can't measure what you can't see. This week, we made the invisible visible."* - Marko, your mentor

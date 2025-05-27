# Week 2: Version Control Crisis - "Where's My Code?" 🔀
**VelocityTech Transformation Week 2**

**Story Context:** Critical bug fix gets lost, Luka blames "the new intern," Ana can't test without stable code  
**Focus:** Implementing collaborative Git workflows and establishing code review culture  
**Characters in Focus:** Luka's resistance to change, Ana's need for stability, Filip's deployment chaos

---

## 🔥 The Weekly Crisis: "The Great Code Disappearance"
**Setting:** VelocityTech office, Tuesday 8:45 AM. Coffee barely kicked in.  
**The Problem:** Three days of payment gateway work vanished overnight

**Scene:** [VelocityTech main development area. Luka staring at his screen with growing panic. Ana frantically searching through email attachments.]

**LUKA** *(voice rising with each word)*  
> "It's gone. Three days of work... just GONE!"  
> "I had the perfect payment gateway implementation. Worked flawlessly yesterday!"  
> *(frantically clicking through folders)* "Where the hell is my feature branch?!"

**ANA** *(digging through dozens of email attachments)*  
> "I have 'payment_gateway_final_v2_FINAL.zip' from yesterday..."  
> "But wait, here's 'payment_gateway_ACTUALLY_final.zip' from this morning?"  
> *(confused)* "Which one has your latest changes, Luka?"

**FILIP** *(on phone with IT support, frustrated)*  
> "What do you mean 'Git repository corruption'? Can't you just restore from backup?"  
> *(covering phone, to team)* "IT says our main branch somehow split into three different versions."  
> "Apparently someone did a 'force push' yesterday?"

**MAJA** *(entering the chaos, checking her calendar)*  
> "Please tell me this isn't affecting the client demo tomorrow..."  
> "We promised them the new payment integration! The contract depends on it!"

**LUKA** *(pointing at you accusingly)*  
> "This started happening after we got our new 'DevOps expert' intern!"  
> "Maybe your modern practices aren't so great after all!"

**MARKO** *(entering, immediately assessing the situation)*  
> "Let me guess - someone worked directly on main, someone else force-pushed, and now we have multiple versions of truth?"  
> *(to you)* "This is exactly why we need proper Git workflows. Ready for some repository archaeology?"

**ANA** *(overwhelmed)*  
> "I can't test anything if I don't know which version is correct!"  
> "And how do I know if my test results are even valid anymore?"

**FILIP** *(hanging up phone)*  
> "IT can't help. They said we need to 'sort out our Git workflow' ourselves."  
> "Whatever that means..."

**[All eyes turn to you. The new intern who supposedly knows about 'modern development practices'.]**

**MAJA** *(desperately)*  
> "Can you help us figure out what version we should be using?"  
> "And maybe... prevent this from happening again?"

**[Your systems thinking kicks in. This isn't just a technical problem - it's a collaboration breakdown.]**

---

## 🎯 What We'll Master This Week
**By the end of this week, you'll be able to:**
1. **Implement Git workflows that prevent code disasters** - Set up branching strategies and protection rules that make code loss nearly impossible
2. **Establish code review culture** - Create pull request processes that catch bugs before they reach production
3. **Resolve merge conflicts like a professional** - Handle collaborative development scenarios with confidence

**Real-world Connection:** Git collaboration skills are essential in Croatian IT companies. Many smaller companies still struggle with proper version control, while larger ones need advanced workflow expertise. These skills immediately differentiate you in the job market.

---

## 📚 The Knowledge Foundation

### Git Fundamentals and Why Version Control Matters
**The Problem it Solves:** Lost code, conflicting changes, no collaboration history, impossible rollbacks  
**How it Works:** Git tracks every change to your code, enables parallel development, and merges work safely
```
Working Directory → Staging Area → Local Repository → Remote Repository
     (edit code)    (git add)      (git commit)       (git push)
```

**Real-world Example:** Linux kernel development - thousands of developers collaborating on the same codebase  
**VelocityTech Connection:** Today's crisis shows what happens without proper version control - work disappears, conflicts arise, no way to track what changed

### Branching Strategies for Team Collaboration
**The Problem it Solves:** Developers interfering with each other's work, unstable main codebase, difficult releases  
**How it Works:** Isolated feature development with systematic integration

**GitHub Flow (Recommended for VelocityTech):**
```
main (always deployable)
├── feature/payment-gateway-luka
├── feature/user-testing-improvements-ana  
└── hotfix/login-timeout-filip
```

**Branch Naming Conventions:**
- `feature/short-description` - New functionality
- `bugfix/issue-description` - Bug fixes
- `hotfix/critical-issue` - Production emergencies
- `release/version-number` - Release preparation

**Branch Protection Rules:**
- No direct commits to main branch
- Require pull request reviews before merge
- Require status checks to pass
- Require branches to be up to date before merge

**Real-world Example:** Microsoft uses similar branching strategies across all their products  
**VelocityTech Connection:** Proper branching would have prevented Luka's work from conflicting with others

### Code Review Culture and Pull Request Best Practices
**The Problem it Solves:** Bugs reaching production, knowledge silos, inconsistent code quality  
**How it Works:** Systematic peer review before code integration

**Pull Request Lifecycle:**
1. Developer creates feature branch
2. Commits changes with clear messages
3. Opens pull request with description
4. Team reviews code and provides feedback
5. Developer addresses feedback
6. Code gets approved and merged

**Effective Code Reviews Focus On:**
- **Logic correctness** - Does the code do what it's supposed to?
- **Security concerns** - Any potential vulnerabilities?
- **Performance implications** - Will this slow things down?
- **Code consistency** - Follows team standards?
- **Test coverage** - Are there appropriate tests?

**Real-world Example:** Google requires all code changes to be reviewed - it's part of their culture  
**VelocityTech Connection:** Code reviews would have caught the issues in Luka's payment gateway before they caused problems

**Key Takeaways:**
- Git workflows prevent most collaboration disasters
- Branch protection rules enforce quality gates
- Code reviews catch bugs and spread knowledge
- Proper naming conventions reduce confusion

---

## 🛠️ Lab Mission: "The Git Rescue Operation"
**Scenario:** VelocityTech's payment gateway code is scattered across different branches and email attachments  
**Your Mission:** Recover the work, establish proper Git workflow, and prevent future disasters  
**Success Criteria:** Working payment gateway with proper Git history and team workflow established

### Phase 1: Repository Recovery (45 minutes)
**Objective:** Reconstruct VelocityTech's lost work and create clean Git history

**Steps:**
1. **Assess the Damage**
   ```bash
   # Clone the corrupted repository
   git clone https://github.com/velocitytech/payment-system
   cd payment-system
   
   # Investigate what we're dealing with
   git log --oneline --graph --all
   git branch -a
   git reflog
   ```
   - Expected output: Multiple conflicting branches, messy history
   - Troubleshooting: If repository won't clone, initialize new repo from backup

2. **Recover Lost Commits**
   ```bash
   # Find Luka's lost work using reflog
   git reflog --all | grep -i payment
   
   # Examine potential lost commits
   git show <commit-hash>
   
   # Cherry-pick valuable commits to recovery branch
   git checkout -b recovery/payment-gateway
   git cherry-pick <commit-hash>
   ```
   - Expected output: Recovered payment gateway code
   - Troubleshooting: If commits are corrupted, manually reconstruct from email attachments

3. **Create Clean Branch Structure**
   ```bash
   # Establish new main branch from stable point
   git checkout -b new-main <stable-commit>
   
   # Create development branch
   git checkout -b develop
   
   # Create proper feature branches
   git checkout -b feature/payment-gateway-reconstruction
   ```
   - Expected output: Clean branch structure ready for team use
   - Troubleshooting: If no stable commit exists, start from earliest working version

**Checkpoint:** Repository has clean structure with recovered payment gateway code

### Phase 2: Establish GitHub Flow Workflow (90 minutes)
**Objective:** Implement collaborative Git workflow that prevents future disasters

**Steps:**
1. **Set Up Branch Protection**
   ```bash
   # Push clean branches to remote
   git push -u origin new-main
   git push -u origin develop
   git push -u origin feature/payment-gateway-reconstruction
   
   # Configure branch protection via GitHub web interface:
   # - Require pull request reviews
   # - Require status checks
   # - Restrict pushes to main
   ```
   - Expected output: Protected main branch, enforced PR workflow
   - Troubleshooting: If you don't have admin access, document the settings needed

2. **Team Workflow Simulation**
   ```bash
   # Each team member gets their own feature branch
   git checkout develop
   git checkout -b feature/luka-payment-integration
   
   # Make realistic changes
   echo "class PaymentProcessor {" > payment.js
   echo "  processPayment(amount, method) {" >> payment.js
   echo "    // Luka's implementation" >> payment.js
   echo "  }" >> payment.js
   echo "}" >> payment.js
   
   # Commit with good practices
   git add payment.js
   git commit -m "Add basic payment processor structure
   
   - Implements core PaymentProcessor class
   - Supports amount and method parameters
   - Ready for specific payment gateway integration"
   ```

3. **Pull Request Creation and Review**
   ```markdown
   # Pull Request Template (create in GitHub)
   
   ## What does this PR do?
   Implements basic payment processor structure for the new gateway integration
   
   ## Why is this change needed?
   Addresses the payment gateway requirements from client demo
   
   ## How was this tested?
   - Unit tests added for PaymentProcessor class
   - Manual testing with sample payment data
   - Integration test with test payment gateway
   
   ## Screenshots/Demo
   [Include relevant screenshots or demo GIFs]
   
   ## Checklist
   - [x] Code follows team style guidelines
   - [x] Tests added/updated
   - [x] Documentation updated
   - [x] No merge conflicts
   - [x] Ready for review
   ```

4. **Code Review Process**
   ```bash
   # Reviewer checks out the PR branch
   git checkout feature/luka-payment-integration
   git pull origin feature/luka-payment-integration
   
   # Review the changes
   git diff develop..feature/luka-payment-integration
   
   # Test the changes locally
   npm test  # or appropriate test command
   
   # Provide feedback via GitHub PR interface
   ```

**Checkpoint:** Working PR workflow with team participation and review

### Phase 3: Conflict Resolution Workshop (45 minutes)
**Objective:** Practice handling merge conflicts professionally

**Steps:**
1. **Create Realistic Conflicts**
   ```javascript
   // Scenario: Both Luka and Ana modified the same payment function
   // Luka's version (feature/payment-gateway):
   function calculateTotal(price, tax) {
       return price + (price * tax);
   }
   
   // Ana's version (feature/testing-improvements):
   function calculateTotal(price, taxRate, discount) {
       return (price - discount) * (1 + taxRate);
   }
   ```

2. **Resolve Conflicts Step by Step**
   ```bash
   # Attempt to merge Ana's changes
   git checkout feature/payment-gateway
   git merge feature/testing-improvements
   
   # Git shows conflict
   Auto-merging payment.js
   CONFLICT (content): Merge conflict in payment.js
   Automatic merge failed; fix conflicts and then commit the result.
   
   # Examine the conflict
   git status
   git diff
   ```

3. **Manual Conflict Resolution**
   ```javascript
   // Edit the conflicted file
   <<<<<<< HEAD
   function calculateTotal(price, tax) {
       return price + (price * tax);
   }
   =======
   function calculateTotal(price, taxRate, discount) {
       return (price - discount) * (1 + taxRate);
   }
   >>>>>>> feature/testing-improvements
   
   // Resolve to combined solution:
   function calculateTotal(price, taxRate, discount = 0) {
       const discountedPrice = price - discount;
       return discountedPrice * (1 + taxRate);
   }
   ```

4. **Complete the Merge**
   ```bash
   # Stage the resolved file
   git add payment.js
   
   # Complete the merge
   git commit -m "Resolve merge conflict in calculateTotal function
   
   - Combined Luka's tax calculation with Ana's discount feature
   - Made discount parameter optional for backward compatibility
   - Maintained both functionalities"
   
   # Push the resolution
   git push origin feature/payment-gateway
   ```

**Checkpoint:** Successfully resolved conflicts with both features preserved

### Validation & Testing
**How to verify everything works:**
1. **Git Workflow Test** - `git log --oneline --graph` shows clean history
2. **Branch Protection Test** - `Direct push to main should be rejected`
3. **PR Process Test** - `Code review process completed successfully`

**Expected Results:**
- **Repository Recovery:** All lost work recovered and organized
- **Branch Protection:** Main branch protected, PRs required
- **Team Workflow:** All members comfortable with new process
- **Conflict Resolution:** Team can handle merge conflicts professionally

---

## 🔧 New Tools in Your Arsenal

### Git Command Line Interface
- **Purpose:** Full control over version control operations
- **Key Commands:**
  - `git log --oneline --graph --all` - Visualize repository history
  - `git reflog` - Find lost commits
  - `git cherry-pick <hash>` - Apply specific commits
- **Pro Tips:** Use aliases for common complex commands
- **Common Pitfalls:** Never use `git push --force` on shared branches

### GitHub Pull Request Workflow
- **Purpose:** Collaborative code review and integration
- **Key Features:**
  - Draft PRs for work-in-progress
  - Review comments and suggestions
  - Auto-merge when requirements met
- **Pro Tips:** Keep PRs small and focused for easier review
- **Common Pitfalls:** Don't accumulate too many commits before creating PR

### Branch Protection Rules
- **Purpose:** Enforce quality gates and prevent direct main branch changes
- **Key Settings:**
  - Require PR reviews before merge
  - Require status checks to pass
  - Restrict who can push to branch
- **Pro Tips:** Start with basic rules, add more as team matures
- **Common Pitfalls:** Don't make rules so strict that they block legitimate work

---

## 🔧 Common Issues & Solutions

### Issue #1: "Merge conflict in binary files"
**Symptoms:** Git cannot automatically merge files like images or documents
**Cause:** Two developers modified the same binary file
**Solution:**
```bash
# Choose which version to keep
git checkout --ours filename.png    # Keep your version
git checkout --theirs filename.png  # Keep their version
git add filename.png
git commit -m "Resolve binary file conflict"
```
**Prevention:** Coordinate binary file changes, use different file names when possible

### Issue #2: "Accidentally committed to main branch"
**Symptoms:** Made commits directly on main instead of feature branch
**Cause:** Forgot to create/switch to feature branch
**Solution:**
```bash
# Undo the commit but keep changes
git reset --soft HEAD~1

# Create proper feature branch
git checkout -b feature/accidental-work

# Re-commit on correct branch
git add .
git commit -m "Move work to proper feature branch"
```
**Prevention:** Always check current branch before starting work

### Issue #3: "Pull request has too many commits"
**Symptoms:** PR history is messy with many small, meaningless commits
**Cause:** Committing too frequently without squashing
**Solution:**
```bash
# Interactive rebase to squash commits
git rebase -i HEAD~5  # Squash last 5 commits

# In editor, change 'pick' to 'squash' for commits to combine
# Edit commit message to describe overall change
```
**Prevention:** Plan commits to represent logical units of work

---

## 🎭 Character Evolution This Week

### Legacy Luka's Journey
**Tuesday:** "Git is too complicated. Why can't we just use folders and ZIP files?"
**Wednesday:** "I admit, having my own branch means Ana can't accidentally break my code..."
**Thursday:** "This pull request actually caught three bugs I missed. Annoying but... helpful."
**Friday:** "I can't believe I used to email ZIP files. This branching thing makes sense now."
**Key Quote:** "Maybe there's something to this collaboration thing after all."

### Anxious Ana's Discovery
**Tuesday:** "Another tool to learn? I barely have time for testing as it is!"
**Wednesday:** "Actually, stable branches mean I can test without code changing under me..."
**Thursday:** "Code reviews help me understand what I'm testing much better."
**Friday:** "Git blame shows me exactly who to ask about confusing code."
**Key Quote:** "Stable code makes testing so much easier. Who knew?"

### Firefighter Filip's Transformation
**Tuesday:** "Great, another system to maintain. More complexity, more problems."
**Wednesday:** "Branch protection prevents a lot of the disasters I usually fix..."
**Thursday:** "I can roll back bad changes in seconds instead of hours."
**Friday:** "My weekend emergency calls have dropped significantly."
**Key Quote:** "Prevention is way better than cure. Even for code."

### Manager Maja's Realization
**Tuesday:** "How long will it take to implement this Git workflow?"
**Wednesday:** "Pull requests give me visibility into what's actually being developed."
**Thursday:** "I can see exactly why features are delayed - no more mystery."
**Friday:** "Clients love that I can show them exact change history."
**Key Quote:** "Transparency makes my job so much easier."

---

## 📊 Progress Metrics: VelocityTech Transformation
**Week 2 Improvements:**

| Metric | Week 1 Baseline | After This Week | Target (Week 15) |
|--------|------------------|------------------|------------------|
| Lead Time | 30 days | 28 days | 2 hours |
| Code Conflicts | Daily | Weekly | Rare |
| Lost Work Incidents | Weekly | Zero | Zero |
| Code Review Coverage | 0% | 85% | 100% |
| Deployment Rollbacks | 30% | 25% | 2% |
| Developer Satisfaction | 3/10 | 5/10 | 9/10 |
| Time to Deploy Fix | 4 hours | 2 hours | 5 minutes |

**Key Improvements:**
- **Collaboration:** From email attachments to proper Git workflow
- **Code Quality:** 85% of changes now peer-reviewed
- **Risk Reduction:** Eliminated lost work incidents through proper version control

---

## 🇭🇷 Croatian IT Market Reality
Git proficiency varies widely across Croatian companies. Smaller companies often still rely on email-based code sharing or basic file sharing, while larger companies struggle with proper branching strategies. Your advanced Git workflow skills make you immediately valuable - most Croatian developers know basic Git but lack collaborative workflow expertise.

---

## 🤔 Weekly Retrospective: "What Did We Learn?"
**Reflection Questions:**
1. **What surprised you most about the Git crisis?** How quickly collaboration breaks down without proper workflows
2. **Which character's resistance was most challenging?** Luka's initial skepticism about "unnecessary complexity"
3. **How did code reviews change team dynamics?** Shifted from blame to shared responsibility
4. **What questions do you still have?** How to handle more complex merge scenarios

**Team Discussion Points:**
- How would each VelocityTech character feel about mandatory code reviews?
- What resistance might we encounter from other teams in the company?
- How would you convince a skeptical manager to invest time in Git training?

**Preparation for Next Week:**
Ana arrives Monday morning to find 47 new bug reports from the weekend. "I can't manually test every scenario," she says, overwhelmed. The testing nightmare begins...

---

## 📚 Want to Learn More?
**Essential Reading:**
- "Pro Git" by Scott Chacon - Comprehensive Git guide
- "Git Flow" by Vincent Driessen - Popular branching model

**Hands-on Practice:**
- Practice merge conflict resolution with sample repositories
- Set up your own Git workflow for personal projects

**Industry Insights:**
- "GitHub State of the Octoverse" - Annual collaboration trends
- Croatian developer surveys - See how Git skills impact salaries

---

*Next up: Week 3 - Testing Nightmare →*

> *"The best branching strategy is the one your team actually follows. The best code review is the one that teaches something. The best merge is the one that brings people together."* - Git Philosophy

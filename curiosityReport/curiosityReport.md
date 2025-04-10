# Curiosity Report: Netflix’s DevOps Strategy

## Why I Chose This Topic

I chose to research Netflix's DevOps strategy simply because I love Netflix. I watch it all the time, and I was curious how such a massive platform runs so smoothly, releases new features often, and rarely seems to go down. I wanted to understand what technologies and practices make that possible.

---

## What I Learned

### 1. Netflix's Move to the Cloud

Netflix experienced a massive service outage in 2008 due to database corruption. This led them to move away from traditional data centers and migrate entirely to the cloud using AWS. Instead of lifting and shifting, they rebuilt their entire system to be cloud-native. This decision enabled them to:

- Adopt microservices architecture
- Scale services horizontally
- Deploy updates faster
- Let each team build and deploy independently

---

### 2. DevOps Culture: “You Build It, You Run It”

Netflix doesn’t have a separate operations team. Developers are responsible for the entire lifecycle of their code—from writing to deploying to maintaining it. This philosophy encourages:

- Ownership and accountability
- Faster feedback and response
- Higher-quality software and fewer silos

---

### 3. Chaos Engineering and the Simian Army

To prepare for failures, Netflix created a tool called **Chaos Monkey** that randomly shuts down production instances to test how resilient their system is. This idea expanded into a suite of tools called the **Simian Army**, including:

- **Latency Monkey**: Simulates network delays
- **Conformity Monkey**: Finds and removes misconfigured instances
- **Security Monkey**: Checks for security issues
- **Chaos Gorilla**: Simulates entire AWS zone failures

These tools help Netflix build systems that can recover automatically and continue operating even when failures occur.

---

### 4. Containers and Titus

Netflix developed its own container orchestration platform called **Titus**. It helps developers:

- Package applications consistently
- Deploy code quickly
- Run batch jobs and services efficiently
- Reduce infrastructure waste and boost productivity

---

### 5. Full-Cycle Developers and Centralized Tooling

Netflix trains its developers to manage the full software lifecycle. To support this, they provide powerful internal tools for testing, deployment, monitoring, and debugging. This reduces dependency on centralized ops teams and speeds up innovation.

---

## Key Takeaways

- **DevOps is a culture, not a checklist**: Netflix didn’t set out to “do DevOps” — it developed naturally through solving real problems.
- **Ownership builds better software**: When developers are responsible for operating what they build, the quality improves.
- **Failure is part of the plan**: Chaos Engineering ensures that unexpected problems don’t take the service down.
- **Tools support people, not control them**: Netflix gives engineers freedom and trust, supported by helpful tooling.

---

## Conclusion

Netflix is more than just a streaming platform—it’s an example of modern engineering done right. I learned that what makes Netflix strong isn’t just AWS or containers, but the mindset of its teams: automate, experiment, recover, and innovate fast. That mindset is something I want to bring into my own approach to software engineering.


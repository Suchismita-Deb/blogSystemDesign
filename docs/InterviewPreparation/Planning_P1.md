Weeks	Area	Priority
1	Linux + Git + Shell	🔴 Must
2	Networking fundamentals	🔴 Must
3–4	GCP	🔴 Must
5	Terraform + Ansible	🔴 Must
6–8	Docker + Kubernetes	🔴 Must
9	CI/CD + Harness + GitHub Actions	🔴 Must
10	Observability	🔴 Must
11	Platform Engineering + Developer Experience	🔴 Must
12	Ephemeral Environments	🔴 Must
13	Security + Financial-services mindset	🔴 Must
14	Go/Python + AI/MCP + System Design	🟠 Differentiator


Core 



Linux
Networking
GCP
Terraform
Docker
Kubernetes
CI/CD
Observability
Programming
Platform engineering

Advanced.
Ephemeral environments
Security
Distributed systems
Service mesh
Developer portals
AI/LLM automation
MCP/agents
Cloud cost/reliability
### Linux + Git + Shell.

Week 1.

### Linux.
processes
threads
signals
systemd
services
filesystems
permissions
users/groups
SSH
environment variables
/proc
CPU/memory/disk
processes
logs

Command.
ps
top
htop
free
df
du
lsblk
mount
lsof
ss
ip
curl
dig
grep
awk
sed
find
xargs
journalctl
systemctl


Scenario based - Server CPU is 100%. What do you do?

Application is running but port isn't reachable.

Disk is full.

Memory is exhausted.

Process keeps restarting.


### Git.

branch
merge
rebase
cherry-pick
reset
revert
stash
bisect
tags
Command.
git log
git diff
git blame
git bisect

### Networking.

### Publicis Sapient.

𝗣𝗔𝗥𝗧 𝟭: 𝗖𝗢𝗥𝗘 𝗝𝗔𝗩𝗔, 𝟵𝟬 𝗠𝗜𝗡𝗨𝗧𝗘𝗦
→ Coding: find unique characters in a string and return them in sorted order
→ Implement a Singleton. Eager vs lazy initialization, with deep discussion
→ Implement a Builder. Use cases and immutability
→ Output-based OOP questions
→ Thread lifecycle, synchronization, volatile vs Atomic, and race conditions with real-world scenarios
→ GC deep dive: G1, ZGC, heap structure, tuning fundamentals, and stop-the-world events
→ Java 8+: functional interfaces, Streams, lambdas, and method references
→ Serialization internals, Serializable vs Externalizable, and preventing serialization issues
→ HashMap internals: resizing, treeification; HashSet, LinkedHashSet, complexity, and use cases
→ JUnit and Mockito fundamentals, mocking vs stubbing

𝗣𝗔𝗥𝗧 𝟮: 𝗦𝗣𝗥𝗜𝗡𝗚 𝗕𝗢𝗢𝗧, 𝟯𝟬 𝗠𝗜𝗡𝗨𝗧𝗘𝗦
→ Design a controller class following best practices
→ Exception handling
→ API versioning: URI-based vs header-based
→ Bean scopes: singleton, prototype, request, and session

𝗣𝗔𝗥𝗧 𝟯: 𝗦𝗣𝗥𝗜𝗡𝗚 𝗦𝗘𝗖𝗨𝗥𝗜𝗧𝗬, 𝟭𝟱 𝗠𝗜𝗡𝗨𝗧𝗘𝗦
→ JWT authentication end to end
→ Implement a JWT filter
→ Secure endpoints and explain the token lifecycle from generation to validation

𝗣𝗔𝗥𝗧 𝟰: 𝗗𝗔𝗧𝗔𝗕𝗔𝗦𝗘 𝗔𝗡𝗗 𝗦𝗤𝗟, 𝟯𝟬 𝗠𝗜𝗡𝗨𝗧𝗘𝗦
→ SQL: identify users who purchased items worth more than 10K in a selected month of the previous year
→ JPA queries
→ Pagination strategies for datasets exceeding one million rows

𝗣𝗔𝗥𝗧 𝟱: 𝗦𝗬𝗦𝗧𝗘𝗠, 𝗜𝗡𝗙𝗥𝗔, 𝗔𝗥𝗖𝗛𝗜𝗧𝗘𝗖𝗧𝗨𝗥𝗘, 𝗧𝗛𝗘 𝗥𝗘𝗦𝗧
→ Redis caching strategies and use cases
→ Sharding concepts
→ Cloud infrastructure and project architecture discussion
→ Retrieving large datasets without excessive memory consumption
→ API performance optimization
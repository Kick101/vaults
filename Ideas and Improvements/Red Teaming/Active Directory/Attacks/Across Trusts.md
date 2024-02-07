![[Pasted image 20240202154909.png]]

>sIDHistory is a user attribute designed for scenarios where a user is moved from one domain to another. When a user's domain is changed, they get a new SID and the old SID is added to sIDHistory.

__sIDHistory can be abused in two ways of escalating privileges within a forest:__
- krbtgt hash of the child
- Trust ticket
---
### Child to Trust Flow
![[Pasted image 20240107115806.png]]



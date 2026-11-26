# Problem statement (draft)

> Draft for review. The original `problem-statement.md` is untouched.
> Anything in **[confirm]** is a guess that Nitin should replace with a real number
> or a real example from a cluster he has worked on.

## 1. Who has this problem

On-call platform and SRE engineers who support Kubernetes clusters, plus the
application developers they page. **[confirm: how many clusters, how many
engineers, which company or which lab setup]**

A second group matters: engineers who are new to Kubernetes. They carry the same
pager but not the same experience.

Nitin: there are 3 clusters  for dev /stage and prod environment which is supported by 15-20 engineers .. they have mixed levels of k8s experience 
it is for one of the uk telecom operator .

## 2. What happens today

A Kubernetes incident starts with an alert or a user complaint. The engineer then
reads several sources by hand, one `kubectl` command at a time: pod events, pod
and container logs, node status, services, endpoints, deployments, resource usage,
and what changed recently.

There is no agreed order for this. Each engineer invents a path through it. The
path depends on what they have seen before.

## 3. Why that is a problem

**It is slow.** Each source is a separate command and a separate screen. Logs run
to thousands of lines. The line that explains the incident is usually buried under
later errors that it caused. **[confirm: typical minutes from alert to root cause
today, even a rough figure from memory]**

**It needs rare knowledge.** A single incident can span Kubernetes, networking,
storage, the application and the infrastructure under it. Few engineers hold all
five. A newcomer often cannot decide where to start, so the first twenty minutes
are spent choosing a direction rather than following one.

**It does not reuse what we already learned.** The same failure has usually been
seen before, but past RCA notes and runbooks sit in wikis and tickets that nobody
can find under pressure.

**It is inconsistent.** Two engineers given the same incident investigate it two
different ways and can reach two different conclusions.

**The record is written last, or never.** RCA documents are typed up after the
incident, from memory, once the pressure is off. That is when they are least
accurate and most likely to be skipped.

Nitin:- typically 2+ hours by experienced enginner 

 

## 4. What good would look like

A measurable target, so the project can be judged rather than admired.

| Measure | Today | Target |
| --- | --- | --- |
| Time from alert to a stated root cause | **[1-2 hours]** | **[less than 5 ]** |
| Sources an engineer opens by hand | 8 or more | 0 for the first pass |
| Incidents with an RCA record written | **[100% drafted automatically]** | every one, drafted automatically |
| Same incident, two engineers, same conclusion | not guaranteed | the stated cause and its evidence are identical |

Each answer must name the evidence it rests on. A root cause with no pod name, no
timestamp and no log line is an opinion, and an engineer cannot act on it.



## 5. What is in scope

The first version explains incidents in a running cluster and writes the record of
what it found. **[confirm: which failure families first, for example crash loops,
image pull failures, OOM kills, failed scheduling, service and endpoint mismatches]**

Picking three or four families and doing them properly beats covering everything
shallowly.

Nitin :-

1. crashloopbackoff
2.ImagepullBackoff
3. DNS failures 

## 6. What is out of scope, on purpose

It does not fix anything. It does not restart, scale, roll back, patch or delete.
It reads the cluster and reports. **[confirm this is the intent]**

Anything that writes to a production cluster needs a human to approve it, and that
is a later phase, not this one.

Nitin: Any action that modifies a production cluster remains a human decision
automated remediation can be considered post approvals

## 7. Constraints

Read-only access to the cluster. No secrets, no pod contents and no customer data
leave the cluster boundary without a decision made on purpose. Logs often carry
personal data, so whatever is sent to a model has to be known and limited.
**[confirm: which clusters he can actually reach, and what he is allowed to send
off-cluster]**

Nitin:- we will not have an access to customer eks environment we may need to create our own and simulate

## 8. How we will know it worked

Replay past incidents whose cause is already known, and check whether the stated
cause matches. **[confirm: can he get a set of real past incidents, or does he need
to break a test cluster on purpose and use those?]**

A wrong answer stated confidently is worse than no answer, so the measure is not
only "did it find the cause" but "did it stay quiet when it did not know".

Nitin:- need to break a test cluster on purpose and use it .
succes criteria :
RCA generation success > 95% 
Correct root cause identification
problem remediation automatically 

## 9. Open questions

1. Which cluster is the first one, and who owns it?
2. Which three failure families come first?
3. Where do past RCA notes live today, and can they be read?
4. Who is the first real user, and when do they try it?

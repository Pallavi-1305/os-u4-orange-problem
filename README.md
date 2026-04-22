# PES-VCS Lab Report

## Student Details

* Name: Pallavi Rajkumar Bubanale
* SRN: PES1UG24CS314
* Course: Operating Systems

---

# Project Title

Building PES-VCS — A Version Control System from Scratch

---

# Objective

To build a local version control system that tracks file changes, stores snapshots efficiently, and supports commit history using operating system and filesystem concepts.

---

# Screenshots Section

## Phase 1: Object Storage Foundation

### Screenshot 1A: `./test_objects` output showing all tests passing

<img width="875" height="383" alt="image" src="https://github.com/user-attachments/assets/05673233-82f1-4788-9142-34d018dd5462" />


### Screenshot 1B: `find .pes/objects -type f`

<img width="821" height="152" alt="image" src="https://github.com/user-attachments/assets/1f4eeb80-2c23-4811-9e51-fcb0f0b425c6" />


---

## Phase 2: Tree Objects

### Screenshot 2A: `./test_tree` output showing all tests passing

<img width="636" height="237" alt="image" src="https://github.com/user-attachments/assets/6b7011f5-238e-427f-abee-dc7d187cfcda" />


### Screenshot 2B: Raw tree object using `xxd`

Command:

```bash
xxd .pes/objects/XX/YYY... | head -20
```

<img width="882" height="232" alt="image" src="https://github.com/user-attachments/assets/58694809-6ba0-42ff-a0e6-da00a77a82c3" />


---

## Phase 3: Index (Staging Area)

### Screenshot 3A: `./pes init` → `./pes add` → `./pes status`

<img width="797" height="765" alt="image" src="https://github.com/user-attachments/assets/72db252b-00d5-407b-8c32-f8d6905ff165" />


### Screenshot 3B: `cat .pes/index`

<img width="875" height="271" alt="image" src="https://github.com/user-attachments/assets/446d39ea-29d5-4978-a085-c31b09c4568c" />



---

## Phase 4: Commits and History

### Screenshot 4A: `./pes log`

<img width="775" height="711" alt="image" src="https://github.com/user-attachments/assets/83da8e0c-d69d-4a2b-940e-9d01fb050326" />


### Screenshot 4B: `find .pes -type f | sort`

<img width="842" height="362" alt="4b" src="https://github.com/user-attachments/assets/e0ff8403-2bc9-4b9c-ba94-ae25f82b4ee0" />


### Screenshot 4C: `cat .pes/refs/heads/main` and `cat .pes/HEAD`

<img width="766" height="142" alt="image" src="https://github.com/user-attachments/assets/d35e4ed5-ed13-435f-bf99-6833931efa17" />


---

## Final Integration Test

### Screenshot Final: `make test-integration`
<img width="953" height="913" alt="final 1" src="https://github.com/user-attachments/assets/c0338386-77cf-430a-bf76-451218c129c5" />

<img width="890" height="885" alt="image" src="https://github.com/user-attachments/assets/039e8be0-d4ef-4aef-9c96-e295124d0114" />
<img width="840" height="255" alt="image" src="https://github.com/user-attachments/assets/63eca962-8417-40b2-9293-b068fc697b38" />


---

# Analysis Questions

## Q5.1 Branch Checkout

A branch is a file containing the latest commit hash. To implement checkout, update HEAD to point to the selected branch and update the working directory files to match that branch's tree. It is complex because uncommitted changes may be overwritten.

## Q5.2 Dirty Working Directory Conflict

Compare the working directory metadata with the index and target branch tree. If a tracked file has local changes and also differs in the target branch, checkout should stop to prevent data loss.

## Q5.3 Detached HEAD

Detached HEAD means HEAD points directly to a commit instead of a branch. New commits can still be created, but they may become unreachable later. Recovery is possible by creating a new branch pointing to those commits.

## Q6.1 Garbage Collection

Start from all branch heads and traverse parent commits and trees recursively. Mark all reachable objects using a hash set. Delete objects not marked reachable. For 100,000 commits and 50 branches, many shared objects reduce repeated visits.

## Q6.2 GC Race Condition

If garbage collection runs during a commit, it may delete objects that the commit is about to reference. Real Git avoids this using locks, temporary references, and careful object reachability checks.

---

# Files Implemented

* object.c
* tree.c
* index.c
* commit.c

---

# Commit History Requirement

Minimum 5 commits per phase were maintained with meaningful messages.

---

# Conclusion

This lab demonstrated how version control systems use hashing, trees, staging areas, commits, and references to manage project history efficiently.

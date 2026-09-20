# Cloud & DevOps Mock Technical Interview Q&A & Answer Keys

---

## 🎯 Question 1: A Kubernetes Pod is stuck in `CrashLoopBackOff`. How do you systematically debug it in production?
* **Category**: Kubernetes Troubleshooting

### 💡 Answer Key & Step-by-Step Diagnostic Protocol
1. **Inspect High-Level Status**:
   ```bash
   kubectl get pods -n <namespace> -o wide
   ```
2. **Examine Events & Exit Codes**:
   ```bash
   kubectl describe pod <pod-name> -n <namespace>
   ```
   - Check `Last State` and `Exit Code`:
     * **Exit Code 137**: Pod was killed by the OS kernel OOMKilled (Out of Memory). Solution: Increase `resources.limits.memory`.
     * **Exit Code 1 / 2**: Application runtime crash (unhandled exception, syntax error, missing environment variable).
     * **Exit Code 0**: Container exited because the primary process PID 1 finished and terminated.
3. **Inspect Application Logs**:
   ```bash
   kubectl logs <pod-name> -n <namespace> --previous
   ```
   *Note: Using `--previous` is mandatory because the currently restarting container might not have generated logs yet.*
4. **Check Configuration & Secrets**:
   - Verify that all environment variables, ConfigMaps, and Secrets referenced in the pod specification actually exist in the target namespace.
5. **Interactive Debugging**:
   - If needed, spin up an ephemeral debug container:
     ```bash
     kubectl debug -it <pod-name> --image=busybox:latest --target=<container-name>
     ```

---

## 🎯 Question 2: Explain Blue-Green Deployment vs Canary Deployment. When would you choose one over the other?
* **Category**: Deployment Strategies

### 💡 Answer Key
| Parameter | Blue-Green Deployment | Canary Deployment |
| :--- | :--- | :--- |
| **How It Works** | Two identical production environments exist (Blue = Live, Green = Idle). Deploy v2 to Green, run smoke tests, then switch router/load balancer traffic 100% to Green instantly. | Gradually route a small percentage (e.g. 5%, then 25%, then 100%) of real production user traffic to the new v2 version while monitoring error rates. |
| **Rollback Speed** | Instant ($O(1)$) by flipping the load balancer router back to Blue. | Fast, but requires updating ingress traffic weights back to 0%. |
| **Resource Cost** | High (Requires $2\times$ 100% infrastructure capacity during deploy). | Low (Only requires small incremental compute for canary pods). |
| **Best Use Case** | Monoliths, database-coupled applications, financial systems where two versions cannot coexist. | High-volume consumer apps, microservices with automated telemetry rollback (e.g. Argo Rollouts). |

---

## 🎯 Question 3: How does AWS IAM policy evaluation work, and what is the precedence order?
* **Category**: Cloud Security & IAM Architecture

### 💡 Answer Key
AWS IAM evaluates permissions following a strict **Deny-by-default** evaluation model:
1. **Explicit Deny**: If any applicable policy (SCP, Permissions Boundary, Identity Policy, Resource Policy) contains an `"Effect": "Deny"`, the request is **immediately DENIED**. An explicit deny overrides all allows.
2. **Explicit Allow**: If no explicit deny exists, the evaluator checks for at least one explicit `"Effect": "Allow"`.
3. **Implicit Deny**: If no explicit allow is found, the request is **implicitly DENIED**.

---

## 🎯 Question 4: How do you prevent Terraform State drift and handle concurrent team runs?
* **Category**: Infrastructure as Code

### 💡 Answer Key
1. **Remote Backend**: Never store `terraform.tfstate` in Git or local machines. Store state in an encrypted cloud bucket (AWS S3 with versioning enabled).
2. **State Locking**: Integrate AWS DynamoDB table (`LockID` string primary key). When an engineer or CI runner executes `terraform apply`, Terraform acquires a lock in DynamoDB. Concurrent pipelines receive `Error: Error acquiring the state lock` and are safely rejected until the running apply finishes.
3. **Drift Detection**: Run automated scheduled CI workflows (`terraform plan -detailed-exitcode`). Exit code 2 indicates infrastructure has drifted from declared code, triggering an immediate Slack/PagerDuty alert.

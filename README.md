# 🚀 AWS CloudFormation Nested Stacks – Practical 2

## 📌 Project Overview

This project demonstrates the implementation of **Nested Stacks in AWS CloudFormation**.

A Root Stack orchestrates two Child Stacks:

- 🏗 Network Stack (Child 1)
- 🔐 Security Stack (Child 2)

The Root stack manages dependencies and parameter passing between the child stacks.

---

## 🏗 Architecture

Root Stack
│
├── Network Stack
│     ├── VPC
│     ├── 2 Public Subnets
│     └── 2 Private Subnets
│
└── Security Stack
      ├── Load Balancer Security Group (Ports 80 & 22)
      └── Web Server Security Group (Allows traffic only from LB SG)

---

## 📂 Project Structure

```
.
├── network.yaml
├── security.yaml
└── root.yaml
```

---

## 🧱 Stack Details

### 1️⃣ Network Stack (Child 1)

Creates:

- VPC (10.0.0.0/16)
- 2 Public Subnets
- 2 Private Subnets
- Internet Gateway

### Outputs:

- VPC ID
- Public Subnet IDs
- Private Subnet IDs

These outputs are passed to the Security Stack using:

```
!GetAtt NetworkStack.Outputs.VPCId
```

---

### 2️⃣ Security Stack (Child 2)

Accepts:

- VPC ID as parameter

Creates:

- Load Balancer Security Group  
  - Allows:
    - HTTP (Port 80)
    - SSH (Port 22)

- Web Server Security Group  
  - Allows traffic ONLY from Load Balancer Security Group  
  - Uses:
    ```
    SourceSecurityGroupId
    ```

---

### 3️⃣ Root Stack

Uses:

```
AWS::CloudFormation::Stack
```

Features:

- Calls both child stacks
- Uses `DependsOn` to ensure correct creation order
- Passes VPC ID dynamically
- Avoids hardcoding resource IDs

---

## 🔁 Parameter Passing Logic

Security Stack receives VPC ID from Network Stack:

```
VpcId: !GetAtt NetworkStack.Outputs.VPCId
```

This ensures:

- Loose coupling
- Reusability
- Modular architecture

---

## 🛠 Deployment Steps

1. Upload `network.yaml` and `security.yaml` to S3.
2. Update S3 URLs inside `root.yaml`.
3. Create stack in AWS CloudFormation using `root.yaml`.
4. Verify nested stacks inside the Root stack.

---

## 🎯 Key Concepts Demonstrated

- Nested Stacks
- Stack Dependencies (`DependsOn`)
- Cross-stack parameter passing
- Output exports
- Security Group referencing
- Infrastructure as Code (IaC)

---

## 🧠 Learning Outcome

After completing this practical, I understood:

- How Root stacks orchestrate child stacks
- How to pass parameters dynamically
- How to enforce stack creation order
- Best practices for modular CloudFormation templates

---

## 📌 Author

Suya  
AWS | DevOps | CloudFormation Practice

---

## 📝 Step-by-Step Implementation (Practical Execution)

Below are the exact steps followed to complete this practical:

### 🔹 Step 1: Created Network Stack Template (network.yaml)

- Defined a VPC with CIDR block `10.0.0.0/16`
- Created:
  - 2 Public Subnets
  - 2 Private Subnets
- Attached an Internet Gateway
- Defined Outputs:
  - VPC ID
  - Public Subnet IDs
  - Private Subnet IDs
 
<img width="1903" height="1019" alt="Screenshot 2026-02-27 225851" src="https://github.com/user-attachments/assets/783082d8-3a1a-4890-81ca-48c3644c190b" />

<img width="1887" height="972" alt="Screenshot 2026-02-27 225859" src="https://github.com/user-attachments/assets/4844fdfa-cea2-40a9-b33e-ad87258b9680" />


These outputs were later used in the Security Stack.

---

### 🔹 Step 2: Created Security Stack Template (security.yaml)

- Declared a parameter:
  ```
  VpcId
  ```
- Created:
  - Load Balancer Security Group
    - Allows Port 80 (HTTP)
    - Allows Port 22 (SSH)
  - Web Server Security Group
    - Allows traffic ONLY from Load Balancer Security Group
    - Used:
      ```
      SourceSecurityGroupId
      ```
      
<img width="1754" height="955" alt="Screenshot 2026-02-27 225830" src="https://github.com/user-attachments/assets/19ea5d15-7b74-42f2-b8ec-b18fa0390020" />


This ensured secure communication between resources.

---

### 🔹 Step 3: Uploaded Templates to S3

- Created an S3 bucket in the same region.
- Uploaded:
  - network.yaml
  - security.yaml
  
<img width="1918" height="921" alt="Screenshot 2026-02-27 230122" src="https://github.com/user-attachments/assets/cac8070d-8491-4168-9546-d3acdaef3857" />

<img width="1919" height="905" alt="Screenshot 2026-02-27 230136" src="https://github.com/user-attachments/assets/e4d40b75-3961-4604-8930-7bc6fbc4f0e4" />


- Copied their Object URLs.
- Updated those URLs inside `root.yaml`.

---

### 🔹 Step 4: Created Root Stack (root.yaml)

- Used:
  ```
  AWS::CloudFormation::Stack
  ```
- Defined two nested stacks:
  - NetworkStack
  - SecurityStack
- Used:
  ```
  DependsOn: NetworkStack
  ```
  to ensure proper creation order.
- Passed VPC ID dynamically using:
  ```
  VpcId: !GetAtt NetworkStack.Outputs.VPCId
  ```

  <img width="1664" height="830" alt="Screenshot 2026-02-27 230530" src="https://github.com/user-attachments/assets/ea513050-d0cf-4b83-b0a5-1e53f4a34718" />


This avoided hardcoding resource IDs.

---

### 🔹 Step 5: Deployed Root Stack in AWS

- Opened AWS Console → CloudFormation.

<img width="1919" height="908" alt="Screenshot 2026-02-27 230553" src="https://github.com/user-attachments/assets/b763553c-b2c1-4718-9614-a85fa1043246" />


<img width="1919" height="913" alt="Screenshot 2026-02-27 230615" src="https://github.com/user-attachments/assets/fd39b9a7-ac8e-429d-8b06-d65fa3da8dc9" />


<img width="1919" height="906" alt="Screenshot 2026-02-27 230627" src="https://github.com/user-attachments/assets/06538ca8-666f-4984-9845-7b9d29a793de" />

<img width="1919" height="910" alt="Screenshot 2026-02-27 230650" src="https://github.com/user-attachments/assets/8acf2315-d536-4871-b9de-a33844e03303" />


- Selected:
  - Upload template file.
  - Uploaded `root.yaml`.
- Provided stack name.
- 
 <img width="1913" height="916" alt="Screenshot 2026-02-27 230715" src="https://github.com/user-attachments/assets/456afea7-70ef-4b3a-b444-028f9cf8dc8a" />

- Clicked Create Stack.

<img width="1919" height="919" alt="Screenshot 2026-02-27 230904" src="https://github.com/user-attachments/assets/a30ed8ef-6324-4fcc-a6d3-88f9935a9b96" />

- Waited for status:
  ```
  CREATE_COMPLETE
  ```

---

### 🔹 Step 6: Verified Nested Stacks

- Opened Root Stack.
- Checked Resources tab.
- Confirmed:
  
  - NetworkStack created successfully.
  - SecurityStack created successfully.
  <img width="1919" height="919" alt="Screenshot 2026-02-27 230904" src="https://github.com/user-attachments/assets/b2a8eb5a-9eec-41fc-ab05-306068fe1b2b" />
  
    
- Verified:
  - VPC & Subnets
    <img width="1913" height="904" alt="Screenshot 2026-02-27 231138" src="https://github.com/user-attachments/assets/8bab99c7-9e52-431f-a2d7-d12c4677857b" />

<img width="1919" height="922" alt="Screenshot 2026-02-27 231156" src="https://github.com/user-attachments/assets/d3b8d373-b35b-4ac5-bef6-f192acb14880" />

  - Security Groups
<img width="1916" height="914" alt="Screenshot 2026-02-27 231216" src="https://github.com/user-attachments/assets/43e7cedb-c3f7-4f3f-b5c0-6687625c90ed" />

---

## ✅ Final Result

Successfully implemented:

- Nested Stack Architecture
- Stack Dependencies using `DependsOn`
- Cross-stack parameter passing using `!GetAtt`
- Secure Security Group referencing
- Modular Infrastructure as Code design

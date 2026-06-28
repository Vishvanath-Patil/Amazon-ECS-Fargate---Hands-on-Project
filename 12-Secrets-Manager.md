# Step 4 - AWS Secrets Manager
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/b86a03a0-fb8f-4995-ab13-36d176a448ce" />

## Objective
Store the database credentials securely instead of hardcoding them.

## Steps
1. Open AWS Secrets Manager.
2. Create a new secret.
3. Select database credentials.
4. Store:
   - Username
   - Password
5. Save the secret.

## Usage
The ECS task retrieves these credentials during application startup.

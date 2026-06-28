# Step 4 - AWS Secrets Manager

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

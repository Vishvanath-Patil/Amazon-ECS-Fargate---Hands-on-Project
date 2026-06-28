# Step 13 - Test the Application
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/fb5955b4-1a7d-4227-ba6a-3ef2a55aacd8" />

## Objective

Verify that the deployed application is accessible through the Application Load Balancer.

## Steps

1. Open the Amazon ECS console.
2. Confirm the ECS Service status is **Running**.
3. Verify the target group reports healthy targets.
4. Copy the Application Load Balancer DNS name.
5. Open the DNS name in a web browser.

## Expected Result

- Coffee Shop application home page loads.
- Beverage voting form is displayed.
- Users can enter:
  - Name
  - Email Address
  - Beverage Selection

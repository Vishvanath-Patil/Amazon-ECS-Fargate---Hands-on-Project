# Step 13 - Test the Application

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

# Step 2 - Security Groups

<img width="1690" height="931" alt="image" src="https://github.com/user-attachments/assets/cc6006d7-cef8-4a16-bb26-83ecf1e972df" />


## Application Load Balancer SG

Inbound
- HTTP (80) from Internet

Outbound
- All Traffic

## Application SG

Inbound
- HTTP (80) from ALB Security Group

Outbound
- All Traffic

## Database SG

Inbound
- MySQL (3306) from Application Security Group

Outbound
- Default

This allows only the application to access the database.

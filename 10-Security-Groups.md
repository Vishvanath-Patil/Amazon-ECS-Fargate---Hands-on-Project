# Step 2 - Security Groups

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

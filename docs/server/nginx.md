# Nginx Reverse Proxy Setup for Strapi with SSL

This guide provides step-by-step instructions for setting up Nginx as a reverse proxy for your Strapi application with SSL on your subdomain (`encyclopedia-cms.initiatehub.com`). We'll use Let's Encrypt with Certbot to obtain and manage the SSL certificate.

## Step 1: Configure DNS

Before we begin the server setup, we need to configure the DNS settings for your subdomain.

**Set Up DNS A Record**:

- Go to your domain registrar or DNS provider's control panel.
- Add an **A record** with the following details:
  - **Name**: `encyclopedia-cms` (or `encyclopedia-cms.initiatehub.com`, depending on your provider's format)
  - **Type**: A
  - **Value**: IP address of your VPS (e.g., `123.456.78.90`)
- Save the DNS settings and allow time for propagation (typically a few minutes to a few hours).

## Step 2: Install Nginx on Your VPS

Now, let's install and set up Nginx on your VPS.

**1. Install Nginx**:

```bash
sudo apt update
sudo apt install nginx -y
```

**2. Start and Enable Nginx**:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

This ensures Nginx starts automatically on system boot.

## Step 3: Configure Nginx as a Reverse Proxy for Strapi

Next, we'll configure Nginx to act as a reverse proxy for your Strapi application.

**1. Create Nginx Configuration for Strapi**:
Create a new configuration file:

```bash
sudo nano /etc/nginx/sites-available/strapi.conf
```

Add the following configuration (replace `encyclopedia-cms.initiatehub.com` with your subdomain):

```nginx
server {
    listen 80;
    server_name encyclopedia-cms.initiatehub.com;

    location / {
        proxy_pass http://localhost:1337;  # Strapi's internal port
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**2. Enable the Nginx Configuration**:

- Link the configuration file to `sites-enabled`:
  ```bash
  sudo ln -s /etc/nginx/sites-available/strapi.conf /etc/nginx/sites-enabled/
  ```
- Test Nginx configuration for syntax errors:
  ```bash
  sudo nginx -t
  ```
- Restart Nginx to apply the changes:
  ```bash
  sudo systemctl restart nginx
  ```

## Step 4: Obtain an SSL Certificate with Certbot (Let's Encrypt)

Now, we'll secure our site with an SSL certificate using Let's Encrypt and Certbot.

**1. Install Certbot and the Nginx Plugin**:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

**2. Obtain and Install the SSL Certificate**:

Run Certbot for your domain:

```bash
sudo certbot --nginx -d encyclopedia-cms.initiatehub.com
```

Follow the prompts:

- Agree to the terms of service.
- Choose whether to share your email with the Electronic Frontier Foundation (optional).
- Certbot will automatically configure Nginx to use the SSL certificate.
- When asked, select the option to redirect all HTTP traffic to HTTPS.

**3. Verify SSL Setup**:

- Visit `https://encyclopedia-cms.initiatehub.com` in your browser to ensure the SSL certificate is working correctly.
- Check the SSL certificate status:
  ```bash
  sudo certbot certificates
  ```

## Step 5: Set Up Automatic Renewal of SSL Certificate

Let's Encrypt certificates are valid for 90 days. It's important to set up automatic renewal to maintain HTTPS security.

**Verify Automatic Renewal**:

Certbot typically sets up a cron job for automatic renewal. You can test the renewal process with:

```bash
sudo certbot renew --dry-run
```

If successful, your certificates will automatically renew when needed.

## Conclusion

You have now successfully set up Nginx as a reverse proxy for your Strapi application, with SSL encryption on your subdomain `encyclopedia-cms.initiatehub.com`. The SSL certificate is managed by Certbot, which automatically configures Nginx to handle secure HTTPS connections.

Remember to keep your server updated and monitor your SSL certificate status regularly. If you encounter any issues or need to make changes to your Nginx configuration, you can edit the `/etc/nginx/sites-available/strapi.conf` file and restart Nginx to apply the changes.

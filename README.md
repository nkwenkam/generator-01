# Dongfeng Email Signature Generator

A multi-step form that allows employees to generate branded HTML email signatures for use in Roundcube webmail.

## Features

- **URL-based pre-population**: Invite links auto-fill name and email
- **Office selection**: Douala or Yaoundé with auto-populated addresses
- **Live preview**: See the signature before copying
- **One-click copy**: Copy HTML code ready for Roundcube

## Invite Link Format

```
https://generate.digitaall.app/?name=Nelson%20Nkwenkam&email=superadmin@dongfengelectric.com
```

URL Parameters:
- `name` - Full name (URL encoded)
- `email` - Email address (URL encoded)

## Deployment Options

### Option 1: Digital Ocean App Platform (Recommended - Simplest)

1. Push code to a GitHub repository
2. Go to Digital Ocean → Apps → Create App
3. Connect your GitHub repo
4. Select "Static Site" as the type
5. Set domain: `generate.digitaall.app`
6. Deploy

**Cost**: Free tier available (3 static sites)

### Option 2: Digital Ocean Spaces + CDN

1. Create a Space in Digital Ocean
2. Upload `index.html`
3. Enable CDN
4. Configure custom domain `generate.digitaall.app`
5. Point DNS CNAME to the Space CDN endpoint

**Cost**: ~$5/month

### Option 3: Nginx on Existing Droplet

If you have an existing droplet:

```bash
# Create directory
sudo mkdir -p /var/www/generate.digitaall.app

# Copy file
sudo cp index.html /var/www/generate.digitaall.app/

# Nginx config
sudo nano /etc/nginx/sites-available/generate.digitaall.app
```

```nginx
server {
    listen 80;
    server_name generate.digitaall.app;
    root /var/www/generate.digitaall.app;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/generate.digitaall.app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# SSL with Certbot
sudo certbot --nginx -d generate.digitaall.app
```

## DNS Configuration

Add to your DNS (Cloudflare/other):

```
Type: CNAME
Name: generate
Target: [your-digitalocean-endpoint]
```

Or for a Droplet:
```
Type: A
Name: generate
Target: [droplet-ip-address]
```

## Brand Assets Used

- **Logo**: https://res.cloudinary.com/woureesystems/image/upload/v1766066113/dongfeng/dongfeng_logo.png
- **Icon**: https://res.cloudinary.com/woureesystems/image/upload/v1766066112/dongfeng/dongfeng_icon.png
- **Primary Blue**: #081693
- **Accent Lime**: #D5F956

## Files

- `index.html` - Complete single-file application (no build required)

## Customization

To modify offices, social links, or branding, edit the configuration objects at the top of the `<script>` section in `index.html`:

```javascript
const OFFICES = {
  douala: { name: 'Douala', address: '...' },
  yaounde: { name: 'Yaoundé', address: '...' }
};

const BRAND = {
  blue: '#081693',
  lime: '#D5F956',
  // ...
};
```

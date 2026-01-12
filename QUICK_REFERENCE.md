# Goon-Co CMS - Quick Reference Guide

This guide provides quick commands and tips for common CMS operations.

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Access admin panel
# Navigate to: http://localhost:1337/admin
```

## 🔧 Common Commands

### Development

```bash
# Start with auto-reload
npm run dev

# Start without auto-reload
npm start

# Build admin panel
npm run build
```

### Database

```bash
# Reset database (development only - CAUTION: deletes all data!)
rm -rf .tmp/data.db

# The database will be recreated on next start
npm run dev
```

### Strapi CLI

```bash
# Generate new content type
npm run strapi generate

# Create admin user via CLI
npm run strapi admin:create-user

# Check Strapi version
npm run strapi version

# Upgrade Strapi
npm run upgrade
```

## 📊 Content Management

### Creating a Complete Goon Profile

1. **Create Specialty** (if not exists)

   - Go to: Content Manager → Specialties → Create new entry
   - Fill in: name, icon
   - Save & Publish

2. **Create Tools** (if not exists)

   - Go to: Content Manager → Tools → Create new entry
   - Fill in: name, icon
   - Save

3. **Create Goon**

   - Go to: Content Manager → Goons → Create new entry
   - Required fields:
     - Name
     - Rating (0-5)
     - Price (hourly rate)
     - Gender
     - Body Type
     - Bio
     - Image (upload)
     - Specialty (select from dropdown)
   - Optional:
     - Social Media URL
   - Save

4. **Link Tools to Goon**

   - Edit the Tool entries
   - Select the goon from the dropdown
   - Save each tool

5. **Add Reviews** (optional)

   - Go to: Content Manager → Reviews → Create new entry
   - Link to the goon
   - Save

6. **Add Previous Works** (optional)
   - Go to: Content Manager → Previous Works → Create new entry
   - Link to the goon
   - Save

## 🔌 API Access

### Testing API Endpoints

```bash
# Get all goons
curl http://localhost:1337/api/goons

# Get all goons with relations populated
curl http://localhost:1337/api/goons?populate=*

# Get specific goon
curl http://localhost:1337/api/goons/1

# Get goons filtered by gender
curl http://localhost:1337/api/goons?filters[gender][$eq]=Male

# Get goons with specialty
curl http://localhost:1337/api/goons?populate=specialty&filters[specialty][name][$eq]=Photography
```

### Enable Public API Access

1. Go to: Settings → Users & Permissions → Roles → Public
2. Enable permissions:
   - Goon: find, findOne
   - Specialty: find, findOne
   - Tool: find, findOne
   - Review: find, findOne
   - Previous-work: find, findOne
3. Save

## 🎯 Filtering & Querying

### Common Query Examples

```bash
# Pagination
?pagination[page]=1&pagination[pageSize]=10

# Sorting
?sort=rating:desc
?sort[0]=rating:desc&sort[1]=price:asc

# Filtering
?filters[gender][$eq]=Female
?filters[rating][$gte]=4.5
?filters[price][$lte]=50

# Populate relations
?populate=specialty
?populate[0]=specialty&populate[1]=tools
?populate=*  # Populate all relations (careful with performance)

# Combine queries
?filters[gender][$eq]=Male&populate=specialty&sort=rating:desc&pagination[pageSize]=5
```

## 🐛 Troubleshooting

### Port Already in Use

```bash
# Find process using port 1337
lsof -i :1337

# Kill the process
kill -9 <PID>

# Or change port in .env
PORT=3000
```

### Admin Panel Not Loading

```bash
# Rebuild admin panel
npm run build

# Clear .cache and dist folders
rm -rf .cache dist

# Restart
npm run dev
```

### Database Issues

```bash
# Development: Reset database
rm -rf .tmp/data.db

# Check database file
ls -lh .tmp/data.db
```

### Permission Denied Errors

```bash
# Fix file permissions
chmod -R 755 .

# Fix node_modules
rm -rf node_modules
npm install
```

## 📦 Environment Variables

### Required Variables

```env
# Server
HOST=0.0.0.0
PORT=1337

# Security (MUST be unique and random!)
APP_KEYS="<random-key-1>,<random-key-2>"
API_TOKEN_SALT=<random-salt>
ADMIN_JWT_SECRET=<random-secret>
TRANSFER_TOKEN_SALT=<random-salt>
JWT_SECRET=<random-secret>
ENCRYPTION_KEY=<random-key>
```

### Generate Secure Keys

```bash
# Generate a random key
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"

# Generate multiple keys at once
for i in {1..5}; do node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"; done
```

## 🚢 Production Deployment Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Configure production database (PostgreSQL recommended)
- [ ] Generate strong environment variables
- [ ] Run `npm run build`
- [ ] Set up HTTPS/SSL
- [ ] Configure CORS properly
- [ ] Set up backup strategy
- [ ] Enable rate limiting
- [ ] Configure monitoring/logging
- [ ] Test API endpoints
- [ ] Set up CDN for media files (optional)

## 🔗 Useful Links

- [Strapi Documentation](https://docs.strapi.io)
- [REST API Reference](https://docs.strapi.io/dev-docs/api/rest)
- [Admin Panel Guide](https://docs.strapi.io/user-docs)
- [Deployment Guide](https://docs.strapi.io/dev-docs/deployment)

## 💡 Tips & Best Practices

1. **Always backup** before making schema changes
2. **Use TypeScript** for type safety
3. **Populate relations selectively** to avoid performance issues
4. **Version control** your schema changes
5. **Test API endpoints** before deploying
6. **Use environment variables** for sensitive data
7. **Regular security audits**: `npm audit`
8. **Keep Strapi updated**: `npm run upgrade`

---

For detailed documentation, see [README.md](./README.md)

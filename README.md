# Makerspace Leiden MediaWiki

A MediaWiki installation for Makerspace Leiden, providing a collaborative knowledge base for the makerspace community.

## Overview

This MediaWiki instance serves as the central knowledge repository for Makerspace Leiden, allowing members to document projects, share knowledge, and collaborate on various maker-related topics.

## Features

- **Multi-language Support**: Dutch (nl) as primary language with translation capabilities
- **File Upload Support**: Supports various file formats including images, PDFs, CAD files, and more
- **Translation Extensions**: Includes Babel, CLDR, and LocalisationUpdate for multi-language content
- **Enhanced Editing**: WikiEditor with syntax highlighting and tabs support
- **Security**: Configured with proper access controls and spam protection

## Technical Details

- **MediaWiki Version**: 1.41.3
- **Database**: MySQL
- **Web Server**: Apache with PHP
- **URL**: https://wiki.makerspaceleiden.nl
- **Script Path**: /mediawiki

## Supported File Extensions

The wiki supports uploads of the following file types:
- Images: PNG, GIF, JPG, JPEG, SVG
- Documents: PDF
- CAD Files: SCAD, SCH, BRD, F3D
- Media: AVI, MOV, MPG
- Archives: ZIP
- Other: MCH

## Configuration

### Environment Variables

The application uses environment variables for sensitive configuration. Create a `.env` file with the following variables:

```env
WIKI_DB_HOST=your_database_host
WIKI_DB_NAME=your_database_name
WIKI_DB_USER=your_database_user
WIKI_DB_PASSWORD=your_database_password
WIKI_SECRET_KEY=your_secret_key
```

### Key Configuration Files

- `LocalSettings.php`: Main MediaWiki configuration
- `mediawiki.conf`: Apache web server configuration

## Installation & Setup

### Prerequisites

- PHP 8.x
- MySQL database
- Apache web server
- Composer (for dotenv dependency)

### Installation Steps

1. **Clone or download the MediaWiki files**
2. **Set up the database**
   ```sql
   CREATE DATABASE your_wiki_db;
   CREATE USER 'wiki_user'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON your_wiki_db.* TO 'wiki_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

3. **Configure environment variables**
   - Copy the example `.env` file and fill in your database credentials
   - Generate a secure secret key for `WIKI_SECRET_KEY`

4. **Set up Apache configuration**
   - Copy `mediawiki.conf` to your Apache sites-available directory
   - Enable the site: `a2ensite mediawiki`
   - Restart Apache: `systemctl restart apache2`

5. **Set proper permissions**
   ```bash
   chown -R www-data:www-data /var/lib/mediawiki
   chmod -R 755 /var/lib/mediawiki
   chmod -R 777 /var/lib/mediawiki/images
   chmod -R 777 /var/lib/mediawiki/cache
   ```

## Extensions

The following MediaWiki extensions are enabled:

- **Babel**: Multi-language user interface
- **CLDR**: Unicode Common Locale Data Repository
- **CleanChanges**: Enhanced recent changes display
- **ImageMap**: Clickable image maps
- **InputBox**: Input form elements
- **LocalisationUpdate**: Automatic language updates
- **PdfHandler**: PDF file handling
- **SpamBlacklist**: Spam protection
- **SyntaxHighlight_GeSHi**: Code syntax highlighting
- **Tabs**: Tabbed interface
- **UniversalLanguageSelector**: Language selection interface
- **WikiEditor**: Enhanced editing interface

## User Permissions

- **Anonymous users**: Cannot create accounts or edit pages
- **Registered users**: Can edit pages and use translation features
- **System operators**: Have additional administrative privileges

## Maintenance

### Log Files

- Debug logs: `/var/log/mediawiki/debug-{database_name}.log`
- Database error logs: `/var/log/mediawiki/dberror.log`

### Regular Maintenance

1. **Database maintenance**
   ```bash
   php maintenance/update.php
   ```

2. **Cache clearing**
   ```bash
   php maintenance/rebuildrecentchanges.php
   php maintenance/rebuildtextindex.php
   ```

3. **Backup**
   - Database: `mysqldump -u username -p database_name > backup.sql`
   - Files: Backup the `/var/lib/mediawiki` directory

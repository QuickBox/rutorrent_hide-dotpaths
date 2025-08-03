# 🔧 Hide Dot-Prefixed Files in ruTorrent (Complete Guide)

This guide provides **two methods** to hide dot-prefixed files and directories in ruTorrent. The **plugin method is recommended** for comprehensive protection, while the **patch method** is for filemanager-only users.

~~---------------------------------------------------~~

## 🏆 **RECOMMENDED: hide-dotpaths Plugin (Complete Solution)**

The **hide-dotpaths plugin** provides comprehensive protection by hiding dot-prefixed files and directories from **ALL** ruTorrent interfaces, not just the filemanager.

### 🎯 **What the Plugin Does**
- 🔒 **Hides ALL dot-prefixed files/directories** from dropdowns, browsers, and menus
- 🛡️ **Dual Protection**: Client-side filtering + server-side filemanager patch
- 🌍 **Multi-language support** (7 languages)
- ⚡ **Real-time filtering** with advanced debug system
- 📊 **Configurable options** for different environments

### 🚀 **QuickBox Installation (Recommended)**

```bash
# Clone to QuickBox ruTorrent plugins directory
git clone "https://github.com/QuickBox/rutorrent_hide-dotpaths.git" "/srv/rutorrent/plugins/hide-dotpaths"

# Set permissions
chown www-data:www-data -R "/srv/rutorrent/plugins/hide-dotpaths"
chmod 755 "/srv/rutorrent/plugins/hide-dotpaths/"
chmod 644 "/srv/rutorrent/plugins/hide-dotpaths/"*.js
chmod 644 "/srv/rutorrent/plugins/hide-dotpaths/"*.php
chmod 644 "/srv/rutorrent/plugins/hide-dotpaths/"*.info

# Restart services
sudo systemctl restart php8.1-fpm nginx
source /opt/quickbox/config/system/mflibs/quickbox >/dev/null 2>&1
read -d '' -ra users_all <<<"$(quickbox::database "SELECT username FROM user_information")"
for user in "${users_all[@]:-}"; do
  sudo systemctl restart rtorrent@"${user}"
done
```

### ⚙️ **Configuration**

Edit `conf.php` to customize behavior:

```php
<?php
// Main settings
$hide_dotpaths = true;           // Enable/disable plugin
$filter_selects = true;          // Filter <select> dropdowns
$filter_modals = true;           // Filter modal file browsers
$filter_autocomplete = true;     // Filter autocomplete dropdowns
$filter_ajax = true;             // Filter AJAX responses
$filter_filemanager = true;      // Filter filemanager plugin table rows
$preserve_parent_dirs = true;    // Keep ".." parent directory references
$debug_mode = false;             // Enable debug logging
$apply_filemanager_patch = true; // Apply server-side filemanager patch
?>
```

### ✅ **Plugin Benefits**
- **Complete Coverage**: Hides dot-prefixed files from ALL ruTorrent interfaces
- **Advanced Debug System**: Intelligent logging with deduplication
- **Configurable**: Flexible settings for different environments
- **Multi-language**: Support for 7 languages
- **Real-time**: Instant filtering with no visible blips

~~---------------------------------------------------~~

## 🔧 **ALTERNATIVE: Filemanager Patch (Filemanager Only)**

**⚠️ Limited Scope**: This patch **only affects the filemanager plugin**. For comprehensive protection, use the plugin above.

This method **disables** the "Show Hidden Files" checkbox in ruTorrent's File Manager so hidden files are **never shown**, even if users try to toggle it.

### 📦 **Step 1: Download the Patch File**

```bash
wget -O /srv/rutorrent/plugins/filemanager/filemanager-force-hide-hidden.patch \
  "https://raw.githubusercontent.com/QuickBox/pro-v3/refs/heads/main/resources/patches/rutorrent/filemanager-force-hide-hidden.patch"
```

### 🛠️ **Step 2: Apply the Patch**

```bash
cd /srv/rutorrent/plugins/filemanager
patch -p1 < filemanager-force-hide-hidden.patch
```

✅ You should see lines prefixed with `patching file ...`  
✅ This means the patch applied successfully.

### 🔄 **Step 3: Refresh the Interface**

```bash
systemctl restart php8.1-fpm && systemctl restart nginx
```

### ⚠️ **Patch Limitations**
- **Filemanager Only**: Only affects the filemanager plugin interface
- **No Dropdown Protection**: Doesn't hide dot-prefixed files from path dropdowns
- **No Menu Protection**: Doesn't hide dot-prefixed files from browse menus
- **Limited Scope**: Only prevents the "Show Hidden Files" checkbox from working

~~---------------------------------------------------~~

## 🎯 **Recommendation**

**For QuickBox users**: Use the **hide-dotpaths plugin** (first method) for complete protection.

**For filemanager-only users**: Use the **patch method** (second method) if you only need filemanager protection.

**For maximum security**: Use **both methods** - the plugin for comprehensive protection and the patch as a backup.

~~---------------------------------------------------~~

## 🧠 **Notes**

- These methods do **not delete hidden files**, only hide them visually
- Power users can still manage hidden files via SSH or SFTP if needed
- The plugin provides **real-time filtering** with advanced debug capabilities
- Both methods are **production-ready** and **well-tested**

~~---------------------------------------------------~~

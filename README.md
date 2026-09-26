# magento-ce-security-patches-247
Adapted composer package of security update patches for Magento Community Edition / Adobe Commerce Open Source 2.4.7

[![Build Status](https://github.com/medigeek/magento-ce-security-patches-247/actions/workflows/php.yml/badge.svg)](https://github.com/medigeek/magento-ce-security-patches-247/actions/workflows/php.yml)

# Help and business engagements

For any custom requirements you can contact me at savvas@radevic.com for business engagements.

# Adobe Commerce Open Source 2.4.7 Security Patches (`medigeek/magento-ce-security-patches-247`)

Automated security patch meta-package providing critical vulnerabilities hotfixes for **Adobe Commerce Open Source / Magento Community Edition 2.4.7** releases.

This repository extracts, formats to favour composer patching, and maintains compatibility-tested security patches derived directly from official Adobe Commerce Security Bulletins ([APSB Releases](https://helpx.adobe.com/security/products/magento.html?utm_source=gemini)), allowing Magento Open Source or Adobe Commerce Community Edition installations to stay secured without waiting for major core upgrade cycles.

# Adaptations of patches

Patches were adapted to match the logic of [cweagans/composer-patches](https://github.com/cweagans/composer-patches). They were split per module package, using for example:

```
mkdir -p patches/split && awk '/^diff --git/ { match($0, /a\/(vendor\/[^\/]+\/[^\/]+)/, m); if(m[1]=="") match($0, /a\/([^\/]+\/[^\/]+)/, m); pkg=m[1]; gsub(/\//, "-", pkg); out="patches/split/" pkg ".patch" } { print > out }' patches/VULN_39341_composer_patches/VULN-39341_247-p10.patch
```

# Adobe Commerce Monthly isolated security patching policy

Topics: Compliance Security Integrations Developer tools Storefront Configuration

To help Adobe Commerce customers apply critical security fixes sooner, Adobe Commerce now delivers monthly isolated security patches on Patch Tuesday (the second Tuesday of the month). See the Adobe Commerce release schedule for dates. These patches are available for Adobe Commerce on Cloud, Adobe Commerce on-premises, and Magento Open Source installations.

An isolated security patch file contains only the code needed to resolve one or more specific security vulnerabilities, delivered as a narrowly scoped code-diff file rather than a full Composer package. Because the changes are specific to security vulnerabilities, they can be reviewed, tested, and applied faster than a security patch release, without triggering the broader dependency resolution and regression testing that a security patch version upgrade requires.

Every monthly isolated security patch file is folded into the next full security patch release, so customers can get all the released isolated patch files through the next security patch (-pN) release.

See more at: https://experienceleague.adobe.com/en/docs/commerce-operations/release/planning/monthly-isolated-security-patches

---

## 🔒 Included Security Bulletins

This package accumulates security fixes sourced from official Adobe Security Bulletins starting from **APSB24-73** onward:

| Security Bulletin | Release Date | Target Component / Fix Focus | Sourced Status |
| --- | --- | --- | --- |
| [APSB26-138](https://helpx.adobe.com/security/products/magento/apsb26-138.html?utm_source=gemini) | Sept 08, 2026 | Critical Remote Code Execution & Privilege Escalation | ✅ Included |
| [APSB26-146](https://helpx.adobe.com/security/products/magento/apsb26-146.html?utm_source=gemini) | Sept 07, 2026 | Security Update / Session Validation Fixes | ✅ Included |
| [APSB26-92](https://helpx.adobe.com/security/products/magento/apsb26-92.html?utm_source=gemini) | Aug 11, 2026 | Improper Access Control & Input Sanitization | ✅ Included |
| [APSB24-73](https://helpx.adobe.com/security/products/magento/apsb26-73.html) | Jul 14, 2026 | Critical, important and moderate vulnerabilities | ✅ Included |

---

## ⚙️ Requirements & Dependencies

This patch package relies on [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches?utm_source=gemini) (v1.x / v2.x compatible) to apply differential file changes during `composer install` / `composer update`.

* **PHP:** `>= 8.1` / `8.2` / `8.3`
* **Magento Version:** `2.4.7-p10`)
* **Composer Plugins Allowed:** `cweagans/composer-patches`

---

## 🚀 Installation & Setup

### 1. Add Repository to `composer.json`

Add this repository to your project's `composer.json` file or configure it via CLI:

```bash
composer config repositories.medigeek-patches vcs https://github.com/medigeek/magento-ce-security-patches-247.git

```

### 2. Configure Composer Patch Settings & Plugin Permissions

Ensure `cweagans/composer-patches` is allowed to execute and configured for Magento core directory structures by setting the required `extra` properties in your project's `composer.json`:

```bash
# Trust the composer-patches plugin
composer config 'allow-plugins.cweagans/composer-patches' true

# Set patch resolution depth for vendor/magento path structures
composer config extra.composer-exit-on-patch-failure true
composer config extra.composer-patches.default-patch-depth 4

```

### 3. Require the Package

Require the package in your Magento project:

```bash
composer require medigeek/magento-ce-security-patches-247:dev-main

```

Upon execution, Composer will download the patch manifests and automatically apply all security hotfixes across your `vendor/magento/` core modules.

---

## 🛠 Troubleshooting & Edge Cases

### Line Endings (`LF` vs `CRLF`)

If running builds across mixed environments (Windows/Linux CI), patches can fail to apply if line endings are converted to `CRLF`. This repository enforces `LF` endings natively via `.gitattributes`. If you maintain custom overrides, ensure your patch execution engine uses `LF`.

### Verifying Applied Patches

To verify that patches were successfully applied during deployment or CI runs:

```bash
# Check if cweagans generated the patch lockfile
cat patches.lock.json

# Check Git diff inside vendor core modules (if applicable)
git -C vendor/magento/module-quote status

```

---

## 🤝 Contributing

1. Fork the repository.
2. Ensure patch files are strictly UNIX (`LF`) line ending formatted.
3. Validate patch execution depth matches `patch -p4` standard vendor paths.
4. Submit a Pull Request targeting `main`.

---

## 📄 License

Distributed under the [Open Software License v3 (OSL-3.0)](https://opensource.org/license/OSL-3.0). 

Sourced patch logic derived from public upstream security patches issued by Adobe Systems Incorporated.

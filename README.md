# Vyaparify Pro - Business Management & Billing System

A modern, responsive offline-first Point of Sale (POS), Inventory, Khata (Ledger), and Billing management application with WhatsApp integration, Google Drive Multi-Device Cloud Synchronization, Cryptographic Security & Lockout Protection, Emergency Recovery Key, and complete Edit & Delete controls.

---

## 🌟 Comprehensive Features

### 1. 🛡️ High-Grade Security & Master PIN Recovery
- **Master PIN Lock**:
  - Full-screen lock modal with modern PIN indicator dots and on-screen numeric keypad (also supports physical keyboards).
  - PINs are hashed using cryptographic **SHA-256** digests via the Web Crypto API (`window.crypto.subtle`). Raw PINs are never stored.
  - **Quick Lock Action**: One-tap `🔒 Lock App` button in both the sidebar and the top header.
- **Master PIN Recovery Options (If PIN is Forgotten)**:
  - **Option 1: Emergency Recovery Key (`VYAPAR-XXXX-XXXX`)**:
    - A 12-character emergency recovery key is generated when the PIN is created.
    - Click **"🔑 Forgot Master PIN?"** on the lock screen or sensitive action prompt, enter the emergency recovery key, and immediately set a new PIN.
  - **Option 2: Registered Store Owner Verification**:
    - Switch to the "Store Owner Phone" tab in the recovery modal.
    - Entering the phone number registered under Shop Details verifies owner identity and allows resetting the PIN.
  - **Option 3: Emergency Physical Device Console Reset**:
    - In an emergency on your trusted hardware device, press <kbd>F12</kbd> (DevTools ➔ Console) and run:
      ```javascript
      localStorage.removeItem('vy_security'); location.reload();
      ```
    - This clears **only** the security lock without altering or deleting any invoices, customers, ledger entries, or stock records.
- **Inactivity Auto-Lock**:
  - Automatically locks the screen if no mouse, touch, or keyboard activity is detected within a configurable duration (1 min, 5 mins, 15 mins, 30 mins, or disabled).
- **Anti-Brute Force Protection**:
  - Automatically locks out PIN attempts for 30 seconds after 5 consecutive incorrect attempts.
- **Sensitive Action Authorization Guard**:
  - Critical actions (Factory Data Reset, Deleting Invoices, Deleting Customers, and Exporting Full Database Backups) require Master PIN verification prior to execution.
- **SHA-256 Backup Tamper Detection & Checksum**:
  - Downloaded database backups generate a cryptographic SHA-256 integrity hash.
  - Restoring a backup automatically validates the checksum and alerts the merchant if the file has been altered or corrupted.
- **Credential Masking & Security Card**:
  - Google OAuth Client ID and Apps Script Webhook URLs are masked with `type="password"` with an interactive `👁️` toggle button.
  - Dedicated **Counter Security & Master PIN Lock** card in the Cloud & Security settings tab.
- **Cross-Site Scripting (XSS) Prevention**:
  - All customer names, product descriptions, notes, and invoice IDs pass through strict HTML entity escaping before rendering in tables, carts, and previews.

---

### 2. 🖨️ PDF Reports, Receipts & Statements (Zero Blank Pages)
- **Elimination of Trailing Blank Pages**:
  - Previously, `@media print` using `visibility: hidden` retained the layout height of the entire application, causing Chromium and WebKit print engines to generate 2 to 3 extra blank pages.
  - Resolved with clean print flow architecture: all application DOM trees (`.app-container`, `header`, `aside`, `main`, `.mobile-bottom-nav`, `.modal-backdrop`) are set to `display: none !important; height: 0 !important;` during print.
  - Print sections (`#printable-report`, `#printable-invoice`, `#printable-statement`) use `position: relative !important;` and `height: auto;`.
- **Complete Business Report Generation**:
  - Clicking **"🖨️ Print Report"** dynamically populates store branding, report title, active date range, KPI financial totals (Revenue, Bills, Cash/UPI, Credit), and itemized tables with automatic `onafterprint` class cleanup.

---

### 3. 🎨 Premium UI & Polished Visual Aesthetics
- **Modern Typography**:
  - Integrated Google Fonts: **Plus Jakarta Sans** for crisp body and data tables, and **Outfit** for modern geometric headings and KPI counters.
- **Glassmorphism & Refined Design Tokens**:
  - Backdrop blur effects (`10px`–`20px`), subtle border gradients, and elevation box shadows (`--card-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05)`).
- **Micro-Animations & Status Pills**:
  - Hover lift effects, smooth modal transitions, animated PIN entry dots, and color-coded stock badges:
    - 🟢 **In Stock** (`Stock: X`)
    - 🟡 **Low Stock** (`Low: X`)
    - 🔴 **Out of Stock**
- **Animated Toast Notifications**:
  - Fixed-position floating notifications for user feedback on locks, saves, syncs, and deletions.

---

### 4. 📱 Multi-Device & Screen Resolution Compatibility
- **Desktop & Laptop (> 1100px)**:
  - Full two-column POS billing interface with searchable product grid on the left and live editable cart panel on the right.
  - Persistent sidebar navigation with counter details and system shortcuts.
- **Tablet (768px – 1100px)**:
  - Adaptive grids that automatically adjust KPI stat cards to 2 columns and collapse dense table columns smoothly.
- **Mobile Phones (< 768px)**:
  - **Sticky Mobile Bottom Navigation**: 5 thumb-friendly touch tabs (`📊 Dashboard`, `⚡ POS`, `📑 Bills`, `📒 Khata`, `☰ More`) with haptic feedback styling.
  - **Floating Mobile Cart Checkout Pill**: Sticky bottom bar displaying item count, total quantity, and grand total price with an automatic smooth-scroll button to the checkout drawer.
  - **Touch Target Optimization**: All buttons and interactive controls meet or exceed accessibility standards ($\ge 44\text{px}$).
  - **iOS Safari Zoom Fix**: Input elements maintain a base font size of `16px` to prevent disruptive auto-zooming on focus.
  - Responsive tables with horizontal scroll wrappers and stacked card-friendly layouts.

---

### 5. 📱 WhatsApp Integration & Invoice Details in Khata
- **Invoice Sharing**:
  - Send professional, formatted bills directly to customer's WhatsApp via `wa.me` links.
  - Available on the **POS Invoice Preview Modal** immediately after completing a sale and on any past bill in the **Invoices** tab.
  - Formatted message includes: Store Name, Contact, Bill ID, Date/Time, Customer Details, Itemized list with quantities and prices, Advance adjusted, and Grand Total.
- **Itemized Invoice Descriptions in Khata**:
  - Every credit sale automatically captures full bill itemization: Product Name, Quantity, and Unit Price (e.g. `Bill #241026-1 (Basmati Rice 5kg x 1.0, Sunflower Oil 1L x 2.0)`).
  - **Interactive Bill Link in Statements**: Each transaction row in the customer statement displays an invoice preview card with itemized breakdown pills and a **"View Bill ➔"** shortcut to open the full invoice receipt.
  - **Statement Print & PDF**: Printed statements itemize each credit purchase with bill ID, product names, quantities, and rates.
  - **Edit Khata Linking**: Editing any Khata transaction automatically detects linked invoices, displaying bill items and providing a quick link to inspect the original bill.
- **Khata Book / Credit Statements & Reminders**:
  - Share complete account statements and friendly payment reminders directly on WhatsApp.
  - Available on each customer row in the **Khata Book** tab, **Customers** tab, and within the **Customer Statement Modal**.
  - Message includes: Outstanding due balance, advance credit status, last 5 recent transactions with **itemized invoice descriptions** (e.g., `• 24/10/2026: You Gave (+) ₹730.00 ↳ Bill #241026-1: Basmati Rice 5kg x1.0, Sunflower Oil 1L x2.0`), and store UPI/payment contact.
- **Smart Phone Number Handler**:
  - If a customer has a saved phone number, WhatsApp opens directly with one click.
  - If phone number is missing (e.g. walk-in customer), a dedicated **WhatsApp Prompt Modal** allows entering the number (and automatically saves it for future use) or sharing directly to WhatsApp contacts.

---

### 6. ☁️ Google Drive Multi-Device Cloud Sync & Auto-Backup
- **Same Google Account on Multiple Devices**:
  - Sign in with the same Google Account on multiple computers, laptops, tablets, or mobile phones.
  - Data seamlessly synchronizes across all devices through your personal Google Drive (`vyaparify_cloud_database.json`).
- **Real-Time Live Status Pill**:
  - The sticky header displays a live sync indicator (`🟢 Drive: Synced (Counter 1)` or `☁️ Drive: Offline`).
- **Two Connection Methods Supported**:
  - **Method A: Google Sign-In (OAuth 2.0)**: Official Google Identity Services (GIS) + Google Drive API v3.
  - **Method B: Google Apps Script Webhook (Zero OAuth Setup!)**: Instant setup via Google Apps Script web app endpoint. Includes validation on connect to verify deployment access permissions.
- **Accurate Script Deletion & Offline Error Detection**:
  - If a Google Apps Script is deleted or un-deployed in Google Drive, the sync status immediately alerts with `⚠️ Script Offline / Deleted` and `Drive: Script Error` rather than falsely displaying "Synced".
  - Dedicated **Disconnect** buttons in the account status bar and Webhook panel allow 1-click clean detachment.
- **Device Attribution on All Entries & Reports**:
  - Each hardware terminal, laptop, or mobile receives a unique device ID and customizable name (e.g., `Counter 1 (Billing PC)`, `Counter 2 (Laptop)`, `Mobile Shop`).
  - **Invoices**: Every generated bill records and displays the terminal name (`💻 Counter 1`) in the invoice table, bill preview modal, thermal receipts, and printouts.
  - **Khata Ledger**: Every transaction tracks the counter that recorded it, displayed with a clean badge in customer statements and printed statements.
  - **Reports & Analytics**: Sales and Khata reports include a dedicated **Device** column in the on-screen table, printable PDF/report view, and CSV export.
- **Distributed Deletion Tombstone Tracking (No Zombie Resurrections)**:
  - Deleting an invoice, khata transaction, inventory product, or customer profile records a deletion tombstone in the local and cloud registry (`deletedIds`).
  - When another terminal pulls cloud updates, it synchronizes the tombstone registry and purges deleted records locally, permanently preventing deleted items from re-appearing on subsequent multi-device syncs.
- **Mobile View & Responsive Optimization**:
  - Dynamic button labeling (`+ Quick Bill (F2)` collapses to `+ Bill` on mobile).
  - Compact header with auto-truncating cloud badge prevents collisions on screens ≤ 768px and ≤ 480px.
  - Improved stat label line wrapping, bottom navigation clearance for sheet modals, and nowrap table action cells prevent text overlapping.
- **Intelligent Multi-Device Conflict Resolution**:
  - Automatically merges invoices and ledger transactions from different counters by unique IDs.
  - Products and customer details merge with latest timestamp priority.
  - Background auto-pull runs every 45 seconds and on window focus with loop prevention guards.

---

### 7. 💾 Manual Backup & Auto-Backup System
- **Manual Backups**:
  - **Download JSON Backup File**: Generates a complete timestamped backup with SHA-256 checksum containing shop profile, products, customers, invoices, ledger, and settings.
  - **Restore / Upload JSON Backup**: Pick any `.json` backup file to restore with automatic safety pre-restore snapshot and SHA-256 verification.
- **Automated Backups**:
  - Morning and Evening local slot auto-snapshots.
  - Automated cloud push on every transaction or data update.
  - Interactive **Snapshots & Auto-Backups List** with 1-click **Restore** and **Download** on any previous snapshot.

---

### 8. ✏️ Complete Edit & 🗑️ Delete Controls (Invoices, Khata & Customers)
- **Invoices**:
  - **Edit Bill**: Click "Edit" on any invoice in the **Invoices** tab or from the bill view modal. You can add or remove items, increase/decrease quantities, change item rates, switch payment modes (Cash, Online, Khata/Credit), change bill dates, and reassign customers. Inventory stock deltas and credit balances are automatically recalculated.
  - **Delete Bill**: Delete invoices with one click from the Invoices table, Bill Preview modal, or Edit Invoice modal. Deleting automatically restores product stock quantities back into inventory and reverses associated Khata ledger debit entries.
- **Khata Book (Credit Ledger)**:
  - **Edit Transaction**: Inside any customer's statement, click the `✏️` button on any transaction entry to edit its type (Debit Due vs Payment Received), amount, date/time, and description notes.
  - **Delete Transaction**: Click the `🗑️` button on any transaction row or from the Edit Khata modal to permanently remove an entry with confirmation, automatically recalculating the customer's pending balance.
- **Customers**:
  - **Edit Profile**: Modify customer name, phone number, email, address, and GST/PAN directly from the Customers tab.
  - **Delete Customer**: Delete a customer profile directly from the Customers table or from inside the Customer Edit modal (`🗑️ Delete Customer`). Includes safety validation that warns if the customer has an outstanding balance before proceeding.

---

## 🚀 Getting Started

Simply open [index.html](file:///d:/ANTIGRAVITY/Billing-Website-main/index.html) in any modern browser (Chrome, Edge, Safari, Firefox). No build tools, runtime servers, or database installations are required. Everything runs locally in the browser with offline-first persistence.

# Sentinel Guard - Mobile App Design Document

## App Overview
Sentinel Guard is a counter-intelligence mobile app designed to detect fake cell towers (IMSI catchers/Stingrays) and monitor Bluetooth devices to identify potential surveillance or stalking. The app provides real-time threat detection, alerts, and comprehensive logging for security-conscious users.

## Design Philosophy
- **iOS-first design** following Apple Human Interface Guidelines (HIG)
- **Dark theme by default** (appropriate for security/surveillance app)
- **One-handed usage** optimized for portrait orientation (9:16)
- **Clear visual hierarchy** with threat levels using color coding

---

## Screen List

### 1. Dashboard (Home)
Primary screen showing current security status at a glance.

### 2. Cell Tower Monitor
Detailed view of connected cell tower information and threat analysis.

### 3. Bluetooth Scanner
List of detected Bluetooth devices with tracking pattern detection.

### 4. Alerts History
Chronological log of all security alerts and suspicious events.

### 5. Settings
App configuration, notification preferences, and about section.

---

## Primary Content and Functionality

### Dashboard (Home)
- **Large threat status indicator** (Safe/Warning/Danger) with animated ring
- **Quick stats cards**: Active threats, Devices tracked, Last scan time
- **Current cell tower summary**: Network type, Signal strength, Tower ID
- **Recent alerts preview** (last 3 alerts)
- **Quick action button**: Start/Stop continuous monitoring

### Cell Tower Monitor
- **Current tower details card**: MCC, MNC, LAC, Cell ID, Network type
- **Signal strength meter** with history graph
- **Threat indicators list**:
  - 2G downgrade detection
  - Unusual tower ID changes
  - IMSI request anomalies
  - Signal strength anomalies
- **Tower history timeline** showing recent connections
- **Map placeholder** for tower location (future feature)

### Bluetooth Scanner
- **Scan control button** (Start/Stop scanning)
- **Device list** with:
  - Device name/MAC address
  - Signal strength (RSSI)
  - First seen / Last seen timestamps
  - Sighting count
  - Tracking risk indicator (Low/Medium/High)
- **Filter chips**: All, Suspicious, Trackers, Unknown
- **Device detail modal** with full history

### Alerts History
- **Filterable alert list** by type and severity
- **Alert card** showing:
  - Alert type icon
  - Timestamp
  - Severity level (color coded)
  - Brief description
  - Expandable details
- **Export functionality** for sharing logs

### Settings
- **Monitoring preferences**: Auto-start, Scan interval, Background mode
- **Notification settings**: Alert sounds, Vibration, Priority levels
- **Privacy options**: Data retention period, Clear history
- **About section**: Version, Legal, Support links

---

## Key User Flows

### Flow 1: Check Current Security Status
1. User opens app → Dashboard displays
2. Large status ring shows current threat level
3. User sees quick stats and recent alerts
4. Tap any card for detailed view

### Flow 2: Investigate Cell Tower Threat
1. Dashboard shows warning status
2. User taps "Cell Tower" card or tab
3. Cell Tower Monitor shows detailed analysis
4. User reviews threat indicators
5. Tap specific indicator for explanation
6. Optional: Export report

### Flow 3: Detect Bluetooth Stalker
1. User taps Bluetooth tab
2. Starts scan (or auto-scanning enabled)
3. Device list populates with nearby devices
4. Suspicious device highlighted (seen multiple times/locations)
5. User taps device for details
6. Views sighting history and pattern
7. Can mark as "Known" or "Suspicious"

### Flow 4: Review Alert History
1. User receives notification about threat
2. Opens app → navigates to Alerts tab
3. Filters by alert type
4. Taps alert for full details
5. Can export or share alert log

---

## Color Choices

### Primary Palette (Dark Theme)
- **Background**: #0D0D0F (near black)
- **Surface/Card**: #1A1A1E (dark gray)
- **Elevated Surface**: #252529 (lighter gray)

### Accent Colors
- **Primary Accent**: #00D4AA (cyber teal) - main actions, safe status
- **Secondary Accent**: #6366F1 (indigo) - Bluetooth, secondary actions

### Status Colors
- **Safe/Secure**: #00D4AA (teal green)
- **Warning/Caution**: #FFB800 (amber)
- **Danger/Threat**: #FF3B5C (red-pink)
- **Info/Neutral**: #3B82F6 (blue)

### Text Colors
- **Primary Text**: #FFFFFF (white)
- **Secondary Text**: #9CA3AF (gray-400)
- **Disabled Text**: #4B5563 (gray-600)

---

## Typography Scale

| Style | Size | Weight | Use Case |
|-------|------|--------|----------|
| Large Title | 34pt | Bold | Screen titles |
| Title 1 | 28pt | Bold | Section headers |
| Title 2 | 22pt | Semibold | Card titles |
| Headline | 17pt | Semibold | List headers |
| Body | 17pt | Regular | Main content |
| Callout | 16pt | Regular | Secondary content |
| Subhead | 15pt | Regular | Captions |
| Footnote | 13pt | Regular | Timestamps, metadata |
| Caption | 12pt | Regular | Labels |

---

## Component Specifications

### Threat Status Ring
- Size: 200x200pt centered
- Animated gradient ring based on status
- Center shows status text and icon
- Subtle pulse animation when active

### Stat Cards
- Height: 80pt
- Corner radius: 16pt
- Horizontal padding: 16pt
- Icon left, value right
- Subtle gradient background

### Device List Item
- Height: 72pt
- Left: Device icon (24pt)
- Center: Name + MAC address stacked
- Right: RSSI indicator + risk badge
- Swipe actions: Mark known, Ignore

### Alert Card
- Corner radius: 12pt
- Left border color indicates severity
- Expandable with chevron
- Timestamp in footnote style

---

## Tab Bar Configuration

| Tab | Icon (SF Symbol) | Material Icon |
|-----|------------------|---------------|
| Dashboard | shield.fill | security |
| Cell Tower | antenna.radiowaves.left.and.right | cell-tower |
| Bluetooth | dot.radiowaves.left.and.right | bluetooth-searching |
| Alerts | bell.badge.fill | notifications |
| Settings | gearshape.fill | settings |

---

## Safe Area Handling
- All screens use `useSafeAreaInsets()`
- Bottom tab bar respects home indicator
- Content never overlaps notch or corners
- Modal sheets have proper top padding

---

## Animations
- Status ring: Continuous subtle rotation when scanning
- Alert badge: Bounce on new alert
- List items: Fade in on load
- Tab transitions: Cross-fade
- Scan button: Pulse when active

# Sentinel Guard - Implementation Guide

## Quick Start

### 1. Install Dependencies

```bash
cd sentinel-guard
pnpm install
```

### 2. Run Development Server

```bash
pnpm dev
```

This starts both the Metro bundler and the backend server.

### 3. Run on Device/Emulator

**Android:**
```bash
pnpm android
```

**iOS:**
```bash
pnpm ios
```

**Web:**
```bash
pnpm dev:metro
```

---

## Integration Steps

### Step 1: Update Security Context

Add imports to `contexts/security-context.tsx`:

```typescript
import { UnifiedSecurityAssessment } from '@/lib/unified-security';
import { CellularSignal } from '@/lib/imsi-detector';
import { TrackerProfile } from '@/lib/tracker-detector';
```

### Step 2: Integrate Unified Assessment

In the `startScan` function:

```typescript
const unifiedAssessment = new UnifiedSecurityAssessment();

// Update with cellular signals
const cellularSignal: CellularSignal = {
  mcc: currentTower.mcc,
  mnc: currentTower.mnc,
  lac: currentTower.lac,
  cellId: currentTower.cellId,
  rssi: currentTower.signalStrength,
  timingAdvance: Math.floor(Math.random() * 64),
  networkType: currentTower.networkType as any,
  timestamp: Date.now(),
};

unifiedAssessment.updateCellularSignal(cellularSignal);

// Perform assessment
const status = await unifiedAssessment.assessSecurity();
```

### Step 3: Create Network Security Screen

Create `app/(tabs)/network-security.tsx`:

```typescript
import { View, Text, ScrollView } from 'react-native';
import { useSecurityContext } from '@/contexts/security-context';
import { simulateNetworkScan, assessNetworkSecurity } from '@/lib/network-scanner';

export default function NetworkSecurityScreen() {
  const [devices, setDevices] = React.useState([]);
  const [assessment, setAssessment] = React.useState(null);

  const scanNetwork = async () => {
    const devices = simulateNetworkScan('192.168.1.0/24');
    const assessment = assessNetworkSecurity(devices);
    setDevices(devices);
    setAssessment(assessment);
  };

  return (
    <ScrollView className="flex-1 bg-[#0D0D0F] p-4">
      <Text className="text-white text-2xl font-bold mb-4">Network Security</Text>
      {/* Display devices and threats */}
    </ScrollView>
  );
}
```

### Step 4: Create Privacy Dashboard Screen

Create `app/(tabs)/privacy.tsx`:

```typescript
import { View, Text, ScrollView } from 'react-native';
import { RealTimeAnonymizer } from '@/lib/facial-anonymizer';

export default function PrivacyScreen() {
  const [anonymizer] = React.useState(() => 
    new RealTimeAnonymizer({ method: 'blur', blurIntensity: 50 })
  );

  const stats = anonymizer.getStats();

  return (
    <ScrollView className="flex-1 bg-[#0D0D0F] p-4">
      <Text className="text-white text-2xl font-bold mb-4">Privacy Protection</Text>
      <Text className="text-white">Faces Anonymized: {stats.totalFacesAnonymized}</Text>
      <Text className="text-white">Processing Time: {stats.averageProcessingTimeMs.toFixed(2)}ms</Text>
    </ScrollView>
  );
}
```

### Step 5: Update Tab Navigation

In `app/(tabs)/_layout.tsx`, add new tabs:

```typescript
<Tabs.Screen
  name="network-security"
  options={{
    title: 'Network',
    tabBarIcon: ({ color }) => <MaterialIcons name="router" size={24} color={color} />,
  }}
/>

<Tabs.Screen
  name="privacy"
  options={{
    title: 'Privacy',
    tabBarIcon: ({ color }) => <MaterialIcons name="privacy-tip" size={24} color={color} />,
  }}
/>
```

---

## Using the Modules

### Network Scanner

```typescript
import { simulateNetworkScan, detectMitmAttacks, assessNetworkSecurity } from '@/lib/network-scanner';

// Scan network
const devices = simulateNetworkScan('192.168.1.0/24', (progress) => {
  console.log(`Scan progress: ${progress}%`);
});

// Detect threats
const threats = detectMitmAttacks(devices);

// Get assessment
const assessment = assessNetworkSecurity(devices);
console.log(`Risk Level: ${assessment.overallRisk}`);
assessment.recommendations.forEach(rec => console.log(`- ${rec}`));
```

### IMSI Catcher Detector

```typescript
import { detectImsiCatcher, calculateImsiThreatLevel, generateImsiRecommendations } from '@/lib/imsi-detector';

// Create signal
const signal = {
  mcc: '310',
  mnc: '410',
  lac: '1234',
  cellId: '5678',
  rssi: -80,
  timingAdvance: 32,
  networkType: '4G',
  timestamp: Date.now(),
};

// Detect threats
const threats = detectImsiCatcher(signal, [previousSignal]);

// Get threat level
const level = calculateImsiThreatLevel(threats);

// Get recommendations
const recommendations = generateImsiRecommendations(threats);
```

### Facial Anonymizer

```typescript
import { anonymizeImage, RealTimeAnonymizer } from '@/lib/facial-anonymizer';

// Single image
const result = await anonymizeImage(imageData, {
  method: 'blur',
  blurIntensity: 50,
  expandBoundingBox: 10,
});

// Real-time video
const anonymizer = new RealTimeAnonymizer({
  method: 'pixelate',
  pixelSize: 15,
});

const frameResult = await anonymizer.processFrame(frameData);
const stats = anonymizer.getStats();
```

### Tracker Detector

```typescript
import { assessTrackerThreat, identifyTrackerType } from '@/lib/tracker-detector';

// Identify tracker
const type = identifyTrackerType(bleAdvertisement);

// Create tracker profile
const tracker = {
  id: 'device_1',
  name: 'Unknown Device',
  type,
  macAddress: '00:11:22:33:44:55',
  firstSeen: Date.now(),
  lastSeen: Date.now(),
  sightingCount: 5,
  locations: [userLocation],
  riskScore: 0,
  isKnown: false,
  isIgnored: false,
};

// Assess threat
const assessment = assessTrackerThreat(tracker, userMovement, userLocations);
console.log(`Risk Score: ${assessment.riskScore}`);
console.log(`Severity: ${assessment.overallSeverity}`);
```

### Unified Security Assessment

```typescript
import { UnifiedSecurityAssessment } from '@/lib/unified-security';

const assessment = new UnifiedSecurityAssessment();

// Update data
assessment.updateCellularSignal(cellularSignal);
assessment.updateTracker(tracker);
assessment.updateUserLocation(location);
assessment.updateMovementPattern(pattern);

// Perform assessment
const status = await assessment.assessSecurity();
console.log(`Overall Threat: ${status.overallThreatLevel}`);
console.log(`Total Threats: ${status.threatSummary.totalThreats}`);
```

---

## Testing

### Unit Tests

Create test files in `tests/` directory:

```typescript
// tests/network-scanner.test.ts
import { describe, it, expect } from 'vitest';
import { simulateNetworkScan, detectMitmAttacks } from '@/lib/network-scanner';

describe('Network Scanner', () => {
  it('should detect MITM attacks', () => {
    const devices = simulateNetworkScan('192.168.1.0/24');
    const threats = detectMitmAttacks(devices);
    expect(threats).toBeDefined();
  });
});
```

Run tests:
```bash
pnpm test
```

---

## Deployment

### Build for Production

```bash
pnpm build
```

### Deploy Backend

```bash
pnpm start
```

### Build APK (Android)

```bash
eas build --platform android
```

### Build IPA (iOS)

```bash
eas build --platform ios
```

---

## Configuration

### Environment Variables

Create `.env.local`:

```
EXPO_PUBLIC_API_URL=http://localhost:3000
EXPO_PUBLIC_ENABLE_NETWORK_SCANNER=true
EXPO_PUBLIC_ENABLE_IMSI_DETECTOR=true
EXPO_PUBLIC_ENABLE_FACIAL_ANONYMIZER=true
EXPO_PUBLIC_ENABLE_TRACKER_DETECTOR=true
```

### App Configuration

Edit `app.config.ts` to customize:
- App name and icon
- Splash screen
- Permissions
- Deep linking

---

## Troubleshooting

### Module Not Found

```bash
# Clear cache and reinstall
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

### Build Errors

```bash
# Check TypeScript
pnpm check

# Lint code
pnpm lint
```

### Runtime Errors

- Check console logs: `pnpm dev`
- Check device logs: `adb logcat` (Android) or Xcode (iOS)
- Review error messages in app

---

## Performance Optimization

### Network Scanner
- Limit scan range to /24 or /25 subnets
- Use progress callbacks for long scans
- Cache results for 5 minutes

### IMSI Detector
- Keep signal history limited to 100 signals
- Process signals in background
- Use confidence thresholds to filter false positives

### Facial Anonymizer
- Detect every 2nd or 3rd frame for performance
- Use lower resolution for detection
- Cache face detections for tracking

### Tracker Detector
- Limit location history to 1000 entries
- Batch process BLE advertisements
- Use movement pattern caching

---

## Security Best Practices

1. **Permissions**: Request only necessary permissions
2. **Data Storage**: Encrypt sensitive data at rest
3. **Network**: Use HTTPS for all API calls
4. **Logging**: Don't log sensitive information
5. **Updates**: Keep dependencies up to date

---

## Additional Resources

- [React Native Documentation](https://reactnative.dev/)
- [Expo Documentation](https://docs.expo.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [TailwindCSS Documentation](https://tailwindcss.com/docs)

---

**Last Updated**: January 2026
**Version**: 2.0

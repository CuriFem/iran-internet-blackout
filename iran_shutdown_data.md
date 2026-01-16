# Iran Internet Shutdowns: Actual BGP Data

## Sources
- [OONI: Iran's nation-wide Internet blackout](https://ooni.org/post/2019-iran-internet-blackout/)
- [IODA: Measurement data and technical observations](https://ioda.inetintel.cc.gatech.edu/reports/irans-nation-wide-internet-blackout-measurement-data-and-technical-observations/)
- [Kentik: Iran Goes Dark](https://www.kentik.com/analysis/iran-goes-dark-as-government-cuts-itself-off-from-internet/)
- [OONI: 2022 Iran Technical Report](https://ooni.org/post/2022-iran-technical-multistakeholder-report/)

---

## November 2019 Shutdown

### Timeline (UTC)

| Date/Time (UTC) | Event |
|-----------------|-------|
| Nov 15, 2019 | Protests begin over 300% fuel price increase |
| Nov 16, 14:00 | First BGP drop - 33% reduction, ~15,000 /24 prefixes withdrawn |
| Nov 16, 14:00-17:00 | Cellular operators disconnect (MCI, Irancell, Rightel) |
| Nov 16, 17:00-19:00 | Fixed-line operators progressively disconnect |
| Nov 16, 17:00 | Shatel (AS31549) drops sharply |
| Nov 16, 18:00 | ParsOnline (AS16322) drops |
| Nov 17, 19:00 | Partial return in 8 provinces |
| Nov 20 | National connectivity at 5% of normal |
| Nov 21, 09:00-10:00 | Recovery begins |
| Nov 23, 10:50 | Significant restoration |

### BGP Metrics

- **Initial BGP signal drop**: 33%
- **Prefixes withdrawn**: ~15,000 globally visible /24s
- **Duration**: 5+ continuous days
- **Lowest point**: 5% of normal connectivity (Nov 20)

### Per-AS Observations

| ASN | Name | Behavior |
|-----|------|----------|
| AS12880 | ITC/DCI | No BGP drop on Nov 16, but Active Probing/IBR to near 0 at 18:00 UTC |
| AS58224 | Iran Telecom | Three separate BGP drops: 16:30, 18:30, 20:00 UTC |
| AS31549 | Shatel | Sharp drop at 17:00 UTC |
| AS16322 | ParsOnline | Drop at 18:00 UTC |
| AS44244 | IranCell | Dropped with mobile operators at 14:00-17:00 UTC |

### Method
- Cellular operators disconnected first (3 hours before fixed-line)
- Each ISP implemented separately (took 5+ hours for full rollout)
- BGP route withdrawal was the primary mechanism

---

## September 2022 Shutdown

### Timeline

| Date | Event |
|------|-------|
| Sep 16-Oct 16, 2022 | Analysis period |
| Sep 19 | Kurdistan province outage (afternoon/evening) |
| Sep 20 | Encrypted DNS (DoH) blocking begins |
| Sep 21 | Daily "digital curfew" begins: 4:00 PM - midnight local |
| Sep 21 | WhatsApp, Instagram blocked |
| Sep 22 | HTTP/3 and QUIC dropped to near-zero |
| Sep 22 | App stores (Google Play, Apple) blocked |
| Sep 22 - Oct 2 | IPv6 disruption on Irancell |
| Sep 23 | Skype, Viber, LinkedIn blocked |
| Sep 26 | Browser extension repositories blocked |
| Oct 3 | Digital curfew ends |
| Oct 8, 12, 15 | Additional outage days |
| Oct 16 | Khuzestan province all-day outage |

### Metrics

- **Total mobile downtime**: ~100 hours over 13 consecutive days
- **Daily shutdown window**: 4:00 PM - midnight (~8 hours/day)
- **Pattern**: "Stealth blackout" - more targeted than 2019

### Networks Affected (2022)

| ASN | Name | Impact |
|-----|------|--------|
| AS44244 | Irancell | Daily shutdowns, IPv6 disruption |
| AS57218 | RighTel | Daily shutdowns |
| AS197207 | MCCI/MCI | Daily shutdowns |

### Evolution from 2019 to 2022

| Aspect | 2019 | 2022 |
|--------|------|------|
| Method | BGP route withdrawal | Protocol-level + time-based |
| Duration | 5+ days continuous | 13 days, 8 hrs/day |
| Visibility | Highly visible | "Stealth" - harder to detect |
| Scope | Near-total | Mobile-focused, service-specific |
| Detection | Easy via BGP | Required multi-source analysis |

---

## January 2026 Shutdown

### Timeline (UTC)

| Date/Time (UTC) | Event |
|-----------------|-------|
| Dec 31, 2025 | HTTP/3 traffic on IranCell drops from 40% to 5% |
| Jan 3, 2026 | DNS-over-TLS (DoT) blocked |
| Jan 5-7, 2026 | Traffic spikes as users download circumvention tools |
| Jan 8, 11:50 | IPv6 routes withdrawn (98.5% drop) |
| Jan 8, 18:45 | Total blackout - traffic drops to effectively zero |
| Jan 9, 11:30 | Universities briefly reconnected, then cut again |
| Jan 10+ | Blackout continues at <0.01% of normal |

### Metrics (Verified via RIPE RIS, Jan 16 2026)

| Date | IPv4 Prefixes | IPv6 Prefixes |
|------|---------------|---------------|
| Jan 7 | 8,209 | 433 |
| Jan 8 | 8,181 | 43 |
| Jan 9 | 7,880 | 33 |
| Jan 10 | 7,904 | 29 |
| Jan 16 | 7,857 | 26 |

- **IPv6 withdrawal**: 94% (433 → 26 prefixes)
- **IPv4 status**: -4% (8,209 → 7,857) - Routes UP but traffic blocked (stealth outage)
- **People affected**: 87 million
- **Method**: Dual-protocol approach - IPv6 erased, IPv4 whitelisted

### Key Difference from Previous Shutdowns

This is a "stealth outage" - BGP monitoring shows IPv4 routes as UP, but traffic is blocked at the network level. Only whitelisted services pass through. Requires both BGP and traffic analysis to detect.

---

## Key ASNs in Iran's Internet Architecture

| ASN | Name | Role | Prefixes (current) |
|-----|------|------|-------------------|
| AS49666 | TIC-GW | International Gateway | 29 |
| AS58224 | TIC | Main backbone | 1,236 |
| AS12880 | DCI | Secondary backbone | 75 |
| AS197207 | MCCI/MCI | Mobile operator | 905 |
| AS44244 | Irancell | Mobile operator | 461 |
| AS31549 | Shatel | Fixed ISP | 185 |
| AS42337 | Respina | ISP (149 downstream) | 725 |
| AS57218 | Rightel | Mobile operator | 90 |

### International Upstreams (via TIC-GW)

| ASN | Name | Power Score |
|-----|------|-------------|
| AS6762 | Telecom Italia Sparkle | 57,102 |
| AS3257 | GTT Communications | 46,388 |
| AS33891 | Core-Backbone | 27,906 |
| AS5511 | Orange S.A. | 23,254 |
| AS6453 | TATA Communications | 19,049 |
| AS29049 | Delta Telecom | 138,447 |

---

## Architecture Overview

### Centralized Gateway Model

- **Chokepoint**: AS49666 (TIC-GW)
- **Two international gateways**: TIC and IPM (AS6736)
- **Control capabilities**: Total shutdown, selective blocking, time-based curfew, protocol blocking, DPI

### Tier Structure

| Tier | Name | ASNs |
|------|------|------|
| 1 | International Gateway | AS49666 |
| 2 | Backbone Providers | AS58224, AS12880, AS42337, AS48159 |
| 3 | ISPs and Mobile Operators | AS197207, AS44244, AS31549, AS25184, AS43754, AS57218 |

---

## Data Sources Used for Analysis

1. **IODA (Internet Outage Detection and Analysis)**
   - BGP route visibility from RouteViews, RIPE RIS
   - Active probing
   - Internet Background Radiation (IBR)

2. **OONI (Open Observatory of Network Interference)**
   - Network measurement tests
   - Censorship detection

3. **RIPE RIS (Routing Information Service)**
   - BGP routing data from 23 Route Collectors (RRCs)
   - 347-370 peers per prefix visibility

4. **Oracle/Dyn Internet Intelligence**
   - Traceroute completion metrics

5. **Google Transparency Report**
   - Traffic data drop correlation

6. **Cloudflare Radar**
   - Live traffic and trends data
   - Protocol-level analysis

7. **NetBlocks**
   - Real-time network monitoring
   - Outage verification

---

## Notes

- No active RIPE Atlas measurement probes in Iran as of January 2026
- The National Information Network (NIN/SHOMA) allows some domestic services during shutdowns
- Shutdown is asymmetric: civilian population blocked, state infrastructure remains online

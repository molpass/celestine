# Celestine ✨

> 🇰🇷 이 저장소는 **molpass가 포크한 사본**입니다. 영어 원문은 [README.en.md](./README.en.md)를 참고하세요.

[![npm version](https://img.shields.io/npm/v/celestine.svg?style=flat-square)](https://www.npmjs.com/package/celestine)
[![npm downloads](https://img.shields.io/npm/dm/celestine.svg?style=flat-square)](https://www.npmjs.com/package/celestine)
[![CI](https://img.shields.io/github/actions/workflow/status/Anonyfox/celestine/ci.yml?branch=main&label=CI&style=flat-square)](https://github.com/Anonyfox/celestine/actions/workflows/ci.yml)
[![Documentation](https://img.shields.io/badge/docs-live-brightgreen?style=flat-square)](https://anonyfox.github.io/celestine)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18-green?style=flat-square&logo=node.js)](https://nodejs.org/)

<div align="center">
  <img src="https://raw.githubusercontent.com/Anonyfox/celestine/main/assets/celestine-logo.png" alt="Celestine Logo" width="400" />
</div>

**군더더기 없는 점성술 계산.** Celestine은 출생 차트(birth chart), 트랜짓(transit), 프로그레션(progression), 그리고 진지한 점성술 소프트웨어에 필요한 모든 것을 위한 TypeScript 라이브러리입니다. 모든 계산은 NASA 데이터, JPL Horizons, 그리고 Swiss Ephemeris에 대해 검증됩니다 — "영감을 받았다"거나 "기반으로 했다"가 아니라, 천문학자들이 실제로 사용하는 원천 데이터와 바이트 단위로 일치하도록 검증됩니다.

정밀도를 진정으로 중시하는 실무자를 위해 만들어졌습니다. 2400개 이상의 단위 테스트. 런타임 의존성 0개. 계산이 틀리면 테스트가 잡아냅니다. NASA가 화성이 127.4532°에 있다고 하면, Celestine도 127.4532°를 줍니다. 127.45°나 "대충 그 정도"가 아니라 — 매번 정확한 실제 숫자를 줍니다.

## 기능 Features

- ✅ **출생 차트(Birth Charts)** - 모든 모듈을 하나의 일관된 API로 결합한 완전한 차트 계산
- ✅ **천체력(Ephemeris)** - 태양, 달, 모든 행성, 키론(Chiron), 4대 소행성, 달의 교점, 릴리스(Lilith), 파트(lots)
- ✅ **JPL/Swiss Ephemeris 검증** - 권위 있는 천문 데이터에 대해 검증된 위치 값
- ✅ **시간 계산(Time Calculations)** - 율리우스일, 율리우스 세기, 항성시, ΔT 보정
- ✅ **NASA 검증** - 공식 NASA 기준 데이터에 대해 테스트된 시간 계산
- ✅ **하우스 시스템(House Systems)** - 7종(Placidus, Koch, Equal, Whole Sign, Porphyry, Regiomontanus, Campanus)
- ✅ **천문학적 검증** - 기본 천문학 원리에 대해 검증된 하우스 계산
- ✅ **황도대 시스템(Zodiac System)** - 회귀 황도대(tropical zodiac), 별자리·품위(dignity)·완전한 메타데이터 포함
- ✅ **전통 점성술(Traditional Astrology)** - 행성 지배(rulership)와 본질적 품위(essential dignities) (Ptolemy, Lilly)
- ✅ **역행 감지(Retrograde Detection)** - 모든 천체의 역행 운동 자동 감지
- ✅ **애스펙트(Aspects)** - 14종 애스펙트(메이저·마이너·케플러), 패턴, 오브(orb), 접근/분리(applying/separating)
- ✅ **트랜짓(Transits)** - 정확한 타이밍, 하우스 진입, 역행 처리를 갖춘 예측 점성술
- ✅ **프로그레션(Progressions)** - 2차 프로그레션, 솔라 아크 디렉션, 달의 위상 및 별자리 이동
- 🔒 **타입 안전(Type-safe)** - 포괄적인 타입을 갖춘 완전한 TypeScript 지원
- 🧪 **충분한 테스트** - 2400개 이상의 단위 테스트
- 🚀 **런타임 의존성 0개** - 가볍고 빠름

## 설치 Installation

```bash
npm install celestine
```

## 빠른 시작 Quick Start

```typescript
import { calculateChart, ephemeris, time, zodiac } from "celestine";

// Calculate a complete birth chart
const chart = calculateChart({
  year: 2000,
  month: 1,
  day: 1,
  hour: 12,
  minute: 0,
  second: 0,
  timezone: 0,
  latitude: 51.5074, // London
  longitude: -0.1278,
});

console.log(`Rising: ${chart.angles.ascendant.formatted}`);
console.log(`Sun: ${chart.planets[0].formatted}`);
console.log(`Aspects: ${chart.aspects.all.length}`);

// Or use individual modules for more control
const jd = time.toJulianDate({ year: 2000, month: 1, day: 1, hour: 12 });
const sun = ephemeris.getSunPosition(jd);
const position = zodiac.eclipticToZodiac(sun.longitude);
console.log(position.formatted); // "10°27' Capricorn"

// Check planetary dignity
const marsAries = zodiac.getPlanetaryDignity(
  zodiac.Planet.Mars,
  zodiac.Sign.Aries
);
console.log(marsAries.state); // "Domicile" (Mars rules Aries)
```

## 사용법 Usage

### 천체력(Ephemeris) — 행성 위치

JPL Horizons와 Swiss Ephemeris에 대해 검증된, 임의의 순간에서의 천체 정밀 위치를 계산합니다.

```typescript
import { ephemeris, time } from "celestine";

// Get Julian Date for a specific moment
const jd = time.toJulianDate({ year: 2000, month: 1, day: 1, hour: 12 });

// Get individual body positions
const sun = ephemeris.getSunPosition(jd);
console.log(sun.longitude); // 280.46° (Capricorn)
console.log(sun.distance); // 0.983 AU
console.log(sun.isRetrograde); // false

const moon = ephemeris.getMoonPosition(jd);
console.log(moon.longitude); // 223.32° (Scorpio)
console.log(moon.latitude); // 5.17°

// Get all positions at once using the unified API
const positions = ephemeris.getAllPositions(jd);
console.log(positions.get(ephemeris.CelestialBody.Mars));

// Check retrograde status
const mercury = ephemeris.getMercuryPosition(jd);
if (mercury.isRetrograde) {
  console.log("Mercury retrograde!");
}

// Get zodiac sign for a body
const sign = ephemeris.getSign(ephemeris.CelestialBody.Venus, jd);
console.log(sign); // "Sagittarius"
```

**지원 천체:**

- **광체(Luminaries)**: 태양, 달
- **고전 행성(Classical Planets)**: 수성, 금성, 화성, 목성, 토성
- **현대 행성(Modern Planets)**: 천왕성, 해왕성, 명왕성
- **켄타우로스(Centaur)**: 키론(Chiron)
- **소행성(Asteroids)**: 세레스(Ceres), 팔라스(Pallas), 주노(Juno), 베스타(Vesta)
- **달의 교점(Lunar Nodes)**: 평균 교점(Mean Node), 진교점(True Node) (북·남)
- **릴리스(Lilith)**: 평균 릴리스(Mean Lilith), 진릴리스(True Lilith, 블랙문)
- **파트(Lots)**: 행운의 점(Part of Fortune), 정신의 점(Part of Spirit)

**특징:**

- 지심(geocentric) 황도 좌표(경도, 위도, 거리)
- longitudeSpeed를 통한 자동 역행 감지
- 1800–2200년에 대해 ±1 각분(arcminute) 정확도
- Jean Meeus의 "Astronomical Algorithms"와 VSOP87 이론에 기반
- 모든 위치가 JPL Horizons 및 Swiss Ephemeris 기준 데이터에 대해 검증됨

### 시간 계산 Time Calculations

Time 모듈은 NASA 기준 데이터에 대해 모두 검증된 포괄적인 천문 시간 계산을 제공합니다.

```typescript
import { time } from "celestine";

// Julian Date conversions
const jd = time.toJulianDate({
  year: 2025,
  month: 12,
  day: 18,
  hour: 15,
  minute: 30,
  second: 0,
});
const date = time.fromJulianDate(jd); // Convert back to calendar date

// Julian Centuries from J2000.0 epoch
const T = time.toJulianCenturies(jd); // For ephemeris calculations

// Sidereal Time (for house calculations)
const gmst = time.greenwichMeanSiderealTime(jd); // Greenwich Mean Sidereal Time
const lst = time.localSiderealTime(gmst, -5.0); // Local Sidereal Time (longitude: -5°)

// Delta T corrections (TT ↔ UT)
const deltaT = time.deltaT(2025, 12); // ΔT in seconds
const ttJD = time.utToTT(jd, 2025, 12); // Convert UT to Terrestrial Time

// Time validation
const isValid = time.isValidDate(2024, 2, 29); // true (leap year)
const validation = time.validateCalendarDateTime({
  year: 2025,
  month: 12,
  day: 18,
  hour: 15,
  minute: 30,
  second: 0,
}); // { valid: true, errors: [] }

// Constants
console.log(time.J2000_EPOCH); // 2451545.0
console.log(time.DAYS_PER_CENTURY); // 36525
```

**제공 기능:**

- 율리우스일 ↔ 달력 날짜 (Meeus 알고리즘, 그레고리력/율리우스력 처리)
- J2000.0 기준점부터의 율리우스 세기
- 그리니치 평균 항성시 및 지방 항성시
- ΔT 보정 (Espenak & Meeus 다항식, NASA 데이터에 대해 검증)
- 날짜/시간 검증 (윤년, 월 경계 등)
- 시간 유틸리티 (각도 정규화, 변환, 포매팅)
- 36개의 천문 상수 (J2000, Unix 기준점, 항성일 등)

모든 계산은 권위 있는 자료(Meeus, NASA, IERS)에 기반하며, 17개의 NASA 공식 기준값을 포함한 309개 단위 테스트로 검증되었습니다.

### 하우스 계산 House Calculations

여러 하우스 시스템을 사용해 점성술 하우스 커스프(cusp)와 앵글(angle)을 계산하며, 모두 Swiss Ephemeris에 대해 검증됩니다.

```typescript
import { houses } from "celestine";

// Calculate houses for a birth chart
const birthChart = houses.calculateHouses(
  { latitude: 51.5074, longitude: -0.1278 }, // London
  245.5, // Local Sidereal Time in degrees
  23.4368, // Obliquity of ecliptic
  "placidus" // House system
);

console.log(birthChart.angles);
// { ascendant: 120.5, midheaven: 285.3, descendant: 300.5, imumCoeli: 105.3 }

console.log(birthChart.cusps);
// { cusps: [120.5, 145.2, 172.8, ...] } // 12 house cusps

// Supported house systems
const systems = [
  "placidus",
  "koch",
  "equal",
  "whole-sign",
  "porphyry",
  "regiomontanus",
  "campanus",
];
```

**제공 기능:**

- 7종 하우스 시스템 (Placidus, Koch, Equal, Whole Sign, Porphyry, Regiomontanus, Campanus)
- ASC, MC, DSC, IC(차트 앵글) 계산
- 황도 경사각(obliquity of ecliptic) (Laskar 1986 공식)
- 유용한 오류 메시지를 갖춘 지리 위치 검증
- 하우스 위치 조회 및 황도대 유틸리티
- 복잡한 시스템에 대한 Swiss Ephemeris 알고리즘의 직접 포팅
- 천문학적 원리(각도 관계, 하우스 간격, 적도 거동)에 대해 검증됨

### 황도대 시스템 & 행성 품위 Zodiac System & Planetary Dignities

전통 점성술 교리에 기반해 회귀 황도대 위치와 본질적 품위를 계산합니다.

```typescript
import { zodiac } from "celestine";

// Convert ecliptic longitude to zodiac position
const venus = zodiac.eclipticToZodiac(217.411111);
console.log(venus);
// {
//   sign: Sign.Scorpio,
//   signName: 'Scorpio',
//   degree: 7,
//   minute: 24,
//   second: 40,
//   formatted: "7°24'40\" Scorpio"
// }

// Get sign properties
const aries = zodiac.getSignInfo(zodiac.Sign.Aries);
console.log(aries);
// {
//   element: Element.Fire,
//   modality: Modality.Cardinal,
//   polarity: Polarity.Positive,
//   ruler: Planet.Mars,
//   symbol: '♈'
// }

// Check planetary dignity
const sunAries = zodiac.getPlanetaryDignity(
  zodiac.Planet.Sun,
  zodiac.Sign.Aries
);
console.log(sunAries);
// {
//   state: DignityState.Exaltation,
//   strength: 4,  // +4 for exaltation
//   exaltationDegree: 19,
//   description: 'Sun exalted in Aries'
// }

// Check multiple dignities
zodiac.isRuler(zodiac.Planet.Mars, zodiac.Sign.Aries); // true (Mars rules Aries)
zodiac.isExalted(zodiac.Planet.Sun, zodiac.Sign.Aries); // true (Sun exalted in Aries)
zodiac.isDetriment(zodiac.Planet.Mars, zodiac.Sign.Libra); // true (opposite Aries)

// Format with options
const formatted = zodiac.formatZodiacPosition(venus, {
  useSymbol: true, // Use ♏ instead of "Scorpio"
  includeSeconds: false, // Omit seconds
});
// "7°24' ♏"
```

**제공 기능:**

- 황도 경도 → 황도대 별자리 + DMS(도/분/초)
- 완전한 별자리 속성 (원소, 양태(modality), 극성, 지배성, 심볼)
- 본질적 품위 (입궁/도미사일, 고양(exaltation), 손상(detriment), 추락(fall), 페러그린(peregrine))
- 전통 체계에 따른 강도 점수 (+5, +4, 0, -4, -5)
- 전통 지배성(Ptolemy) + 현대 지배성(천왕성, 해왕성, 명왕성)
- 정확한 고양 도수 (태양 양자리 19°, 화성 염소자리 28° 등)
- 모든 별자리(♈-♓)와 행성(☉☽☿♀♂♃♄♅♆♇)에 대한 유니코드 심볼
- 유연한 포매팅 옵션 (심볼, 십진 도수, DMS)
- Swiss Ephemeris에 대해 검증된 알고리즘
- 모든 데이터가 Ptolemy의 "Tetrabiblos"와 Lilly의 "Christian Astrology"에 대해 검증됨

### 애스펙트 Aspects — 각 관계

설정 가능한 오브, 패턴 감지, 접근/분리 지표와 함께 천체 간 애스펙트를 계산합니다.

```typescript
import { aspects, ephemeris, time } from "celestine";

// Get planetary positions
const jd = time.toJulianDate({ year: 2000, month: 1, day: 1, hour: 12 });
const sun = ephemeris.getSunPosition(jd);
const moon = ephemeris.getMoonPosition(jd);
const mars = ephemeris.getMarsPosition(jd);

// Create bodies array for aspect detection
const bodies = [
  { name: "Sun", longitude: sun.longitude, longitudeSpeed: sun.longitudeSpeed },
  {
    name: "Moon",
    longitude: moon.longitude,
    longitudeSpeed: moon.longitudeSpeed,
  },
  {
    name: "Mars",
    longitude: mars.longitude,
    longitudeSpeed: mars.longitudeSpeed,
  },
];

// Find all aspects between bodies
const result = aspects.calculateAspects(bodies);

for (const aspect of result.aspects) {
  console.log(aspects.formatAspect(aspect));
  // "Sun ⚹ Moon (2°57', 51%, separating)"
}

// Detect aspect patterns (T-Square, Grand Trine, Yod, etc.)
const patterns = aspects.findPatterns(result.aspects);
for (const pattern of patterns) {
  console.log(`${pattern.type}: ${pattern.bodies.join(", ")}`);
}

// Calculate angular separation
const separation = aspects.angularSeparation(sun.longitude, moon.longitude);
console.log(separation); // 57.05°

// Check if within orb of an aspect
const match = aspects.findMatchingAspect(separation);
if (match) {
  console.log(`${match.aspect.name} with ${match.deviation}° orb`);
}
```

**지원 애스펙트 (14종):**

- **메이저(프톨레마이오스)**: 합 ☌, 섹스타일 ⚹, 스퀘어 □, 트라인 △, 어포지션 ☍
- **마이너**: 세미섹스타일 ⚺, 세미스퀘어 ∠, 퀸타일 Q, 세스퀴쿼드레이트 ⚼, 바이퀸타일 bQ, 퀸컹크스 ⚻
- **케플러**: 셉타일 S (360°/7), 노바일 N (360°/9), 데사일 D (360°/10)

**애스펙트 패턴:**

- T-스퀘어 (어포지션 + 스퀘어 2개)
- 그랜드 트라인 (트라인 3개)
- 그랜드 크로스 (스퀘어 4개 + 어포지션 2개)
- 요드 / 신의 손가락 (퀸컹크스 2개 + 섹스타일)
- 카이트 (그랜드 트라인 + 어포지션)
- 미스틱 렉탱글 (어포지션 2개 + 트라인 2개 + 섹스타일 2개)
- 스텔리움 (합 3개 이상)

**특징:**

- 애스펙트 종류별 설정 가능한 오브
- 강도 계산 (100% = 정확, 오브 가장자리로 갈수록 감소)
- 접근(applying) vs 분리(separating) 애스펙트 감지
- 별자리를 벗어난(dissociate) 애스펙트 감지
- 복잡한 구성에 대한 패턴 감지
- 수학적으로 정확한 각도의 케플러 애스펙트 ("Harmonices Mundi", 1619)

### 출생 차트 Birth Charts

모든 모듈을 하나의 일관된 API로 결합해 완전한 점성술 출생 차트를 계산합니다. 모든 계산은 Swiss Ephemeris에 대해 검증됩니다.

```typescript
import { calculateChart, CelestialBody } from "celestine";

// Calculate a complete birth chart
const chart = calculateChart({
  year: 1879,
  month: 3,
  day: 14,
  hour: 11,
  minute: 30,
  second: 0,
  timezone: 0.667, // LMT offset (or use standard timezone)
  latitude: 48.4,
  longitude: 10.0,
});

// Access planetary positions
for (const planet of chart.planets) {
  console.log(`${planet.name}: ${planet.formatted} in House ${planet.house}`);
}
// "Sun: 23°30' Pisces in House 9"
// "Moon: 14°31' Sagittarius in House 6"

// Access chart angles
console.log(`Rising: ${chart.angles.ascendant.formatted}`);
// "Rising: 11°38' Cancer"
console.log(`Midheaven: ${chart.angles.midheaven.formatted}`);
// "Midheaven: 12°50' Pisces"

// Access house cusps
for (const [num, cusp] of Object.entries(chart.houses.cusps)) {
  console.log(`House ${num}: ${cusp.formatted}`);
}

// Access aspects
for (const aspect of chart.aspects.all) {
  console.log(`${aspect.body1Name} ${aspect.symbol} ${aspect.body2Name}`);
}

// Access chart summary
console.log(chart.summary.elements);
// { fire: 3, earth: 1, air: 2, water: 4 }
console.log(chart.summary.modalities);
// { cardinal: 2, fixed: 3, mutable: 5 }
console.log(chart.summary.retrograde);
// ['Uranus']
console.log(chart.summary.patterns);
// [{ type: 'TSquare', bodies: ['Sun', 'Moon', 'Saturn'] }]
```

**차트 옵션:**

```typescript
const chart = calculateChart(birthData, {
  houseSystem: "placidus", // 'koch', 'equal', 'whole-sign', etc.
  includeAsteroids: true, // Ceres, Pallas, Juno, Vesta
  includeChiron: true, // Chiron
  includeLilith: true, // Black Moon Lilith
  includeNodes: true, // Lunar Nodes
  includeLots: true, // Part of Fortune, Part of Spirit
  aspectTypes: "major", // 'all', 'major', or custom array
  zodiacType: "tropical", // Currently only tropical supported
});
```

**그 외 제공:**

```typescript
import {
  calculateChart,
  calculatePlanets,
  calculateHouseCusps,
  validateBirth,
  formatChart,
  getAvailableHouseSystems,
} from "celestine";

// Calculate only planetary positions
const planets = calculatePlanets(birthData);

// Calculate only house cusps and angles
const houses = calculateHouseCusps(birthData, { houseSystem: "koch" });

// Validate birth data before calculation
const validation = validateBirth(birthData);
if (!validation.valid) {
  console.log(validation.errors);
}

// Format chart for display
console.log(formatChart(chart, "text"));

// Get available house systems
const systems = getAvailableHouseSystems(); // ['placidus', 'koch', ...]
```

**특징:**

- 행성, 하우스, 애스펙트, 요약을 갖춘 완전한 출생 차트
- 7종 하우스 시스템 모두 지원
- 각 행성에 대한 자동 품위 계산
- 애스펙트 패턴 감지 (T-스퀘어, 그랜드 트라인, 요드 등)
- 원소/양태/극성 분포 분석
- 반구(hemisphere) 및 사분면(quadrant) 강조 분석
- 역행 행성 추적
- Swiss Ephemeris 기준 데이터에 대해 검증
- 아인슈타인의 차트를 Swiss Ephemeris 2.10.03에 대해 검증

### 트랜짓 Transits — 예측 점성술

현재 행성 위치가 출생 차트 위치와 애스펙트를 이루는 시점을 계산합니다 — 예측 점성술의 토대입니다.

```typescript
import { transits, time, type NatalPoint } from "celestine";

// Define natal points from a birth chart
const natalPoints: NatalPoint[] = [
  { name: "Sun", longitude: 280.37, type: "planet" },
  { name: "Moon", longitude: 223.32, type: "luminary" },
  { name: "ASC", longitude: 101.65, type: "angle" },
];

// Calculate transits at a specific moment
const jd = time.toJulianDate({ year: 2025, month: 12, day: 19, hour: 12 });
const result = transits.calculateTransits(natalPoints, jd);

for (const transit of result.transits) {
  console.log(transits.formatTransit(transit));
  // "Saturn □ Sun (1°23', 85%, applying)"
}

// Search transits over a date range
const searchResult = transits.searchTransits({
  natalPoints,
  startJD: jd,
  endJD: jd + 365, // One year
  transitingBodies: [
    transits.CelestialBody.Saturn,
    transits.CelestialBody.Jupiter,
  ],
});

// Get timing details for each transit
for (const timing of searchResult.transits) {
  console.log(
    `${timing.transit.transitingBody} ${timing.transit.symbol} ${timing.transit.natalPoint}`
  );
  console.log(
    `  Enters orb: ${timing.enterOrbDate?.month}/${timing.enterOrbDate?.day}`
  );
  console.log(
    `  Exact: ${timing.exactDates[0]?.month}/${timing.exactDates[0]?.day}`
  );
  console.log(
    `  Exits orb: ${timing.exitOrbDate?.month}/${timing.exitOrbDate?.day}`
  );
}

// Detect house ingresses
const houseCusps = [0, 30, 60, 90, 120, 150, 180, 210, 240, 270, 300, 330];
const ingresses = transits.calculateAllIngresses(
  transits.CelestialBody.Mars,
  houseCusps,
  jd,
  jd + 365
);

// Find retrograde periods
const mercuryRx = transits.findRetrogradePeriods(
  transits.CelestialBody.Mercury,
  jd,
  jd + 365
);

for (const period of mercuryRx) {
  console.log(transits.formatRetrogradePeriod(period));
  // "Mercury Rx: Dec 13 - Jan 2 (21 days), 8°23' Capricorn → 22°10' Sagittarius"
}
```

**트랜짓 종류:**

- **행성-행성(Planet-to-Planet)**: 트랜짓 토성이 출생 태양과 스퀘어
- **행성-앵글(Planet-to-Angle)**: 트랜짓 목성이 출생 ASC와 합
- **행성-하우스(Planet-to-House)**: 화성이 7번째 하우스에 진입
- **역행 트랜짓(Retrograde Transits)**: 역행 운동으로 인한 다중 통과

**특징:**

- 접근/분리 위상을 갖춘 트랜짓 감지
- 강도 계산 (100% = 정확, 오브 가장자리로 갈수록 감소)
- 광체와 앵글에 대한 확장이 가능한 설정형 오브
- 이분 탐색(binary search)을 통한 정확한 트랜짓 시각 탐색
- 전체 트랜짓 기간에 대한 오브 진입/이탈 타이밍
- 하우스 진입(ingress) 감지 (행성이 출생 하우스에 진입/이탈)
- 정류점(station point) 감지를 포함한 역행 운동 처리
- 느린 행성에 대한 다중 트랜짓 통과
- 월/천체/출생점별 그룹화를 갖춘 날짜 범위 검색
- 역사적 트랜짓 사건 및 Swiss Ephemeris에 대해 검증

### 프로그레션 Progressions — 예측 점성술

2차 프로그레션, 솔라 아크 디렉션 및 기타 프로그레션 기법을 사용해 출생 차트가 시간에 따라 어떻게 전개되는지 계산합니다.

```typescript
import {
  progressions,
  calculateProgression,
  formatProgressedChart,
} from "celestine";

// Define birth data
const birth = {
  year: 1990,
  month: 6,
  day: 15,
  hour: 14,
  minute: 30,
  second: 0,
  timezone: -5,
  latitude: 40.7128, // New York
  longitude: -74.006,
};

// Calculate secondary progressions for a target date
const target = { year: 2024, month: 1, day: 1 };
const result = calculateProgression(birth, target);

console.log(`Age: ${result.ageAtTarget.toFixed(1)} years`);
console.log(`Solar Arc: ${result.solarArc.toFixed(2)}°`);

// View progressed positions
for (const body of result.bodies) {
  console.log(`${body.name}: ${body.progressedFormatted}`);
  if (body.hasChangedSign) {
    console.log(
      `  ★ Changed from ${body.natalSignName} to ${body.progressedSignName}`
    );
  }
}

// Check progressed angles
console.log(`Progressed ASC: ${result.angles.ascendant.progressedFormatted}`);
console.log(`Progressed MC: ${result.angles.midheaven.progressedFormatted}`);

// View aspects between progressed and natal positions
for (const aspect of result.aspects.slice(0, 5)) {
  console.log(
    `${aspect.progressedBody} ${aspect.aspectType} ${aspect.natalBody}`
  );
}

// Get detailed progressed Moon report
const moonReport = progressions.getProgressedMoonReport(birth, target);
console.log(`Moon Phase: ${moonReport.phase.phaseName}`);
console.log(`Moon Sign Transits:`);
for (const transit of moonReport.signTransits.slice(0, 5)) {
  console.log(
    `  ${transit.signName}: age ${transit.entryAge.toFixed(
      1
    )} - ${transit.exitAge.toFixed(1)}`
  );
}

// Use solar arc directions
const solarArc = progressions.calculateSolarArc(
  result.birthJD,
  result.targetJD
);
console.log(`Solar Arc: ${progressions.formatSolarArc(solarArc)}`);

// Format complete result
console.log(formatProgressedChart(result));
```

**프로그레션 종류:**

- **2차 프로그레션(Secondary Progressions)**: 1일 = 1년 (가장 흔한 기법)
- **솔라 아크 디렉션(Solar Arc Directions)**: 모든 점이 태양의 프로그레션 운동만큼 전진
- **마이너 프로그레션(Minor Progressions)**: 항성월 1개월 = 1년
- **3차 프로그레션(Tertiary Progressions)**: 1일 = 1개월

**특징:**

- 완전한 프로그레션 차트 계산
- 솔라 아크 계산 및 디렉션
- 프로그레션-출생 애스펙트 감지
- 모든 천체의 별자리 변경 추적
- 전문적인 프로그레션 달 분석
- 달 별자리 이동 타임라인
- 프로그레션 달 위상 계산
- 다양한 프로그레션 종류 지원
- 설정 가능한 오브 및 애스펙트 종류
- 보기 좋게 포매팅된 출력

## API 문서 API Documentation

전체 API 문서는 [https://anonyfox.github.io/celestine](https://anonyfox.github.io/celestine) 에서 확인할 수 있습니다.

## 개발 Development

```bash
npm install        # Install dependencies
npm test           # Run tests
npm run build      # Build package
npm run docs       # Generate documentation
npm run lint       # Check linting
npm run format     # Format code
```

---

<div align="center">

**[Anonyfox](https://anonyfox.com) • [MIT License](LICENSE)**

</div>

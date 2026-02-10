# HTML Presentation Template (PowerPoint Style)

> เทมเพลตสำหรับสร้างสไลด์นำเสนอจากไฟล์ HTML เดียว แสดงผลเหมือน PowerPoint จริง
> พร้อมปุ่ม Export เป็นไฟล์ .pptx และ .pdf ได้

---

## สรุปรูปแบบ

| รายการ | รายละเอียด |
|--------|------------|
| ไฟล์หลัก | `present.html` — HTML ไฟล์เดียว (self-contained) |
| ขนาดสไลด์ | **1920 x 1080 px** (Full HD 16:9) — capture แล้วคมชัดไม่แตก |
| โทนสี | ขาว-เทา สะอาดตา (สำหรับผู้บริหาร) |
| ฟอนต์ | **Sarabun** + Inter (Google Fonts) — คล้าย TH SarabunPSK |
| พื้นหลังนอกสไลด์ | สีดำ (#111111) |
| Navigation | ปุ่มซ้าย/ขวา, Dots, Keyboard (←→ Space Home End), Touch Swipe, F=Fullscreen |
| Export PPTX | PptxGenJS สร้าง .pptx จริงฝั่ง client → `JBFarmHUB_PPTX-ระบบจัดการฟาร์ม.pptx` |
| Export PDF | window.print() → Save as PDF → `JBFarmHUB_PDF-ระบบจัดการฟาร์ม.pdf` |
| Auto-scale | สไลด์ย่อ/ขยายพอดีจออัตโนมัติ (ไม่ขยายเกิน 1:1) |
| โลโก้ | แสดงมุมบนขวาทุกสไลด์ (class `.slide-logo`, height 110px) |
| ภาพหน้าปก | ใช้ไฟล์ภาพ PNG แทน emoji (ไม่แตกเมื่อขยาย) |

---

## โครงสร้าง HTML

```
<!DOCTYPE html>
<html lang="th">
<head>
  <!-- Google Fonts: Sarabun + Inter -->
  <!-- CSS ทั้งหมดอยู่ใน <style> รวม @media print สำหรับ PDF -->
</head>
<body>
  <!-- DECK: กล่องสไลด์ขนาดคงที่ 1920x1080 อยู่กลางจอ -->
  <div class="deck" id="deck">
    <div class="progress" id="prog"></div>   <!-- แถบ progress bar -->

    <div class="slide active">               <!-- Slide 1 (active = แสดง) -->
      <img class="slide-logo" src="logo.png">
      ...เนื้อหา...
    </div>
    <div class="slide">                       <!-- Slide 2 -->
      <img class="slide-logo" src="logo.png">
      ...เนื้อหา...
    </div>
    <!-- ...Slide N... -->
  </div>

  <!-- ปุ่ม Download (มุมบนขวาจอ) -->
  <div class="dl-wrap">
    <button class="dl-btn" onclick="exportPPTX()">PPTX</button>
    <button class="dl-btn pdf" onclick="exportPDF()">PDF</button>
  </div>

  <!-- Navigation Bar (ล่างกลางจอ) -->
  <div class="nav">
    <button>◀</button>
    <div class="dots" id="dots"></div>
    <span class="num" id="num">1 / 4</span>
    <button>▶</button>
  </div>

  <!-- Scripts -->
  <script>/* Slide engine + Auto-scale */</script>
  <script src="pptxgen.bundle.js"></script>
  <script>/* exportPPTX() + exportPDF() */</script>
</body>
</html>
```

---

## CSS Architecture

### CSS Variables (ปรับสีหลักที่นี่)
```css
:root {
  --bg: #ffffff;        /* พื้นหลังสไลด์ */
  --bg2: #f7f8fa;       /* พื้นหลังการ์ด/แถว */
  --bg3: #eef0f4;       /* พื้นหลังเข้มขึ้น */
  --border: #e2e5eb;    /* เส้นขอบ */
  --text: #1a1d23;      /* ข้อความหลัก */
  --text2: #5c6270;     /* ข้อความรอง */
  --text3: #8b91a0;     /* ข้อความจาง */
  --accent: #2563eb;    /* สีเน้นหลัก (น้ำเงิน) */
  --accent-light: #eff4ff;
  --green: #059669;     /* สีเขียว */
  --orange: #d97706;    /* สีส้ม */
  --red: #dc2626;       /* สีแดง */
  --purple: #7c3aed;    /* สีม่วง */
}
```

### CSS Classes สำคัญ

| Class | หน้าที่ |
|-------|---------|
| `.deck` | กล่องสไลด์ 1920x1080 อยู่กลางจอ มี auto-scale |
| `.slide` | สไลด์แต่ละหน้า (absolute ซ้อนกัน, opacity 0/1) |
| `.slide.active` | สไลด์ที่แสดงอยู่ |
| `.fu` | Fade-up animation (ใส่ให้ child elements) |
| `.pad` | Padding layout สำหรับเนื้อหา (68px 86px) |
| `.center` | จัดกลางทั้งแนวตั้ง/นอน |
| `.kicker` | ป้ายเล็กๆ มุมบน (เช่น "Slide 2/6") พื้นหลังพอดีข้อความ (`width: fit-content`) |
| `.g2` `.g3` `.g5` | Grid 2/3/1 คอลัมน์ |
| `.card` | การ์ดมีขอบ + hover effect (padding 36px) |
| `.card.ca` `.cg` `.co` `.cp` | การ์ดมีเส้นสีด้านบน (accent/green/orange/purple, border-top 4px) |
| `.obj` | แถววัตถุประสงค์ (วงกลมตัวเลข 54px + ข้อความ) |
| `.user-row` | แถวผู้ใช้งาน (ไอคอน 52px + ชื่อ + คำอธิบาย) |
| `.benefit` | แถวประโยชน์ (✔ 26px + ข้อความ 19px) |
| `.slide-logo` | โลโก้บริษัทมุมบนขวาทุกสไลด์ (height 110px, absolute) |
| `.cover-deco` | พื้นที่ตกแต่งฝั่งขวาของหน้าปก (clip-path เฉียง, 45% width) |
| `.big-icon` | ภาพหมูหน้าปก (300x300px, ใช้ไฟล์ PNG ไม่ใช่ emoji เพื่อไม่แตก) |
| `.dl-wrap` | กลุ่มปุ่ม Download มุมบนขวาจอ (fixed) |
| `.dl-btn` | ปุ่ม Download (hover น้ำเงิน) |
| `.dl-btn.pdf` | ปุ่ม PDF (hover แดง) |
| `.nav` | แถบ Navigation ด้านล่าง (พื้นเข้ม, blur) |
| `.progress` | Progress bar ด้านบนสไลด์ |
| `.sec-label` | หัวหมวดย่อย (18px, uppercase, letter-spacing) |

---

## รูปแบบสไลด์ 4 หน้า

### Slide 1: Cover (หน้าปก)
```
┌─────────────────────────────┬──────────────┐
│ [LOGO มุมบนขวา]             │              │
│                              │              │
│  [kicker: การนำเสนอ...]     │  🐷 (ภาพ PNG)│
│  [h1: ชื่อระบบ]              │  300x300px   │
│  [h2: คำอธิบาย]              │  (พื้นเทา)   │
│  [sub: รายละเอียด]           │              │
│  [date-tag: วันที่]          │              │
│                              │              │
└─────────────────────────────┴──────────────┘
```
- ซ้าย 55%: เนื้อหา (`cover-left`, padding 90px) | ขวา 45%: `cover-deco` พื้นเทา + ภาพ PNG
- ใช้ `clip-path: polygon()` ตัดขอบเฉียง
- ภาพหน้าปกใช้ไฟล์ PNG (ไม่ใช่ emoji) เพื่อไม่แตกเมื่อขยาย

### Slide 2: วัตถุประสงค์ + ผู้ใช้งาน (2 คอลัมน์)
```
┌──────────────────┬──────────────────┐
│ วัตถุประสงค์ (5 ข้อ) │ กลุ่มผู้ใช้งาน      │
│                  │                  │
│ [1] ● หัวข้อ      │ 🛡️ Admin/IT     │
│ [2] ● หัวข้อ      │ 👔 ผู้บริหาร      │
│ [3] ● หัวข้อ      │ 🌾 ผจก.ฟาร์ม    │
│ [4] ● หัวข้อ      │ 📋 หัวหน้างาน    │
│ [5] ● หัวข้อ      │ 👷 พนักงาน      │
│                  │ 👁️ ภายนอก      │
└──────────────────┴──────────────────┘
```
- **ซ้าย (Objectives):** ใช้ class `.obj` (วงกลมตัวเลข 54px สีต่างกัน + h4 + p)
- **ขวา (User Groups):** ใช้ class `.user-row` (ไอคอน 52px + h4 + p)
- ใช้ `.g2` grid 2 คอลัมน์ แบ่งครึ่งหน้าจอ

### Slide 3: โมดูลหลัก (8 โมดูล)
```
┌──────────────────┬──────────────────┬──────────────────┬──────────────────┐
│ 🔐 พื้นฐาน       │ 🛒 จัดซื้อ        │ 📦 คลังสินค้า     │ 🐷 การผลิต       │
│                  │                  │                  │                  │
├──────────────────┼──────────────────┼──────────────────┼──────────────────┤
│ 💉 วัคซีน        │ 💰 การขาย        │ 🔧 ซ่อมบำรุง      │ 📊 การเงิน       │
│                  │                  │                  │                  │
└──────────────────┴──────────────────┴──────────────────┴──────────────────┘
```
- ใช้ `grid-template-columns: repeat(4, 1fr)` 4 คอลัมน์ (gap 32px)
- แสดงไอคอนขนาดใหญ่ (64px) + ชื่อโมดูล (18px)
- การ์ดพื้นหลังเรียบง่าย (`background: var(--bg2)`) เน้นความสะอาดตา

### Slide 4: Timeline (แนวนอน จุดต่อจุด สไตล์ขนส่ง)
```
┌────────────────────────────────────────────┐
│ [h2: แผนงาน 22 สัปดาห์]                     │
│                                            │
│  🚀──🔐──🛒──📦──🐷──💰──🔧──📊           │
│  64px วงกลม, เส้น gradient 5px             │
│  WK (15px) + ชื่อ (18px) + รายละเอียด (15px)│
│                                            │
│  ● Phase 1  ● Phase 2  ● Phase 3  ● Finish│
│  (pill 16px, dot 14px)                     │
│  Tech: Next.js | .NET 10 | PostgreSQL (17px)│
└────────────────────────────────────────────┘
```
- เส้นแนวนอน gradient สี 5px (accent→green→orange→purple)
- 8 จุดวงกลม 64px (border ขาว 5px + box-shadow) มีไอคอน emoji 28px
- ช่องสถานี 185px กว้าง
- ใต้จุด: สัปดาห์ (15px สี phase) + ชื่อ (18px bold) + รายละเอียด 3 บรรทัด (15px)
- Legend: pill สี 4 Phase (dot 14px + text 16px)
- Tech Stack: pill 17px แนวนอน
- จุดสุดท้ายมี 🏁 Go Live

---

## Slide Engine (JavaScript)

### Auto-scale
```javascript
function scaleDeck() {
  const sw = 1920, sh = 1080;               // ขนาดสไลด์จริง (Full HD)
  const vw = window.innerWidth;
  const vh = window.innerHeight - 80;        // เว้นพื้นที่ nav
  const s = Math.min(vw / sw, vh / sh, 1);  // ไม่ขยายเกิน 1:1
  deck.style.transform = `translate(-50%,-50%) scale(${s})`;
}
```

### Navigation
```javascript
// ปุ่ม ◀ ▶ / Dots / Keyboard (←→ Space Home End) / Touch Swipe / F=Fullscreen
function go(d) { cur += d; render(); }
function jump(i) { cur = i; render(); }
function render() {
  // toggle .active บน slides
  // update dots (.on), counter (#num), progress bar (#prog)
}
```

### Export PPTX (PptxGenJS)
```javascript
async function exportPPTX() {
  const pptx = new PptxGenJS();
  pptx.layout = 'LAYOUT_WIDE';  // 13.33 x 7.5 in
  // สร้างสไลด์ทีละหน้า ด้วย addText / addShape
  // fontFace ใช้ 'Sarabun' ทุกจุด
  await pptx.writeFile({ fileName: 'JBFarmHUB_PPTX-ระบบจัดการฟาร์ม.pptx' });
}
```

### Export PDF (Print)
```javascript
function exportPDF() {
  document.title = 'JBFarmHUB_PDF-ระบบจัดการฟาร์ม';  // ชื่อไฟล์ PDF
  // แสดงทุกสไลด์พร้อมกัน (add .active ทุกหน้า)
  window.print();  // เปิด Print dialog → Save as PDF
  // คืนค่ากลับหลัง print (title + active slide เดิม)
}
```
- ใช้ `@media print` ใน CSS จัดหน้าให้สไลด์ละ 1 หน้า
- `@page { size: landscape; margin: 0; }` — แนวนอน ไม่มีขอบ
- ซ่อน `.dl-wrap`, `.nav`, `.progress` ตอน print
- ทุกสไลด์ `position: relative`, `page-break-after: always`

---

## วิธีใช้กับโปรเจกต์ใหม่

1. **คัดลอก** `present.html` + โลโก้ + ภาพหน้าปก ไปโปรเจกต์ใหม่
2. **แก้เนื้อหา** ในแต่ละ `<div class="slide">` ตาม pattern ด้านบน
3. **เปลี่ยนสี** ที่ `:root` CSS Variables
4. **เปลี่ยนโลโก้** ที่ `<img class="slide-logo" src="...">`
5. **เปลี่ยนภาพหน้าปก** ที่ `<img class="big-icon" src="...">` (ใช้ไฟล์ PNG/SVG ไม่ใช้ emoji เพื่อไม่แตก)
6. **เปลี่ยนขนาดสไลด์** ที่ `.deck` + `.slide` (width/height) + `scaleDeck()` + `@media print min-height`
7. **แก้ exportPPTX()** ให้ตรงกับเนื้อหาใหม่
8. **แก้ชื่อไฟล์ดาวน์โหลด** ใน `exportPPTX()` (fileName) และ `exportPDF()` (document.title)

### Prompt สำหรับบอก AI สร้างใหม่
```
สร้างหน้าเว็บ HTML ไฟล์เดียว สไตล์ PowerPoint presentation:
- ขนาดสไลด์คงที่ 1920x1080 px (Full HD) อยู่กลางจอ พื้นดำรอบนอก
- โทนขาว-เทา สะอาดตา (white/gray theme)
- ฟอนต์: Sarabun + Inter จาก Google Fonts (คล้าย TH SarabunPSK)
- มี auto-scale พอดีจอ, Navigation (←→ Space, dots, swipe)
- มี progress bar, slide counter, fullscreen (F)
- มี fade-up animation (class="fu") สำหรับ elements
- มี logo บริษัทมุมบนขวาทุกสไลด์ (class="slide-logo", height 110px)
- ภาพหน้าปกใช้ไฟล์ PNG (ไม่ใช้ emoji เพราะขยายแล้วแตก)
- มีปุ่ม Download 2 ปุ่มมุมบนขวา:
  - PPTX: ใช้ PptxGenJS สร้างไฟล์ .pptx จริง (fontFace: 'Sarabun')
  - PDF: ใช้ window.print() + @media print CSS จัดหน้าสไลด์ละหน้า landscape
- เนื้อหา 4 สไลด์:
  1. Cover (ชื่อระบบ + คำอธิบาย + วันที่ + ภาพ PNG ฝั่งขวา clip-path เฉียง)
  2. วัตถุประสงค์และผู้ใช้งาน (2 คอลัมน์: Objectives ซ้าย, User Groups ขวา)
  3. โมดูลหลัก (Grid 4x2 ไอคอนใหญ่ + ชื่อโมดูล)
  4. Timeline แนวนอน จุดต่อจุด สไตล์ขนส่ง (วงกลม 64px + เส้น gradient 5px + legend + tech stack)
```

---

## ไฟล์ในโปรเจกต์

| ไฟล์ | คำอธิบาย |
|------|----------|
| `present.html` | ไฟล์ presentation หลัก |
| `logo-Biccorp.png` | โลโก้บริษัท (แสดงมุมบนขวาทุกสไลด์, 110px) |
| `pig-icon.png` | ภาพหมู 3D หน้าปก (PNG คมชัด ไม่แตกเมื่อขยาย) |
| `index.html` | หน้าเว็บหลักของโปรเจกต์ |
| `README.md` | เอกสารนี้ — โครงสร้างและรูปแบบการสร้าง |

## Dependencies

- **Google Fonts** — Sarabun, Inter (CDN)
- **PptxGenJS 3.12** — สร้างไฟล์ .pptx ฝั่ง client (CDN)
- ไม่มี dependency อื่น (Vanilla HTML/CSS/JS)

## หมายเหตุ

- **อย่าใช้ emoji ขนาดใหญ่ในสไลด์** — emoji เป็น bitmap จะแตกเมื่อขยาย ให้ใช้ไฟล์ PNG/SVG แทน
- **ขนาด 1920x1080** ถูกเลือกเพื่อให้ screenshot/capture ได้ภาพ Full HD คมชัด
- **ฟอนต์ Sarabun** จาก Google Fonts หน้าตาใกล้เคียง TH SarabunPSK มาก ใช้ได้ทุกเครื่องไม่ต้องติดตั้ง

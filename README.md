# HTML Presentation Template (PowerPoint Style)

> เทมเพลตสำหรับสร้างสไลด์นำเสนอจากไฟล์ HTML เดียว แสดงผลเหมือน PowerPoint จริง
> พร้อมปุ่ม Export เป็นไฟล์ .pptx และ .pdf ได้

---

## สรุปรูปแบบ

| รายการ | รายละเอียด |
|--------|------------|
| ไฟล์หลัก | `present.html` — HTML ไฟล์เดียว (self-contained) |
| ขนาดสไลด์ | 1255 x 705 px (ใกล้เคียง 16:9) |
| โทนสี | ขาว-เทา สะอาดตา (สำหรับผู้บริหาร) |
| ฟอนต์ | Noto Sans Thai + Inter (Google Fonts) |
| พื้นหลังนอกสไลด์ | สีดำ (#111111) |
| Navigation | ปุ่มซ้าย/ขวา, Dots, Keyboard (←→ Space Home End), Touch Swipe, F=Fullscreen |
| Export PPTX | PptxGenJS สร้าง .pptx จริงฝั่ง client → `JBFarmHUB_PPTX-ระบบจัดการฟาร์ม.pptx` |
| Export PDF | window.print() → Save as PDF → `JBFarmHUB_PDF-ระบบจัดการฟาร์ม.pdf` |
| Auto-scale | สไลด์ย่อ/ขยายพอดีจออัตโนมัติ |
| โลโก้ | แสดงมุมบนขวาทุกสไลด์ (class `.slide-logo`) |

---

## โครงสร้าง HTML

```
<!DOCTYPE html>
<html lang="th">
<head>
  <!-- Google Fonts: Noto Sans Thai + Inter -->
  <!-- CSS ทั้งหมดอยู่ใน <style> รวม @media print สำหรับ PDF -->
</head>
<body>
  <!-- DECK: กล่องสไลด์ขนาดคงที่ อยู่กลางจอ -->
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
    <span class="num" id="num">1 / 6</span>
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
| `.deck` | กล่องสไลด์ขนาดคงที่ อยู่กลางจอ มี auto-scale |
| `.slide` | สไลด์แต่ละหน้า (absolute ซ้อนกัน, opacity 0/1) |
| `.slide.active` | สไลด์ที่แสดงอยู่ |
| `.fu` | Fade-up animation (ใส่ให้ child elements) |
| `.pad` | Padding layout สำหรับเนื้อหา |
| `.center` | จัดกลางทั้งแนวตั้ง/นอน |
| `.kicker` | ป้ายเล็กๆ มุมบน (เช่น "Slide 2/6") พื้นหลังพอดีข้อความ (`width: fit-content`) |
| `.g2` `.g3` `.g5` | Grid 2/3/1 คอลัมน์ |
| `.card` | การ์ดมีขอบ + hover effect |
| `.card.ca` `.cg` `.co` `.cp` | การ์ดมีเส้นสีด้านบน (accent/green/orange/purple) |
| `.obj` | แถววัตถุประสงค์ (วงกลมตัวเลข + ข้อความ) |
| `.user-row` | แถวผู้ใช้งาน (ไอคอน + ชื่อ + คำอธิบาย) |
| `.benefit` | แถวประโยชน์ (✔ + ข้อความ) |
| `.slide-logo` | โลโก้บริษัทมุมบนขวาทุกสไลด์ (opacity: 1, height: 72px) |
| `.cover-deco` | พื้นที่ตกแต่งฝั่งขวาของหน้าปก (clip-path เฉียง) |
| `.dl-wrap` | กลุ่มปุ่ม Download มุมบนขวาจอ (fixed) |
| `.dl-btn` | ปุ่ม Download (hover น้ำเงิน) |
| `.dl-btn.pdf` | ปุ่ม PDF (hover แดง) |
| `.nav` | แถบ Navigation ด้านล่าง (พื้นเข้ม, blur) |
| `.progress` | Progress bar ด้านบนสไลด์ |

---

## รูปแบบสไลด์ 6 หน้า

### Slide 1: Cover (หน้าปก)
```
┌─────────────────────────────┬──────────────┐
│ [LOGO มุมบนขวา]             │              │
│                              │              │
│  [kicker: การนำเสนอ...]     │   🐷 (ใหญ่)  │
│  [h1: ชื่อระบบ]              │   (พื้นเทา)  │
│  [h2: คำอธิบาย]              │              │
│  [sub: รายละเอียด]           │              │
│  [date-tag: วันที่]          │              │
│                              │              │
└─────────────────────────────┴──────────────┘
```
- ซ้าย 55%: เนื้อหา (`cover-left`) | ขวา 45%: `cover-deco` พื้นเทา + ไอคอนใหญ่ (200px, opacity 0.85)
- ใช้ `clip-path: polygon()` ตัดขอบเฉียง

### Slide 2: วัตถุประสงค์ (5 ข้อ)
```
┌────────────────────────────────────────────┐
│ [kicker] [LOGO]                            │
│ [h2: หัวข้อ]                                │
│                                            │
│  [1] ●──── หัวข้อ ─────────────────────     │
│            คำอธิบาย                         │
│  [2] ●──── หัวข้อ ─────────────────────     │
│            คำอธิบาย                         │
│  ... (5 แถว)                               │
└────────────────────────────────────────────┘
```
- ใช้ class `.obj` — วงกลมตัวเลขสี (.on) + h4 + p
- สีวงกลมต่างกัน: น้ำเงิน, เขียว, ส้ม, แดง, ม่วง

### Slide 3: ผู้ใช้งาน + ประโยชน์ (2 คอลัมน์)
```
┌──────────────────┬─────────────────────────┐
│ กลุ่มผู้ใช้งาน     │ ประโยชน์สำหรับผู้บริหาร    │
│                  │                         │
│ 🛡️ Admin/IT     │ ✔ เข้าถึงข้อมูลทันที     │
│ 👔 ผู้บริหาร      │ ✔ ตัดสินใจแม่นยำ        │
│ 🌾 ผจก.ฟาร์ม    │ ✔ ควบคุมต้นทุน          │
│ 📋 หัวหน้างาน    │ ✔ โปร่งใส               │
│ 👷 พนักงาน      │ ✔ ขยายธุรกิจ            │
│ 👁️ ภายนอก      │                         │
└──────────────────┴─────────────────────────┘
```
- ซ้าย: `.user-row` (ไอคอน `.ua` + h4 + p)
- ขวา: `.card` + `.benefit` (✔ `.ck` + p)
- หัวคอลัมน์ใช้ `.sec-label`

### Slide 4–5: โมดูล (2x2 การ์ด)
```
┌──────────────────┬──────────────────┐
│ 🔐 พื้นฐาน       │ 🛒 จัดซื้อ        │
│ ● item 1        │ ● item 1        │
│ ● item 2        │ ● item 2        │
│ ● item 3        │ ● item 3        │
│ ● item 4        │ ● item 4        │
├──────────────────┼──────────────────┤
│ 📦 คลังสินค้า     │ 🐷 การผลิต       │
│ ● item 1        │ ● item 1        │
│ ...             │ ...             │
└──────────────────┴──────────────────┘
```
- ใช้ `.g2` grid 2 คอลัมน์
- `.card.ca` `.cg` `.co` `.cp` = เส้นสีด้านบนต่างกัน (border-top: 3px)
- แต่ละการ์ด: `.ico` + h3 + ul li (จุดกลม accent ก่อน li ผ่าน ::before)

### Slide 6: Timeline (แนวนอน จุดต่อจุด สไตล์ขนส่ง)
```
┌────────────────────────────────────────────┐
│ [h2: แผนงาน 22 สัปดาห์]                     │
│                                            │
│  🚀──🔐──🛒──📦──🐷──💰──🔧──📊           │
│  Wk1  Wk3  Wk6  Wk9  Wk13 Wk18 Wk20 Wk22 │
│  Kick Auth Purc Ware Prod Sale Maint Fin   │
│  ...  ...  ...  ...  ...  ...  ...   ...   │
│                                            │
│  ● Phase 1  ● Phase 2  ● Phase 3  ● Finish│
│  Tech: Next.js | .NET 10 | PostgreSQL      │
└────────────────────────────────────────────┘
```
- เส้นแนวนอน gradient สี (accent→green→orange→purple)
- 8 จุดวงกลม (42x42px, border ขาว 4px + box-shadow) มีไอคอน emoji
- ใต้จุด: สัปดาห์ (สีตาม phase) + ชื่อ (bold) + รายละเอียด 3 บรรทัด
- Legend: pill สี 4 Phase (วงกลมเล็ก + text)
- Tech Stack: pill เล็กแนวนอน
- จุดสุดท้ายมี 🏁 Go Live

---

## Slide Engine (JavaScript)

### Auto-scale
```javascript
function scaleDeck() {
  const sw = 1255, sh = 705;                // ขนาดสไลด์จริง
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
  // แต่ละสไลด์ recreate layout ใน PPTX format
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

1. **คัดลอก** `present.html` + โลโก้ ไปโปรเจกต์ใหม่
2. **แก้เนื้อหา** ในแต่ละ `<div class="slide">` ตาม pattern ด้านบน
3. **เปลี่ยนสี** ที่ `:root` CSS Variables
4. **เปลี่ยนโลโก้** ที่ `<img class="slide-logo" src="...">`
5. **เปลี่ยนขนาดสไลด์** ที่ `.deck` + `.slide` (width/height) + `scaleDeck()`
6. **แก้ exportPPTX()** ให้ตรงกับเนื้อหาใหม่
7. **แก้ชื่อไฟล์ดาวน์โหลด** ใน `exportPPTX()` (fileName) และ `exportPDF()` (document.title)

### Prompt สำหรับบอก AI สร้างใหม่
```
สร้างหน้าเว็บ HTML ไฟล์เดียว สไตล์ PowerPoint presentation:
- ขนาดสไลด์คงที่ 1255x705 px อยู่กลางจอ พื้นดำรอบนอก
- โทนขาว-เทา สะอาดตา (white/gray theme)
- ฟอนต์: Noto Sans Thai + Inter จาก Google Fonts
- มี auto-scale พอดีจอ, Navigation (←→ Space, dots, swipe)
- มี progress bar, slide counter, fullscreen (F)
- มี fade-up animation (class="fu") สำหรับ elements
- มี logo บริษัทมุมบนขวาทุกสไลด์ (class="slide-logo")
- มีปุ่ม Download 2 ปุ่มมุมบนขวา:
  - PPTX: ใช้ PptxGenJS สร้างไฟล์ .pptx จริง
  - PDF: ใช้ window.print() + @media print CSS จัดหน้าสไลด์ละหน้า landscape
- เนื้อหา 6 สไลด์:
  1. Cover (ชื่อระบบ + คำอธิบาย + วันที่ + ไอคอนใหญ่ฝั่งขวา clip-path เฉียง)
  2. วัตถุประสงค์ (แถวตัวเลขสี 5 ข้อ class="obj")
  3. ผู้ใช้งาน + ประโยชน์ (2 คอลัมน์: user-row ซ้าย, benefit ขวา)
  4. โมดูลส่วน 1 (การ์ด 2x2 มีเส้นสีด้านบน .card.ca/.cg/.co/.cp)
  5. โมดูลส่วน 2 (การ์ด 2x2)
  6. Timeline แนวนอน จุดต่อจุด สไตล์ขนส่ง (วงกลม + เส้น gradient + legend + tech stack)
```

---

## ไฟล์ในโปรเจกต์

| ไฟล์ | คำอธิบาย |
|------|----------|
| `present.html` | ไฟล์ presentation หลัก |
| `logo-Biccorp.png` | โลโก้บริษัท (แสดงมุมบนขวาทุกสไลด์) |
| `README.md` | เอกสารนี้ — โครงสร้างและรูปแบบการสร้าง |

## Dependencies

- **Google Fonts** — Noto Sans Thai, Inter (CDN)
- **PptxGenJS 3.12** — สร้างไฟล์ .pptx ฝั่ง client (CDN)
- ไม่มี dependency อื่น (Vanilla HTML/CSS/JS)

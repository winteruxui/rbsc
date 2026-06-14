# RBSC Racing Website — Prototype Build Spec (สำหรับรีวิวก่อนสร้าง)

> **สถานะ:** ร่างเพื่อรีวิว ยังไม่ build · **ขอบเขต:** Clickable prototype 4 หน้า สำหรับเทสกับ user จริง · **อ้างอิง:** SOW §3.1–3.7 + รายงานวิจัย UX (RBSC_UX_Research_TH.md)
>
> **วิธีอ่านเอกสารนี้:** อ่าน §0–§3 เพื่อเข้าใจภาพรวมและขอบเขต แล้วรีวิว §4 (raรหน้า) ให้ละเอียด ถ้า ✅ ทุกส่วน ค่อยสั่ง build ตาม §9 (Apply guide) ส่วนไหนอยากแก้ คอมเมนต์ที่หัวข้อนั้นได้เลย

---

## 0. เป้าหมายของ Prototype & สิ่งที่จะ "ไม่" ทำ

**เป้าหมาย:** ทดสอบกับ user จริงว่า การออกแบบตามผลวิจัย (dual-audience, mobile-first, ตารางหนาแน่นที่สแกนได้, drill-down) ใช้งานได้จริงไหม — เน้นวัด 5 อย่าง:
1. คนหา "วันแข่งถัดไป + สภาพสนาม" เจอไหม (newbie orientation)
2. คนสลับ **มุมมอง ง่าย/เต็ม** บน Race Card แล้วเข้าใจไหม (dual-audience)
3. คน drill-down จากชื่อม้า → โปรไฟล์ม้า ได้ลื่นไหม (information scent)
4. ตาราง Race Card บนมือถือ อ่าน/สแกนได้ไหม (responsive table)
5. คนเข้าใจ form line + รู้ว่ากดดู tooltip ได้ไหม (progressive disclosure)

**สิ่งที่ prototype จะ "ไม่" ทำ (กันหลุด SOW / กัน over-build):**
- ❌ ไม่มี backend / ฐานข้อมูลจริง — ใช้ mock data ใน JS ล้วน
- ❌ ไม่มี CMS, auth/MFA, SEO, hosting, analytics (เป็นงาน technical scope §4 ของ SOW ไม่ใช่ prototype)
- ❌ ไม่ทำ PDF จริง — ปุ่ม Download เป็น placeholder (alert/console)
- ❌ ไม่ทำครบทุกหน้าใน SOW — ทำเฉพาะ **4 หน้า priority** เท่านั้น (หน้าอื่นเป็นลิงก์ตัน + ป้าย "อยู่นอกขอบเขต prototype")
- ❌ ไม่ฝังวิดีโอจริง — ใช้ thumbnail + modal placeholder
- ❌ ไม่ทำสองภาษาเต็ม — ทำ **toggle TH/EN ที่สลับได้จริงเฉพาะ navigation + หัวข้อหลัก** (เพื่อเทสว่า layout รองรับ) เนื้อหา body เป็นไทยเป็นหลัก *(ดู §7 — ปรับได้ตามต้องการ)*

---

## 1. ขอบเขต 4 หน้า + Mapping กับ SOW

| # | หน้า | SOW อ้างอิง | Purpose (mental state) |
|---|------|-------------|------------------------|
| 1 | **Landing** | §3.4 ข่าว, §3.7 ประสบการณ์ | ปฐมนิเทศ + นำทาง |
| 2 | **Race Card** | §3.2 เรซการ์ด/แฮนดิแคป | เปรียบเทียบม้าในเรซ |
| 3 | **Horse Profile** | §3.3 ฐานข้อมูลม้า | สืบค้นประวัติม้า |
| 4 | **Race Day Detail** | §3.2 ปฏิทิน/ผล + §3.6 สนาม | วางแผน + บริบทวันแข่ง |

**Flow ที่ต้อง clickable (เส้นทางหลักที่จะให้ user ทดสอบ):**
```
Landing ──"แข่งวันนี้"──▶ Race Day Detail ──กดเรซ──▶ Race Card ──กดชื่อม้า──▶ Horse Profile
   │                          ▲                            │                      │
   └──────────────────────────┴────────────────────────────┴──────────────────────┘
          (ทุกหน้ามี nav กลับ + breadcrumb + โลโก้กลับ Landing)
```

---

## 2. Tech Stack & ข้อจำกัด (เพื่อประหยัด token + เทสง่าย)

- **1 ไฟล์ HTML เดียว** (`index.html`) รวม HTML + CSS + JS — ไม่มี build step, เปิดไฟล์เดียวจบ, ส่งให้ user เทสง่าย (แชร์ลิงก์/เปิดมือถือได้ทันที)
- **Vanilla JS** เท่านั้น — ไม่มี framework, ไม่มี npm, ไม่มี CDN ที่ blockได้ (กันเน็ตเวิร์ก fail ตอนเทส)
- **Routing แบบ hash** (`#landing`, `#raceday`, `#racecard`, `#horse`) — SPA ง่ายๆ สลับ section ด้วย JS
- **Mock data** เป็น JS object เดียวด้านบนไฟล์ (แก้ง่าย, reuse ข้ามหน้า)
- **ฟอนต์:** Google Fonts 2 ตัว (ดู §3) — โหลดผ่าน `<link>` มาตรฐาน ถ้าเทสออฟไลน์ fallback เป็น system font
- **ขนาดเป้าหมาย:** ~1 ไฟล์ < 1,500 บรรทัด → token-efficient ตอน vibe code

> **เหตุผล single-file:** ทดสอบ usability ไม่ต้องใช้ backend; แชร์ง่าย; แก้เร็วระหว่างรอบเทส; ประหยัด token ตอน generate มากที่สุด

---

## 3. Design System (Font, Spacing, Color, Motion)

### 3.1 Typography
- **ฟอนต์ไทย/หลัก:** `IBM Plex Sans Thai` (อ่านง่าย ไม่มีหัว ทันสมัย รองรับวรรณยุกต์ดี) — ใช้ทั้ง TH + EN ได้ในตัวเดียว ลด HTTP request
- **ฟอนต์ตัวเลข/ตาราง:** `IBM Plex Mono` เฉพาะ **form line + เวลา + น้ำหนัก + เรตติ้ง** (ตัวเลขเรียงตรงคอลัมน์ สแกนง่าย)
- **Line-height:** body `1.6`, ตาราง `1.5`, heading `1.3` — *(ไทยต้องการ ≥1.55 ตามผลวิจัย WCAG 1.4.13)*
- **Type scale (rem):** h1 `2rem` / h2 `1.5rem` / h3 `1.25rem` / body `1rem` (16px) / small `0.875rem` / table `0.9375rem`
- **น้ำหนัก:** 400 (body), 500 (label/medium), 600 (heading/bold) — เลี่ยง 700 กับไทย (หนักเกิน ทับวรรณยุกต์)

### 3.2 Spacing (4px base grid)
ใช้ token: `--s1:4px --s2:8px --s3:12px --s4:16px --s5:24px --s6:32px --s7:48px --s8:64px`
- ระยะใน card: `--s4` (16px) · ระหว่าง section: `--s7` (48px) · gap ในตาราง cell: `--s3` (12px)
- container max-width: `1200px` desktop · padding ขอบจอมือถือ: `--s4`

### 3.3 Color (โทน RBSC — เขียว/ทอง สื่อความเป็นทางการ + heritage)
```
--green-900:#0F3D2E  (หลัก/heading bar)
--green-700:#1B5E43  (ปุ่มหลัก/ลิงก์)
--green-50 :#E8F2ED  (พื้นอ่อน/hover)
--gold-500 :#C9A227  (accent/feature race/CTA สำคัญ)
--ink-900  :#1A1A1A  (ตัวอักษรหลัก)
--ink-600  :#5C5C5C  (ตัวอักษรรอง)
--line     :#E2E2E2  (เส้นตาราง/ขอบ)
--bg       :#FFFFFF  --bg-alt:#F7F8F7  (zebra row)
--good     :#2E7D32 (ฟอร์มดี/เขียว) --warn:#ED6C02 (กลาง/ส้ม) --bad:#9E9E9E (เทา)
```
> **เน้น contrast ≥4.5:1** (WCAG AA) ทุกคู่ข้อความ/พื้น

### 3.4 Interactive & Motion (พอดี ไม่เวอร์)
- **Transition มาตรฐาน:** `0.18s ease` สำหรับ hover/expand เท่านั้น
- **ไม่มี:** parallax, auto-carousel เด้ง, animation ฟุ่มเฟือย (กันหลุด scope + กันรบกวนการเทส)
- **มี:** hover state ทุกลิงก์/ปุ่ม, active state nav, expand/collapse accordion (form line), tooltip fade-in, view-toggle slide
- **Touch target ≥44×44px** ทุกปุ่มบนมือถือ
- **Focus ring** ชัดเจน (keyboard nav ทดสอบได้)

---

## 4. รายละเอียดรายหน้า (ส่วนที่ต้องรีวิวละเอียดที่สุด)

### 4.1 หน้า LANDING (`#landing`)

**โครงสร้างบนลงล่าง (mobile-first):**

1. **Header (sticky)** — โลโก้ RBSC (กดกลับ landing) · เมนู 6 อัน *(desktop: แนวนอน / mobile: hamburger + คง 3 อันเด่น: Racing, แข่งวันนี้, ค้นหา)* · **toggle TH/EN** · ช่องค้นหา (dropdown ประเภท: ทั้งหมด/ม้า/จ็อกกี้/เทรนเนอร์/เจ้าของ)
   - 6 เมนู: `Racing · Horses & People · Track & Regulations · Experience · News · About` (ตัวที่ไม่อยู่ใน 4 หน้า = ลิงก์ตัน + tooltip "นอกขอบเขต prototype")
2. **Hero** — ภาพแข่งม้า (placeholder gradient + ภาพ stock 1 รูป) · overlay: ชื่อ "สนามม้า RBSC" + ประโยค value + **CTA หลัก "ดูการแข่งวันนี้"** (Z-pattern: ข้อความซ้ายบน, CTA ขวา/ล่าง)
3. **โมดูล "แข่งวันนี้ / วันแข่งถัดไป"** ⭐ *(สำคัญสุด — โมเดล STC Fixtures)*
   - การ์ดหัว: สนาม + วันที่ + **สภาพอากาศ (ไอคอน+°C)** + **going** + จำนวนเรซ + เวลาออกตัวแรก
   - แถบ **chip เลขเรซ 1–8** เลื่อนแนวนอน — กดแล้วไป Race Card เรซนั้น
   - ปุ่ม "ดู Race Day แบบเต็ม" → Race Day Detail · ปุ่ม "ดาวน์โหลด PDF" (placeholder)
   - *กรณีไม่ใช่วันแข่ง:* การ์ดนับถอยหลัง "วันแข่งถัดไป: อา. 21 มิ.ย." → Race Day Detail
4. **2 การ์ดแยกกลุ่ม** *(dual-audience ให้เห็นชัด)*
   - 🟢 **"มือใหม่หัดดู?"** → วิธีอ่าน Race Card, อภิธานศัพท์, ข้อมูลบัตร (ลิงก์ตัน/ป้าย demo)
   - 🔵 **"ฟอร์ม & ข้อมูล"** → Race Cards, ผลแข่ง, เรตติ้ง, Leaderboard
5. **ข่าวล่าสุด** — 3–4 การ์ด (thumbnail + พาดหัว + วันที่) หมวด: Race Preview / Selection / Early Scratchings *(แบบ STC)*
6. **Footer** — quick links + heritage line "ก่อตั้ง 1901 · แข่งม้าตั้งแต่ 1903"

**Interactive ที่ต้องมี:** hamburger toggle · TH/EN สลับ nav จริง · chip เลขเรซกดได้ · ทุก CTA นำทางจริง · search box เปิด dropdown (mock result 2-3 รายการ)

---

### 4.2 หน้า RACE CARD (`#racecard`) — หนาแน่นสุด ต้องรีวิวเข้ม

**โครงสร้าง:**

1. **Breadcrumb:** ปฏิทิน › วันแข่ง 14 มิ.ย. › เรซ 3
2. **หัวเรซ (fixed order):** เลขเรซ + ชื่อ → วันที่/สนาม/เวลาออกตัว → พื้น/ระยะ/**going** → เงินรางวัล/คลาส/จำนวนม้า · ปุ่ม **Download PDF (เรซนี้/ทุกเรซ)** · ลิงก์ขึ้น Race Day
3. **แท็บเลขเรซ** (1–8) เลื่อนแนวนอน — สลับเรซได้ในหน้าเดียว *(โมเดล HKJC)*
4. **⭐ View Toggle "ง่าย ⇄ เต็ม"** (ตัวควบคุม dual-audience หลัก — จุดเทสสำคัญ)

   **มุมมอง "ง่าย" (default):** การ์ดต่อม้า เรียงซ้อน — แต่ละการ์ดมี:
   - เบอร์ + silks (สี่เหลี่ยมสี) + **ชื่อม้า (ลิงก์ใหญ่)** + จ็อกกี้ + เทรนเนอร์ + น้ำหนัก + ตำแหน่งสตาร์ท
   - **แถบสรุปฟอร์มใช้สี** (traffic-light: เขียว/ส้ม/เทา) + ป้ายภาษาคน "ฟอร์มดี"/"กำลังพัฒนา"/"ห่างหาย"
   - tooltip บนทุก label (กด ? หรือ long-press)

   **มุมมอง "เต็ม":** ตารางแน่น — คอลัมน์ซ้าย→ขวา:
   ```
   [เบอร์][silks][ม้า(ลิงก์)] | ฟอร์ม6ครั้ง | เทรนเนอร์ | จ็อกกี้ | น้ำหนัก | สตาร์ท | เรตติ้ง | +/- | วันห่าง | gear
   ```
   - **sticky header** + **ตรึง 3 คอลัมน์ซ้าย** (เบอร์+silks+ม้า) + เลื่อนแนวนอนส่วนที่เหลือ
   - **เน้นสี** เรตติ้งสูงสุด + ฟอร์มดีสุดในเรซ *(conditional formatting)*
   - **กดหัวคอลัมน์ = sort** (เบอร์/เรตติ้ง/น้ำหนัก)
   - ฟอร์ม = monospace, ล่าสุดขวา, กดดู tooltip อธิบาย "5=ที่5, 0=ที่10+"
5. **กล่องแฮนดิแคป:** แถบเรตติ้ง + tooltip "แฮนดิแคปคืออะไร" *(SOW §3.2)*
6. **Legend** (รหัส gear: B/TT/SR…) — collapse ได้ ปุ่ม "ดูคำอธิบายสัญลักษณ์"

**Interactive ที่ต้องมี:** view toggle สลับจริง · sort คอลัมน์ · แท็บเรซสลับ · ตรึงคอลัมน์ + scroll แนวนอนบนมือถือ · ชื่อม้า/จ็อกกี้/เทรนเนอร์ ทุกตัวกดได้ · tooltip form line · legend collapse

> **จุดเทสวิกฤต:** มุมมองเต็มบนจอมือถือ — ตารางตรึงคอลัมน์ + scroll ต้องไม่พัง, ต้องเห็นว่ามีคอลัมน์ต่อ (peek)

---

### 4.3 หน้า HORSE PROFILE (`#horse`)

**โครงสร้าง (progressive disclosure ≤2 ระดับ):**

1. **Breadcrumb** + ปุ่มกลับ
2. **Hero/identity:** รูปม้า (placeholder) + ชื่อ TH/EN + แถวข้อมูลพาดหัวเห็นตลอด: อายุ · เพศ · สี · เทรนเนอร์(ลิงก์) · เจ้าของ(ลิงก์) · **เรตติ้งปัจจุบัน (เด่น)**
3. **แผงข้อมูลพื้นฐาน** (key–value 2 คอลัมน์, มือถือเรียงซ้อน): พ่อพันธุ์ · แม่พันธุ์ · วันเกิด · ผู้เพาะพันธุ์ · คอก · ประเภท (Thoroughbred/New TB/Country-Bred ตาม SOW §3.3)
4. **ฟอร์ม/ผลงานที่ผ่านมา** ⭐ — ตารางย้อนหลัง: วันที่(ลิงก์→Race Day) · สนาม · ระยะ · going · คลาส · อันดับ · น้ำหนัก · จ็อกกี้ · เรตติ้ง · ความเห็นกรรมการ(1บรรทัด)
   - **default 6 แถว** + ปุ่ม "ดูทั้งหมด" · แต่ละแถว **กดขยาย** เผยความเห็นเต็ม + ปุ่ม **▶ วิดีโอย้อนหลัง** (modal placeholder)
5. **กราฟเรตติ้ง** — เส้นง่ายๆ (SVG/canvas mock) + คำบรรยาย "สูง=ดีกว่า, เส้นขึ้น=พัฒนา"
6. **ความเห็นกรรมการ** — list collapse (default ปิด)
7. **entity เกี่ยวข้อง:** ลิงก์ เทรนเนอร์/เจ้าของ/จ็อกกี้ประจำ/ม้าคอกเดียวกัน (ลิงก์ตัน + ป้าย demo)

**Interactive ที่ต้องมี:** แถวฟอร์มขยาย/ยุบ · "ดูทั้งหมด" · modal วิดีโอ · วันที่ในฟอร์มกดกลับ Race Day · กราฟ hover แสดงค่า

---

### 4.4 หน้า RACE DAY DETAIL (`#raceday`) — หน้าใหม่ตามที่ขอ

**โครงสร้าง:**

1. **Breadcrumb:** ปฏิทิน › วันแข่ง 14 มิ.ย. 2026
2. **หัว Meeting:** สนาม + วันที่เต็ม + ชื่อ feature race + สถานะ (กำลังจะถึง/สด/จบแล้ว — chip สี)
3. **⭐ แผงสภาพ (เหตุผลหลักของหน้านี้ — SOW §3.6):**
   - **สภาพอากาศ** (ไอคอน + °C + คำ เช่น "แดดจัด 33°C")
   - **going/สภาพสนาม** (เช่น "Good · สนามแห้ง")
   - พื้นผิว · อุณหภูมิ · อัปเดตล่าสุด (timestamp)
   - แสดงเป็น 3-4 การ์ดไอคอนเรียงแถว (มือถือ = 2×2 grid)
4. **ภาพรวมทั้งวัน:** จำนวนเรซ + เวลาออกตัวแรก-สุดท้าย + **list เรซ** *(โมเดล STC)* — แต่ละแถว: เวลา · ชื่อเรซ · คลาส/ระยะ · จำนวนม้า · ปุ่ม "ดู Race Card" (กำลังจะถึง) / "ดูผล" (จบแล้ว)
5. **ม้าที่ลงแข่งวันนี้:** list ย่อ A–Z หรือตามเรซ + ลิงก์ไปโปรไฟล์ม้า
6. **สื่อ & ประสบการณ์:** ปุ่ม live stream (placeholder) · บล็อก "วางแผนมาชม" (บัตร 100฿, เวลา, การแต่งกาย — SOW §3.7) · ปุ่ม "ซื้อบัตร" (redirect placeholder)
7. *(ถ้าจบแล้ว)* สรุปผล + ลิงก์รายงานกรรมการ

**Interactive ที่ต้องมี:** list เรซกดไป Race Card · ม้ากดไปโปรไฟล์ · chip สถานะ · ปุ่มบัตร/stream (placeholder alert)

---

## 5. Mock Data ที่ต้องเตรียม (ชุดเล็ก reuse ข้ามหน้า)

เพื่อประหยัด token — ใช้ **1 วันแข่ง · 8 เรซ · เรซตัวอย่างละเอียด 1 เรซ (เรซ 3, 10 ม้า) · ม้าโปรไฟล์ละเอียด 1 ตัว**
```js
DATA = {
  meeting: { date, track, weather, going, status, races:[8] },
  raceDetail: { no:3, name, dist, class, going, prize, runners:[10 ม้า: เบอร์,silks,ชื่อTH/EN,จ็อกกี้,เทรนเนอร์,น้ำหนัก,สตาร์ท,เรตติ้ง,ฟอร์ม"x6",gear,formTag] },
  horse: { ชื่อ,อายุ,เพศ,สี,sire,dam,เกิด,breeder,คอก,เทรนเนอร์,เจ้าของ,เรตติ้ง, form:[6 แถว], ratingHistory:[8 จุด], stewards:[] },
  news: [3], searchMock:[3]
}
```
> ม้าตัวอื่นๆ ในตารางใช้ข้อมูลซ้ำ/สุ่มเล็กน้อย — ละเอียดจริงแค่เท่าที่จำเป็นต่อการเทส

---

## 6. Responsive Breakpoints

| ช่วง | layout |
|------|--------|
| **< 600px** (มือถือ) | nav→hamburger · Race Card "ง่าย"=การ์ด, "เต็ม"=ตารางตรึงคอลัมน์+scroll · แผงสภาพ 2×2 · key-value เรียงซ้อน |
| **600–1024px** (แท็บเล็ต) | nav แนวนอนย่อ · ตารางเต็มเริ่มเห็นหลายคอลัมน์ · grid 2 คอลัมน์ |
| **> 1024px** (PC) | container 1200px · ตารางเต็มทุกคอลัมน์ · grid 3 คอลัมน์ |

ทดสอบหลัก = **มือถือ** (ผลวิจัยชี้ traffic ไทย mobile-first)

---

## 7. ❓ จุดที่อยากให้ยืนยันก่อน build (Decision points)

> ตอบ/แก้ตรงนี้ก่อนสั่ง build จะได้ไม่ต้องรื้อ

1. **ภาษา:** ทำ toggle TH/EN แบบสลับ nav+heading จริง (body ไทย) — พอไหม? หรืออยากให้สลับครบทุกคำ? *(ครบทุกคำ = token เพิ่ม ~30%)*
2. **โทนสี:** เขียว-ทอง (heritage RBSC) โอเคไหม หรือมี brand color/โลโก้จริงให้ใช้?
3. **default view ของ Race Card:** เปิดมาเป็น "ง่าย" (เน้น newbie) หรือ "เต็ม" (เน้น insider)? — ผมเสนอ "ง่าย" เพื่อให้เทสได้ว่า newbie เข้าใจ แล้ว insider กดสลับเอง
4. **จำนวนเรซ:** 8 เรซพอไหม (RBSC จริง ~10) — ตั้ง 8 เพื่อประหยัด แต่ปรับเป็น 10 ได้
5. **รูปภาพ:** ใช้ gradient placeholder ล้วน (เทสง่าย ไม่พึ่งเน็ต) หรือใส่ภาพ stock จาก URL? *(stock = สวยกว่าแต่เสี่ยง offline fail)*
6. **ขอบเขตหน้า:** ยืนยัน 4 หน้าเท่านั้น — Leaderboard/Results/People profiles เป็นลิงก์ตัน ใช่ไหม?

---

## 8. Acceptance Checklist (เกณฑ์ "เสร็จ" ของ prototype)

- [ ] เปิดไฟล์เดียวรันได้ ไม่ต้องติดตั้งอะไร
- [ ] 4 หน้าครบ + flow Landing→RaceDay→RaceCard→Horse คลิกทะลุได้
- [ ] Race Card สลับ ง่าย/เต็ม ได้จริง + sort + ตรึงคอลัมน์บนมือถือ
- [ ] ชื่อม้า/จ็อกกี้/เทรนเนอร์/วันที่ กดได้ทุกจุดที่ระบุ
- [ ] tooltip form line + แฮนดิแคป ทำงาน
- [ ] แผงสภาพอากาศ/going เด่นชัดในหน้า Race Day
- [ ] responsive ผ่านทั้ง 3 breakpoint (เทสที่ 375px, 768px, 1280px)
- [ ] toggle TH/EN สลับ nav ได้
- [ ] touch target ≥44px, focus ring มี, contrast ผ่าน AA
- [ ] ปุ่ม placeholder (PDF/วิดีโอ/บัตร) มี feedback ชัด (alert/modal) ไม่ใช่กดแล้วเงียบ

---

## 9. Apply Guide — วิธีนำไป Vibe Code (ละเอียด ทีละขั้น ประหยัด token)

### ขั้น A — เตรียม (ครั้งเดียว)
1. รีวิวเอกสารนี้ ตอบ §7 ให้ครบ
2. เปิดแชตใหม่/เครื่องมือ vibe code (Claude, Cursor, v0 ฯลฯ)

### ขั้น B — Prompt ที่ใช้ (คัดลอกทั้งบล็อก แล้วแนบไฟล์ spec นี้)
```
สร้าง interactive prototype เว็บแข่งม้า RBSC เป็นไฟล์ index.html เดียว
(HTML+CSS+JS รวมกัน, vanilla JS, ไม่มี framework/CDN, hash routing).

ยึดตาม build spec ที่แนบมาทุกข้อ โดยเฉพาะ:
- Design system §3 (ฟอนต์ IBM Plex Sans Thai + Mono, spacing 4px grid, สีเขียว-ทอง, motion 0.18s)
- 4 หน้า §4 (Landing, Race Card, Horse Profile, Race Day) ครบทุก section + interactive ที่ระบุ
- Mock data §5 (1 วันแข่ง 8 เรซ, เรซ 3 ละเอียด 10 ม้า, ม้าโปรไฟล์ 1 ตัว)
- Responsive §6, Acceptance §8

ข้อกำหนดสำคัญ:
- Race Card ต้องมี view toggle "ง่าย/เต็ม" + sticky header + ตรึง 3 คอลัมน์ซ้าย + scroll แนวนอนบนมือถือ
- ทุกชื่อม้า/จ็อกกี้/เทรนเนอร์/วันที่ คลิก drill-down ได้
- tooltip อธิบาย form line + แฮนดิแคป
- ปุ่ม PDF/วิดีโอ/บัตร = placeholder มี feedback (alert/modal)
- mobile-first, touch target ≥44px, contrast AA

[การตัดสินใจจาก §7]: ภาษา=... / default view=... / เรซ=8 / รูป=gradient placeholder
ทำให้กระชับ ประหยัด token เขียน comment เฉพาะที่จำเป็น
```

### ขั้น C — ถ้าไฟล์ยาวเกิน/ตัดกลางคัน
- สั่งให้ทำ **ทีละหน้า**: "ทำเฉพาะ §4.1 Landing ให้เสร็จก่อน แล้วรอคำสั่งทำหน้าต่อไป" → ต่อ section ทีหลัง ประหยัด context
- หรือสั่ง "เขียน CSS + mock data + routing shell ก่อน แล้วค่อยเติม 4 หน้า"

### ขั้น D — รอบแก้ (iterate ประหยัด token)
- อย่าสั่ง regenerate ทั้งไฟล์ — สั่งแก้เฉพาะจุด: "แก้เฉพาะฟังก์ชัน renderRaceCard ให้ตรึงคอลัมน์เพิ่มเป็น 3" 
- เก็บ spec นี้ไว้อ้างอิง: "ตาม spec §4.2 ข้อ 4" แทนพิมพ์ใหม่

### ขั้น E — เทสกับ user
1. เปิดบนมือถือจริง (ส่งไฟล์/host ชั่วคราว เช่น GitHub Pages, Netlify drop)
2. ให้ 5 task ตาม §0 (หาวันแข่งถัดไป, สลับมุมมอง, ดูโปรไฟล์ม้า ฯลฯ)
3. สังเกต + จด: จุดที่ลังเล, กดผิด, หาไม่เจอ → กลับมาแก้ spec → build รอบใหม่

---

## 10. สรุปสั้น — ทำไม spec นี้ไม่หลุด SOW & ประหยัด token
- **ไม่หลุด SOW:** ทำเฉพาะ frontend 4 หน้าใน §3.1–3.7; ตัด backend/CMS/security/hosting (§4) ออกชัดเจน; ทุก feature map กับข้อ SOW
- **ประหยัด token:** ไฟล์เดียว, vanilla, mock data ชุดเล็ก reuse, สั่งทีละหน้าได้, iterate เฉพาะจุด, อ้าง spec ด้วยเลขข้อแทนพิมพ์ซ้ำ
- **เทสได้จริง:** เปิดไฟล์เดียวบนมือถือ, 5 task ชัด, flow คลิกทะลุครบ

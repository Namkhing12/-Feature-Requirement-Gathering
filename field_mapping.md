# n8n Webhook Field Mapping Reference

This document maps all form inputs to the JSON payload structure sent to your n8n Webhook URL: `https://n8n.arterypartner.dev/webhook/line-router`

---

## 📦 JSON Payload Structure

When a user submits the form, n8n receives a POST request with the following JSON format:

```json
{
  "answers": {
    "line_user_id": "U1234567890abcdef...",
    "displayName": "Line User Name",
    "full_name": "Somchai Jaidee",
    "company": "Artery Partner",
    "email": "somchai@example.com",
    "project_name": "ERPNext Development",

    "q1_subject": "...",
    "q1_attachment": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "q1_attachment_name": "dashboard_mockup.png",
    "q1_link": "https://www.figma.com/file/mockup123",
    "q2_doctypes": "...",
    "q3_asis": "...",
    "q4_tobe": "...",
    "q5_conditions": "...",

    "q_has_workflow": "yes",
    "q6_workflow": "...",
    "q7_permissions": "...",
    "q8_approval": "...",
    "q9_notifications": "...",

    "q10_has_condition": "yes",
    "q10_integration": "...",
    "q11_has_condition": "no",
    "q11_legacy_data": "",

    "q12_has_condition": "yes",
    "q12_master_data": "...",
    "q13_has_condition": "no",
    "q13_print_format": "",
    "q14_has_condition": "no",
    "q14_reports": "",
    "q15_has_condition": "yes",
    "q15_new_fields": "...",
    "q16_has_condition": "no",
    "q16_attachments": "",
    "q17_has_condition": "no",
    "q17_exceptions": "",

    "q18_expected_result": "..."
  },
  "line_user_id": "U1234567890abcdef...",
  "displayName": "Line User Name",
  "submitted_at": "2026-08-29T11:30:00.000Z"
}
```

---

## 📋 5-Session Field Mapping Table

### 🔹 Session 1: ข้อมูลพื้นฐานทั่วไป (Mandatory)
| ข้อที่ | หัวข้อคำถาม | JSON Key | ประเภทข้อมูล | เงื่อนไข |
|:---:|---|---|:---:|:---:|
| - | **ชื่อผู้แจ้ง (Full Name)** | `full_name` | Read-Only ERPNext Profile | Mandatory |
| - | **บริษัท (Company)** | `company` | Read-Only ERPNext Profile | Mandatory |
| - | **ผู้แจ้งเรื่อง (Email)** | `email` | Read-Only ERPNext Profile | Mandatory |
| - | **โครงการ/ระบบ (Project Name)** | `project_name` | Read-Only ERPNext Profile | Mandatory |
| 1 | ต้องการขอฟังก์ชันอะไร? | `q1_subject` | Textarea | Mandatory |
| 2 | Doctype / เอกสารไหนเกี่ยวข้องบ้าง? | `q2_doctypes` | Input (Text) | Mandatory |
| 3 | As-Is ปัจจุบันทำงานอย่างไร? | `q3_asis` | Textarea | Mandatory |
| 4 | To-Be ต้องการให้ทำงานอย่างไร? | `q4_tobe` | Textarea | Mandatory |
| 5 | เงื่อนไขในการทำงานมีอะไรบ้าง? | `q5_conditions` | Input (Text) | Mandatory |

---

### 🔹 Session 2: Workflow และสิทธิ์การใช้งาน (Conditional Block)
*สวิตช์หลัก: `"มีการเปลี่ยนแปลง หรือปรับปรุง Workflow / สถานะเอกสารหรือไม่?"` (`q_has_workflow`: `yes` / `no`)*
*หากเลือก `yes` ฟิลด์ข้อ 6-9 จะแสดงผลและบังคับกรอก (Mandatory) หากเลือก `no` ฟิลด์ข้อ 6-9 จะถูกซ่อนและยกเว้นการกรอก*

| ข้อที่ | หัวข้อคำถาม | JSON Key | ประเภทข้อมูล | เงื่อนไข |
|:---:|---|---|:---:|:---:|
| Switch | มีการเปลี่ยนแปลง หรือปรับปรุง Workflow หรือไม่? | `q_has_workflow` | Hidden Toggle (`yes`/`no`) | Mandatory Switch |
| 6 | Workflow / สถานะเอกสารเป็นอย่างไร? | `q6_workflow` | Textarea | Mandatory (เมื่อ `q_has_workflow` = yes) |
| 7 | ใครมีสิทธิ์ทำอะไรบ้าง? | `q7_permissions` | Input (Text) | Mandatory (เมื่อ `q_has_workflow` = yes) |
| 8 | ต้องมี Approval / การอนุมัติหรือไม่? | `q8_approval` | Input (Text) | Mandatory (เมื่อ `q_has_workflow` = yes) |
| 9 | ต้องมี Notification / การแจ้งเตือนหรือไม่? | `q9_notifications` | Input (Text) | Mandatory (เมื่อ `q_has_workflow` = yes) |

---

### 🔹 Session 3: การเชื่อมต่อระบบและข้อมูลเดิม (Conditional per item)
| ข้อที่ | หัวข้อคำถาม | JSON Key (Toggle / Detail) | ประเภทข้อมูล | เงื่อนไข |
|:---:|---|---|:---:|:---:|
| 10 | ต้องเชื่อมต่อกับระบบอื่นหรือไม่? | `q10_has_condition` / `q10_integration` | Toggle (`yes`/`no`) + Input | Mandatory Detail (เมื่อตอบ ใช่) |
| 11 | ข้อมูลเดิมต้องนำมาใช้ด้วยหรือไม่? | `q11_has_condition` / `q11_legacy_data` | Toggle (`yes`/`no`) + Textarea | Mandatory Detail (เมื่อตอบ ใช่) |

---

### 🔹 Session 4: การปรับปรุงข้อมูลหลักและหน้าเอกสารอื่น ๆ (Conditional per item)
| ข้อที่ | หัวข้อคำถาม | JSON Key (Toggle / Detail) | ประเภทข้อมูล | เงื่อนไข |
|:---:|---|---|:---:|:---:|
| 12 | ต้องมีการปรับปรุงข้อมูลหลัก (Master Data) หรือไม่? | `q12_has_condition` / `q12_master_data` | Toggle (`yes`/`no`) + Input | Mandatory Detail (เมื่อตอบ ใช่) |
| 13 | ต้องเพิ่ม / แก้ไข Print Format หรือไม่? | `q13_has_condition` / `q13_print_format` | Toggle (`yes`/`no`) + Input | Mandatory Detail (เมื่อตอบ ใช่) |
| 14 | ต้องเพิ่ม / แก้ไข Report หรือไม่? | `q14_has_condition` / `q14_reports` | Toggle (`yes`/`no`) + Input | Mandatory Detail (เมื่อตอบ ใช่) |
| 15 | มีข้อมูล / Field ใหม่ที่ต้องเพิ่มหรือไม่? | `q15_has_condition` / `q15_new_fields` | Toggle (`yes`/`no`) + Textarea | Optional Detail (เลือกตอบตามสมัครใจ) |
| 16 | ต้องมีการแนบไฟล์หรือเอกสารหรือไม่? | `q16_has_condition` / `q16_attachments` | Toggle (`yes`/`no`) + Input | Mandatory Detail (เมื่อตอบ ใช่) |
| 17 | มีกรณีพิเศษ / Possible Case อะไรบ้าง? | `q17_has_condition` / `q17_exceptions` | Toggle (`yes`/`no`) + Textarea | Mandatory Detail (เมื่อตอบ ใช่) |

---

### 🔹 Session 5: ผลลัพธ์ที่คาดหวัง (Mandatory)
| ข้อที่ | หัวข้อคำถาม | JSON Key | ประเภทข้อมูล | เงื่อนไข |
|:---:|---|---|:---:|:---:|
| 18 | Expected Result คืออะไร? | `q18_expected_result` | Textarea | Mandatory |
| - | **เวลาที่บันทึกข้อมูล** | `submitted_at` | Timestamp (ISO) | Mandatory |

---

### 📎 ข้อมูลแนบเพิ่มเติม (Question Multi-File & Link Attachments)

สำหรับทุกคำถามข้อที่ `N` (ตั้งแต่ 1 ถึง 18) ระบบรองรับการแนบไฟล์ได้ **หลายไฟล์** (Multi-file uploads) โดยผู้ใช้งานสามารถเลือกไฟล์หรือกดแนบไฟล์เพิ่มได้ตามต้องการ (จำกัดขนาดไม่เกิน 10MB ต่อไฟล์):

**ประเภทไฟล์ที่รองรับ (Supported Extensions):**
- **เอกสาร PDF**: `.pdf`
- **เอกสาร Microsoft Word**: `.doc`, `.docx`
- **เอกสาร Microsoft Excel**: `.xls`, `.xlsx`
- **เอกสาร Microsoft PowerPoint**: `.ppt`, `.pptx`
- **รูปภาพ**: `.png`, `.jpg`, `.jpeg`, `.gif`, `.webp`
- **ข้อความ / ข้ออมูล**: `.txt`, `.csv`
- **ไฟล์บีบอัด Archive**: `.zip`, `.rar`

| JSON Key | คำอธิบาย (Description) | ประเภทข้อมูล (Format) | สถานะฟิลด์ |
|---|---|---|---|
| `qN_attachments` | อาร์เรย์ของไฟล์แนบทั้งหมดในข้อที่ `N` พร้อมรายละเอียดชื่อไฟล์ ขนาด ประเภท และ Base64 Data URL | Array `[ { "name": "...", "type": "...", "size": 1234, "data": "data:..." } ]` | **Optional** |
| `qN_attachment_count` | จำนวนไฟล์ที่ถูกแนบในข้อที่ `N` | Number (เช่น `3`) | **Optional** |
| `qN_attachment_names` | รายชื่อไฟล์ที่แนบทั้งหมดคั่นด้วยเครื่องหมายจุลภาค | String (เช่น `mockup1.png, requirement.pdf`) | **Optional** |
| `qN_attachment` | ข้อมูลไฟล์แนบไฟล์แรกของข้อที่ `N` (เพื่อความยืดหยุ่นกับระบบเดิม) | String (`data:*/*;base64,...`) | **Optional** |
| `qN_attachment_name` | ชื่อไฟล์แนบไฟล์แรกของข้อที่ `N` | String (เช่น `mockup1.png`) | **Optional** |
| `qN_links` | อาร์เรย์ของลิงก์แนบภายนอกทั้งหมดในข้อที่ `N` (เช่น Google Drive, Figma) | Array `[ "https://...", "https://..." ]` | **Optional** |
| `qN_link_count` | จำนวนลิงก์ที่ถูกแนบในข้อที่ `N` | Number (เช่น `2`) | **Optional** |
| `qN_links_text` | รายการลิงก์ทั้งหมดคั่นด้วยการขึ้นบรรทัดใหม่ | String | **Optional** |
| `qN_link` | รวมลิงก์ที่แนบในข้อที่ `N` คั่นด้วยเครื่องหมายจุลภาค (เพื่อความยืดหยุ่นกับระบบเดิม) | String (URL หรือ Comma-separated URLs) | **Optional** |





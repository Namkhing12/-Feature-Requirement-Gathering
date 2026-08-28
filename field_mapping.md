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
    "q1_subject": "...",
    "q1_attachment": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "q1_attachment_name": "dashboard_mockup.png",
    "q1_link": "https://www.figma.com/file/mockup123",
    "q2_doctypes": "...",
    "q3_asis": "...",
    "q4_tobe": "...",
    "q5_conditions": "...",
    "q6_workflow": "...",
    "q7_permissions": "...",
    "q8_approval": "...",
    "q9_notifications": "...",
    "q10_integration": "...",
    "q11_legacy_data": "ใช้เฉพาะข้อมูลใหม่",
    "q12_master_data": "...",
    "q13_print_format": "...",
    "q14_reports": "...",
    "q15_new_fields": "...",
    "q16_attachments": "...",
    "q17_exceptions": "...",
    "q18_expected_result": "..."
  },
  "line_user_id": "U1234567890abcdef...",
  "displayName": "Line User Name",
  "submitted_at": "2026-08-24T07:48:00.000Z"
}
```

---

## 📋 Field Mapping Table

| ข้อที่ | หัวข้อคำถาม (Question Name) | JSON Key | ประเภทข้อมูล (Input Type) | สถานะฟิลด์ |
|:---:|---|---|:---:|:---:|
| - | **LINE User ID** | `line_user_id` | Hidden Input | **Required** |
| - | **ชื่อบัญชี LINE** | `displayName` | Hidden Input | **Required** |
| 1 | ชื่อผู้แจ้ง (Full Name) | `full_name` | Input (Text) | **Required** |
| 2 | บริษัท (Company) | `company` | Input (Text) | **Required** |
| 3 | ผู้แจ้งเรื่อง (Email) | `email` | Input (Email) | **Required** |
| 4 | โครงการ/ระบบ (Project Name) | `project_name` | Input (Text) | **Required** |
| 5 | ต้องการขอฟังก์ชันอะไร? | `q1_subject` | Textarea | **Required** |
| 6 | Doctype / เอกสารไหนเกี่ยวข้องบ้าง? | `q2_doctypes` | Input (Text) | **Required** |
| 7 | As-Is ปัจจุบันทำงานอย่างไร? | `q3_asis` | Textarea | **Required** |
| 8 | To-Be ต้องการให้ทำงานอย่างไร? | `q4_tobe` | Textarea | **Required** |
| 9 | เงื่อนไขในการทำงานมีอะไรบ้าง? | `q5_conditions` | Input (Text) | **Required** |
| 10 | Workflow / สถานะเอกสารเป็นอย่างไร? | `q6_workflow` | Textarea | **Required** |
| 11 | ใครมีสิทธิ์ทำอะไรบ้าง? | `q7_permissions` | Input (Text) | **Required** |
| 12 | ต้องมี Approval / การอนุมัติหรือไม่? | `q8_approval` | Input (Text) | **Required** |
| 13 | ต้องมี Notification / การแจ้งเตือนหรือไม่? | `q9_notifications` | Input (Text) | **Required** |
| 14 | ต้องเชื่อมต่อกับระบบอื่นหรือไม่? | `q10_integration` | Input (Text) | **Required** |
| 15 | ข้อมูลเดิมต้องนำมาใช้ด้วยหรือไม่? | `q11_legacy_data` | Custom Radio Cards | **Required** |
| 16 | ต้องมีการปรับปรุงข้อมูลหลัก (Master Data) หรือไม่? | `q12_master_data` | Input (Text) | **Required** |
| 17 | ต้องเพิ่ม / แก้ไข Print Format หรือไม่? | `q13_print_format` | Input (Text) | **Required** |
| 18 | ต้องเพิ่ม / แก้ไข Report หรือไม่? | `q14_reports` | Input (Text) | **Required** |
| 19 | มีข้อมูล / Field ใหม่ที่ต้องเพิ่มหรือไม่? | `q15_new_fields` | Textarea | **Required** |
| 20 | ต้องมีการแนบไฟล์หรือเอกสารหรือไม่? | `q16_attachments` | Input (Text) | **Optional** |
| 21 | มีกรณีพิเศษ / Possible Case อะไรบ้าง? | `q17_exceptions` | Textarea | **Required** |
| 22 | Expected Result คืออะไร? | `q18_expected_result` | Textarea | **Required** |
| - | **เวลาที่บันทึกข้อมูล** | `submitted_at` | Timestamp (ISO) | **Required** |

### 📎 ข้อมูลแนบเพิ่มเติม (Question File & Link Attachments)

สำหรับคำถามที่มีรหัส `qN` (ตั้งแต่ q1 ถึง q18 ซึ่งสอดคล้องกับหัวข้อข้อที่ 5 ถึง 22 ในหน้าจอ) ระบบจะทำการส่งฟิลด์แนบไฟล์ หรือแนบลิงก์เพิ่มเติมแบบอัตโนมัติหากผู้ใช้งานกรอกเข้ามา:

| JSON Key | คำอธิบาย (Description) | ประเภทข้อมูล (Format) | สถานะฟิลด์ |
|---|---|---|---|
| `qN_attachment` | ข้อมูลไฟล์แนบของข้อรหัส `qN` ในรูปแบบ Base64 Data URL | String (`data:*/*;base64,...`) | **Optional** |
| `qN_attachment_name` | ชื่อไฟล์แนบดั้งเดิมพร้อมนามสกุลของข้อรหัส `qN` | String (เช่น `mockup.png`) | **Optional** |
| `qN_link` | ลิงก์แนบภายนอกของข้อรหัส `qN` (เช่น Google Drive, Figma) | String (URL) | **Optional** |



# เอกสารสรุปโครงการพัฒนาระบบจัดการข้อมูลผู้รับบริการ (Diamond Friend)

เอกสารฉบับนี้รวบรวมข้อมูลการวิเคราะห์ระบบ ข้อกำหนดทางเทคนิค การออกแบบฐานข้อมูล และแผนการดำเนินงาน สำหรับโครงการพัฒนาระบบดิจิทัลของ **Diamond Friend** เพื่อยกระดับการจัดการข้อมูลจากรูปแบบกระดาษสู่ระบบที่มีประสิทธิภาพและปลอดภัย

---

## 1. บทนำและวัตถุประสงค์ (Executive Summary)

ปัจจุบันการเก็บข้อมูลผ่านกระดาษ (Paper-based) มีความเสี่ยงต่อการสูญหาย การเข้าถึงข้อมูลที่ล่าช้า และความปลอดภัยของข้อมูลส่วนบุคคล โครงการนี้จึงมีเป้าหมายเพื่อเปลี่ยนกระบวนการทำงานสู่ระบบดิจิทัล (Digital Transformation) โดยมุ่งเน้นที่:
*   **Data Accuracy:** ความถูกต้องและแม่นยำของข้อมูลผ่านระบบ Validation
*   **Accessibility:** การเข้าถึงข้อมูลได้จากทุกที่ (Mobile/Field Work)
*   **Security & Privacy:** การคุ้มครองข้อมูลส่วนบุคคลที่มีความอ่อนไหวตามมาตรฐาน PDPA

---

## 2. การวิเคราะห์คุณค่าทางธุรกิจ (Business Value Analysis)

| หัวข้อ | ปัญหาปัจจุบัน (Pain Points) | สิ่งที่ระบบใหม่จะส่งมอบ (Solution Value) |
| :--- | :--- | :--- |
| **การทำงานหน้างาน** | การจดบันทึกลงกระดาษขณะออกหน่วยเคลื่อนที่ทำได้ยากและซ้ำซ้อน | ระบบ Responsive รองรับ Tablet/iPad บันทึกข้อมูลได้ทันทีแบบ Real-time |
| **ความปลอดภัย** | ข้อมูลความลับ (ผลตรวจ/พฤติกรรมเสี่ยง) อาจถูกเปิดเผยได้ง่าย | ระบบ Role-based Access Control (RBAC) จำกัดสิทธิ์การเห็นข้อมูลเฉพาะผู้ที่เกี่ยวข้อง |
| **การติดตามผล** | การติดตามผลการส่งตัว (Referral) ทำได้ยากและขาดความต่อเนื่อง | ระบบ Dashboard ติดตามสถานะการส่งต่อและแจ้งเตือนผลตรวจที่ยังไม่ได้รับการอัปเดต |
| **การรายงานผล** | ต้องเสียเวลาสรุปผลรายงานรายเดือนด้วยมือ | ระบบสามารถ Export รายงานและสรุปผลเชิงสถิติได้ทันที |

---

## 3. โมดูลหลักของระบบ (System Modules)

ระบบประกอบด้วย 6 โมดูลการทำงานหลัก ได้แก่:

1.  **ระบบจัดการผู้ใช้งาน (User Management):** จัดการบัญชีเจ้าหน้าที่และสิทธิ์การเข้าถึง (RBAC)
2.  **ระบบจัดการข้อมูลผู้รับบริการ (Client Management):** ลงทะเบียนและเก็บประวัติประชากรของผู้รับบริการ (Single Source of Truth)
3.  **ระบบจัดการรอบการบริการ (Service Session):** บันทึกข้อมูลการปฏิบัติงานในแต่ละครั้ง (Mobile Clinic, Outreach, Drop-in)
4.  **ระบบประเมินและจ่ายเวชภัณฑ์ (Assessment & Delivery):** บันทึกการประเมินพฤติกรรมเสี่ยงและจำนวนถุงยาง/สารหล่อลื่นที่แจก
5.  **ระบบจัดการการส่งต่อ (Referral Management):** สร้างใบส่งตัวไปตรวจ (HIV, STIs, TB) และติดตามผลการตรวจจากโรงพยาบาล
6.  **ระบบติดตามการตรวจ HIVST (HIVST Tracking):** ติดตามการแจกและผลการตรวจด้วยตนเอง

---

## 4. บทบาทผู้ใช้งาน (User Roles & Actors)

1.  **Volunteer (อาสาสมัคร):** บันทึกข้อมูลพื้นฐานหน้างานและการจ่ายเวชภัณฑ์
2.  **Staff / Health Worker (เจ้าหน้าที่):** ผู้ใช้งานหลัก บันทึกข้อมูลสุขภาพ การประเมิน และการส่งต่อ
3.  **Supervisor (หัวหน้างาน):** ตรวจสอบคุณภาพข้อมูล ติดตามผล และดูรายงานสรุป
4.  **Administrator (ผู้ดูแลระบบ):** จัดการผู้ใช้งานและตั้งค่าโครงสร้างระบบ

> **หมายเหตุสำคัญ:** ผู้รับบริการ (Client) คือ "หัวเรื่องของข้อมูล" แต่ไม่ใช่ "ผู้ใช้งานระบบ" ในเวอร์ชันนี้ เพื่อความปลอดภัยสูงสุดของข้อมูลสุขภาพที่อ่อนไหวตามมาตรฐาน PDPA

---

## 5. ข้อกำหนดของระบบ (System Requirements)

### 5.1 ข้อกำหนดทางฟังก์ชัน (Functional Requirements)
*   **FR-01:** ระบบต้องยืนยันตัวตนก่อนใช้งานทุกครั้ง
*   **FR-02:** ป้องกันการลงทะเบียนผู้รับบริการซ้ำด้วยเลขบัตรประชาชน
*   **FR-03:** ข้อมูลการประเมินและการแจกของต้องเชื่อมโยงกับ "รอบการบริการ (Session)" เสมอ
*   **FR-04:** รองรับ Unified Workflow (กรอกข้อมูลครบจบในหน้าเดียว)
*   **FR-05:** ผู้ใช้ต้องสามารถสร้างรายการส่งต่อระหว่างรอบการบริการ และกลับมาอัปเดต "ผลการตรวจ" ได้ในภายหลัง
*   **FR-06:** บันทึกประวัติการแก้ไขข้อมูล (Audit Logging) โดยอัตโนมัติ

### 5.2 ข้อกำหนดที่ไม่ใช่ฟังก์ชัน (Non-Functional Requirements)
*   **NFR-01 (Security):** ข้อมูลสุขภาพต้องได้รับสิทธิ์เข้าถึงตามบทบาท (RBAC) อย่างเคร่งครัด
*   **NFR-02 (Responsive):** หน้าจอต้องใช้งานได้ดีทั้งบนคอมพิวเตอร์และแท็บเล็ต (สำหรับงาน Mobile Clinic)
*   **NFR-03 (Data Integrity):** บังคับใช้ Foreign Keys และการตรวจสอบความถูกต้องของข้อมูลนำเข้า
*   **NFR-04 (Performance):** บันทึก Log ทุกครั้งที่มีการเข้าดูหรือแก้ไขข้อมูล (Audit Trail)

---

## 6. รายละเอียดพฤติกรรมการใช้งาน (Use Case Specification)

| UC ID | ชื่อยูสเคส | Volunteer | Staff | Supervisor | Admin |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **UC-01** | เข้าสู่ระบบ / ออกจากระบบ | ✓ | ✓ | ✓ | ✓ |
| **UC-02** | จัดการบัญชีผู้ใช้งาน | - | - | - | ✓ |
| **UC-03** | จัดการตั้งค่าระบบ (แหล่งทุน/คู่ความร่วมมือ) | - | - | - | ✓ |
| **UC-04** | ลงทะเบียน/แก้ไขข้อมูลผู้รับบริการ | - | ✓ | - | - |
| **UC-05** | ค้นหาและดูประวัติผู้รับบริการ | - | ✓ | ✓ | - |
| **UC-06** | สร้างรอบการรับบริการ (Service Session) | ✓ | ✓ | - | - |
| **UC-07** | บันทึกการจ่ายเวชภัณฑ์และถุงยาง | ✓ | ✓ | - | - |
| **UC-08** | บันทึกการประเมินพฤติกรรมเสี่ยง | - | ✓ | - | - |
| **UC-09** | จัดการการส่งตัวไปตรวจ (Referral) | - | ✓ | - | - |
| **UC-10** | อัปเดตผลการตรวจ | - | ✓ | ✓ | - |
| **UC-11** | ติดตามการแจกชุดตรวจ HIVST | - | ✓ | - | - |
| **UC-12** | ดูแดชบอร์ดและรายงานสถิติ | - | - | ✓ | ✓ |
| **UC-13** | ตรวจสอบความถูกต้องของข้อมูล (Audit) | - | - | ✓ | - |

---

## 7. แผนภาพความสัมพันธ์ของข้อมูล (ER Diagram)

\`\`\`mermaid
erDiagram
    STAFF ||--o{ SERVICE_SESSION : "บันทึกการบริการ"
    STAFF ||--o{ SERVICE_SESSION : "สร้างโดย"
    CLIENT ||--o{ SERVICE_SESSION : "รับการบริการ"
    
    SERVICE_SESSION ||--|| BEHAVIORAL_ASSESSMENT : "รวมข้อมูล"
    SERVICE_SESSION ||--|| SERVICE_DELIVERY : "บันทึกการจ่าย"
    SERVICE_SESSION ||--o{ REFERRAL_RECORD : "สร้างใบส่งตัว"
    SERVICE_SESSION ||--o{ HIVST_RECORD : "ติดตามชุดตรวจ"

    CLIENT {
        string client_id PK "รหัสผู้รับบริการ"
        string full_name "ชื่อ-นามสกุล"
        string nickname "ชื่อเล่น"
        string id_card_number "เลขบัตรประชาชน"
        date birth_date "วันเกิด"
        string nationality "สัญชาติ"
        string birth_gender "เพศโดยกำเนิด"
        string phone_number "เบอร์โทรศัพท์"
        string line_id "Line ID"
        string address "ที่อยู่"
        string education_level "ระดับการศึกษา"
        string occupation "อาชีพ"
        string income_range "รายได้เฉลี่ย"
        string health_insurance "สิทธิการรักษา"
        string target_group "กลุ่มเป้าหมาย"
        datetime created_at "สร้างเมื่อ"
        datetime updated_at "แก้ไขเมื่อ"
    }

    STAFF {
        string staff_id PK "รหัสเจ้าหน้าที่"
        string username "ชื่อผู้ใช้"
        string password_hash "รหัสผ่านที่เข้ารหัส"
        string staff_name "ชื่อ-นามสกุลเจ้าหน้าที่"
        string role "บทบาท (Admin, Supervisor, Staff, Volunteer)"
        string email "อีเมล"
        string phone_number "เบอร์โทรศัพท์"
        boolean is_active "สถานะการใช้งาน"
        datetime last_login "เข้าสู่ระบบล่าสุด"
        datetime created_at "สร้างเมื่อ"
    }

    SERVICE_SESSION {
        int session_id PK "รหัสรอบการบริการ"
        string client_id FK "รหัสผู้รับบริการ"
        string staff_id FK "เจ้าหน้าที่ผู้ให้บริการ"
        string created_by FK "เจ้าหน้าที่ผู้บันทึกข้อมูล"
        date service_date "วันที่รับบริการ"
        string access_channel "ช่องทางการเข้าถึง"
        string service_province "จังหวัดที่ให้บริการ"
        string service_district "อำเภอที่ให้บริการ"
        boolean is_mobile_clinic "ออกหน่วยเคลื่อนที่"
        string partner_agency "หน่วยงานร่วม"
        string funding_source "แหล่งงบประมาณ"
        datetime created_at "สร้างเมื่อ"
        datetime updated_at "แก้ไขเมื่อ"
    }

    BEHAVIORAL_ASSESSMENT {
        int session_id PK, FK "รหัสรอบการบริการ"
        string partner_type_3m "ประเภทคู่นอน (3 เดือน)"
        string last_sex_method "วิธีมีเพศสัมพันธ์ล่าสุด"
        string condom_use_freq "ความถี่การใช้ถุงยาง"
        boolean chemsex_history "ประวัติ Chemsex"
        boolean needle_sharing "การใช้เข็มร่วมกัน"
        string last_hiv_test_result "ผลตรวจ HIV ล่าสุด"
        string art_status "สถานะการรับยา ART"
        string disclosure_status "สถานะการเปิดเผยผล"
    }

    SERVICE_DELIVERY {
        int session_id PK, FK "รหัสรอบการบริการ"
        boolean edu_hiv_stis "ความรู้ HIV/STIs"
        boolean edu_prep_pep "ความรู้ PrEP/PEP"
        int condom_49mm_qty "ถุงยาง 49มม. (ชิ้น)"
        int condom_52mm_qty "ถุงยาง 52มม. (ชิ้น)"
        int lubricant_qty "สารหล่อลื่น (ซอง)"
        int syringe_qty "เข็มฉีดยา (ชิ้น)"
    }

    REFERRAL_RECORD {
        int referral_id PK "รหัสการส่งต่อ"
        int session_id FK "รหัสรอบการบริการ"
        string referral_type "ประเภทการส่งตรวจ"
        string destination_hospital "โรงพยาบาลที่ส่งไป"
        date test_date "วันที่ตรวจ"
        string test_result "ผลการตรวจ"
    }

    HIVST_RECORD {
        int session_id PK, FK "รหัสรอบการบริการ"
        date test_date "วันที่ตรวจ"
        string distribution_channel "ช่องทางการรับชุดตรวจ"
        string result "ผลการตรวจเบื้องต้น"
        string confirmed_test_link "ลิงก์ยืนยันผล"
    }
\`\`\`

---

## 8. มาตรการความปลอดภัยและ PDPA

*   **Role-Based Access Control (RBAC):** จำกัดสิทธิ์การเห็นข้อมูลเฉพาะผู้ที่เกี่ยวข้อง อาสาสมัครจะไม่เห็นข้อมูลผลตรวจและพฤติกรรมเสี่ยงเชิงลึก
*   **Data Encryption:** เข้ารหัสรหัสผ่านและข้อมูลสำคัญในฐานข้อมูล
*   **Audit Trail:** บันทึก Log ทุกครั้งที่มีการเข้าดูหรือแก้ไขข้อมูล (ใคร, ทำอะไร, เมื่อไหร่)
*   **Data Minimization:** หน้าจอจะแสดงผลเฉพาะข้อมูลที่จำเป็นตามบทบาทของผู้ใช้งานเท่านั้น

---

## 9. แผนการดำเนินงาน (Implementation Roadmap)

1.  **Phase 1: Foundation (2-4 สัปดาห์)**
    *   ตั้งฐานข้อมูลตาม ERD ที่ออกแบบไว้
    *   พัฒนาระบบ Login และจัดการบทบาทผู้ใช้
2.  **Phase 2: Core Workflow (4-6 สัปดาห์)**
    *   พัฒนาระบบลงทะเบียนผู้รับบริการ (Client Registration)
    *   พัฒนาระบบบันทึกการบริการและแบบประเมินพฤติกรรมเสี่ยง
3.  **Phase 3: Extended Services (3-5 สัปดาห์)**
    *   ระบบจัดการการส่งต่อ (Referral) และการติดตามชุดตรวจ HIVST
    *   ระบบรายงานและ Dashboard เบื้องต้น
4.  **Phase 4: Deployment & Training (2-3 สัปดาห์)**
    *   ทดสอบระบบ (UAT) ร่วมกับผู้ใช้งานจริง
    *   อบรมเจ้าหน้าที่และการนำข้อมูลเข้าระบบ (Data Migration)

---

## 10. สรุปผล (Conclusion)

การนำระบบดิจิทัลนี้มาใช้ ไม่ใช่เพียงแค่การเปลี่ยนจากกระดาษเป็นคอมพิวเตอร์ แต่เป็นการสร้าง **"ระบบนิเวศข้อมูล" (Data Ecosystem)** ที่จะช่วยให้ Diamond Friend สามารถยกระดับคุณภาพการบริการผู้รับบริการได้อย่างยั่งยืน แม่นยำ และปลอดภัยที่สุด

# แผนภาพความสัมพันธ์ของข้อมูล (ER Diagram) - ระบบ Diamond Friend

เอกสารนี้อธิบายโครงสร้างฐานข้อมูลที่ออกแบบมาเพื่อรองรับการทำงานของระบบจัดการข้อมูลผู้รับบริการ โดยอ้างอิงจาก "แบบฟอร์มเก็บข้อมูลผู้รับบริการ"

## แผนภาพ ER Diagram (Mermaid)

```mermaid
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
```

---

## รายละเอียดหน้าที่ของแต่ละตาราง (Entity Descriptions)

### 1. CLIENT (ตารางผู้รับบริการ)
*   **หน้าที่:** เก็บข้อมูลประวัติส่วนตัวและข้อมูลประชากรพื้นฐานของผู้รับบริการ
*   **ความสำคัญ:** เป็นตารางหลักที่ใช้ระบุตัวตน ข้อมูลในนี้มักจะไม่เปลี่ยนแปลงบ่อยครั้ง โดยใช้เลขบัตรประชาชนเพื่อป้องกันการลงข้อมูลซ้ำ

### 2. STAFF (ตารางเจ้าหน้าที่/ผู้ใช้งาน)
*   **หน้าที่:** เก็บข้อมูลเจ้าหน้าที่ทุกคนที่เข้าใช้งานระบบ รวมถึงการกำหนดบทบาท (Role)
*   **ความสำคัญ:** ใช้สำหรับการเข้าสู่ระบบ (Authentication) และการควบคุมสิทธิ์การเข้าถึงข้อมูล (Authorization) ตามระดับ Admin, Supervisor, Staff หรือ Volunteer

### 3. SERVICE_SESSION (ตารางรอบการรับบริการ)
*   **หน้าที่:** เป็นศูนย์กลางของการบันทึกข้อมูล ทุกครั้งที่ผู้รับบริการมาพบเจ้าหน้าที่ จะต้องมีการสร้าง "รอบการบริการ" ใหม่
*   **ความสำคัญ:** ทำหน้าที่เชื่อมโยงระหว่าง "ผู้รับบริการ" กับ "กิจกรรมต่างๆ" (เช่น การแจกของ หรือการประเมิน) ทำให้ทราบว่าการบริการนั้นเกิดขึ้นเมื่อไหร่ ที่ไหน และใครเป็นผู้ให้บริการ

### 4. BEHAVIORAL_ASSESSMENT (ตารางประเมินพฤติกรรมเสี่ยง)
*   **หน้าที่:** เก็บข้อมูลพฤติกรรมทางเพศและความเสี่ยงในช่วงเวลาที่กำหนด (เช่น 3 เดือน)
*   **ความสำคัญ:** เป็นข้อมูลที่มีความอ่อนไหวสูง ใช้สำหรับประเมินความเสี่ยงเพื่อจัดหาบริการที่เหมาะสมให้กับผู้รับบริการ

### 5. SERVICE_DELIVERY (ตารางบันทึกการจ่ายเวชภัณฑ์)
*   **หน้าที่:** บันทึกจำนวนวัสดุอุปกรณ์ป้องกัน (ถุงยางอนามัย, สารหล่อลื่น) และหัวข้อการให้ความรู้
*   **ความสำคัญ:** ใช้สำหรับการจัดการสต็อกและการสรุปยอดการแจกอุปกรณ์ตามแหล่งงบประมาณต่างๆ

### 6. REFERRAL_RECORD (ตารางบันทึกการส่งต่อ)
*   **หน้าที่:** ติดตามสถานะเมื่อมีการส่งตัวผู้รับบริการไปตรวจที่โรงพยาบาล
*   **ความสำคัญ:** ช่วยให้เจ้าหน้าที่สามารถติดตามผลการตรวจ (Referral Loop) ได้อย่างต่อเนื่อง ไม่ให้ข้อมูลขาดหาย

### 7. HIVST_RECORD (ตารางบันทึกชุดตรวจด้วยตนเอง)
*   **หน้าที่:** บันทึกข้อมูลเฉพาะสำหรับการแจกชุดตรวจ HIV แบบตรวจด้วยตนเอง (HIV Self-Test)
*   **ความสำคัญ:** ใช้ติดตามว่าผู้รับบริการนำชุดตรวจไปใช้จริงหรือไม่ และผลการตรวจเบื้องต้นเป็นอย่างไรเพื่อดำเนินการให้คำปรึกษาต่อไป

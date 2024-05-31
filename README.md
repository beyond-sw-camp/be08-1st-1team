# 파인닥 (Findoc)

![인트로이미지](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/28796063/ac7970ba-df03-4250-b755-364d25dc31ac)

> [플레이 데이터] 한화시스템 BEYOND SW캠프 8기 / Team Primary
  
|[박준혁](https://github.com/monet2155) | [김도하](https://github.com/esueng) | [김은경](https://github.com/kuk329) | [이지정](https://github.com/leejijung) | [황지우](https://github.com/jbr1tr) |
|------------------------------------------|--------------------------------------|------------------------------------------|-----------------------------------|-------------------------------------|

<!-- 기술 스택 (아이콘) -->
<img src="https://img.shields.io/badge/mariaDB-003545?style=for-the-badge&logo=mariaDB&logoColor=white">


<!-- 🎬[Demo 시연영상](https://www.youtube.com/watch?v=dhMrKTwNI8U&lc=UgzCJR3WxkvsckRyyO94AaABAg&ab_channel=%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B4%EC%84%9C%EB%B0%B0%EC%9A%B0%EB%8A%94IT)   
📃[프로젝트 회고록](블로그주소)
-->

----------

## ✨ 프로젝트 설명

![프로젝트_필요성](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/28796063/099759b4-0509-4d3e-bd20-49a8f6b8cacb)

- 갑작스런 아픔이나 질병으로 병원에 가야 할 때, 어디로 가야 할지 막막했죠?
- 특히 휴일이나 밤늦게 아플 경우, 더욱 어려움을 겪으셨을 거예요.
- 어렵게 병원을 찾는다고 해도, 진료 대기 시간이 길어 답답하지 않으셨나요?

파인닥은 바로 이런 불편함을 해결하기 위해 만들어진 서비스입니다.

파인닥은 지금 당장 또는 원하는 날짜에 이용 가능한 병원을 쉽게 찾을 수 있고, 예약해서 기다리지 않고 바로 진료를 볼 수 있어요.

**파인닥과 함께, 갑작스런 아픔에도 당황하지 않고 건강한 삶을 위한 첫걸음을 내딛으세요!**

----------

## 🐧 프로젝트 주요 기능
[요구사항 명세](https://docs.google.com/spreadsheets/d/1-901JV0erwZaMJBfVRsbWhYAnOgtMyhiOb7uzIzZk0g/edit#gid=0)

#### 병원 찾기
- 영업 시간, 점심 시간 기반 지금 갈 수 있는 병원 탐색
- 병원 정보, 진료 과목 등 다양한 조건으로 병원 검색
#### 병원 예약
- 환자 정보와 증상을 기재하고 진료 예약 신청
- 병원 시스템에서 예약 관리(예약 확정, 취소 등)
#### 회원 관리
- 회원가입, 로그인 등 기본적인 회원 관리
- 예약/진료기록 열람
- 기저질환, 복용 중인 약 설정
#### 보호자 대리 예약
- 보호자-피보호자 시스템으로 피보호자의 진료 대리 예약 가능

## 💻 프로젝트 구현
<!-- 구동 움짤 -->
### 주요 특징

### DB 모델링
<details>
  <summary> <span class="summary-header">개념 모델링</span></summary>

  ![erd_gn](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/edbb8c5c-306c-4dd6-978a-e0291d34e5a2)

</details>
<details>
  <summary> <span class="summary-header">논리 모델링</span></summary>

  ![ERD_logical_findoc](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/2e084a49-68a6-4191-96d7-06a3a5583527)

</details>
<details>
  <summary> <span class="summary-header">물리 모델링</span></summary>

  ![ERD_physical_findoc](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/d4f78a01-21e8-408e-8340-bde06e37b678)

</details>

  
### DDL 및 주요 쿼리

  <details>
    <summary> time_interval</summary>
  </details>
  <details>
    <summary> time_interval</summary>
      <p align="center">
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/0910f3fc-4b46-4968-b307-1809f2039b99" alt="Description of first image" width="300"/>
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/ccaed4d3-bcc1-403a-aa5b-266084773362" alt="Description of second image" width="300"/>
    </p>




```sql
-- 일주일간의 시간들 담을 테이블
CREATE OR REPLACE TABLE time_interval (
    half_hour DATETIME,
    onactive ENUM('active', 'deactive'),
    doctor_no INT,
    FOREIGN KEY (doctor_no) REFERENCES doctor(doctor_no)
);
'''
금일부터 일주일간의 시간들 생성 프로시저
(오늘 이전은 삭제 오늘로부터 일주일 중 없는 시간이 있다면 생성,
이미 테이블에 있는 시간에 대해서는 변동없음)
'''

DELIMITER $$

CREATE OR REPLACE PROCEDURE loopwhile()
BEGIN
    DECLARE start_datetime DATETIME;
    DECLARE end_datetime DATETIME;
    DECLARE current_datetime DATETIME;

    -- 시작과 종료 시간 설정
    SET start_datetime = DATE(NOW());  -- 오늘 자정
    SET end_datetime = DATE_ADD(start_datetime, INTERVAL 7 DAY);  -- 일주일 후

    -- 오늘 이전의 데이터 삭제
    DELETE FROM time_interval WHERE half_hour < start_datetime;

    -- 의사별 일주일 간 30분 간격 데이터 삽입
    WHILE start_datetime < end_datetime DO
        INSERT INTO time_interval (half_hour, onactive, doctor_no)
        SELECT start_datetime, 'deactive', doctor_no
        FROM doctor
        WHERE NOT EXISTS (
            SELECT 1 FROM time_interval
            WHERE half_hour = start_datetime AND doctor_no = doctor.doctor_no
        );

        -- 다음 30분 간격 설정
        SET start_datetime = DATE_ADD(start_datetime, INTERVAL 30 MINUTE);
    END WHILE;
END$$

DELIMITER ;

-- 일주일 시간 업데이트 프로시저 실행
CALL loopwhile();

-- 근무시간 테이블 생성
CREATE TABLE worktime (
    doctor_no INT,
    start_worktime DATETIME,
    end_worktime DATETIME,
    FOREIGN KEY (doctor_no) REFERENCES doctor(doctor_no)
);

DELIMITER $$

-- 근무시간표가 업데이트 될 때 해당 사이 시간 active 로 변경
CREATE TRIGGER activate_time_intervals
AFTER INSERT ON worktime
FOR EACH ROW
BEGIN
    -- time_interval 테이블의 onactive 상태를 'active'로 업데이트
    UPDATE time_interval
    SET onactive = 'active'
    WHERE doctor_no = NEW.doctor_no
      AND half_hour >= NEW.start_worktime
      AND half_hour <= NEW.end_worktime;
END$$

DELIMITER ;

-- 특정 의사의 특정 시간에 대해서 activate 하는 쿼리 ( deactive도 문제 없음 )
UPDATE time_interval
SET onactive = 'active'
WHERE doctor_no = 1
  AND half_hour = '2024-05-01 08:00:00';
  
-- worktime 테스트 케이스 삽입
INSERT INTO worktime (doctor_no, start_worktime, end_worktime) VALUES
(1, '2024-06-02 08:00:00', '2024-06-02 09:30:00');

-- time_interval 테이블 업데이트 확인
SELECT *
FROM time_interval
WHERE doctor_no=1;
```
  </details>
  
  #### 병원 검색 
  <details>
  <summary>필터 기반 병원 검색</summary>
  <div>
   
  * 수술실, MRI가 있는 외과 검색
  ```sql

  SELECT h.hosp_name AS "병원명",
        w.worktime_start AS "진료 시작 시간",
        w.worktime_end AS "진료 종료 시간",
        dept.dept_name AS "진료 과목"
  FROM hospital h
  JOIN facility f ON h.hosp_no = f.hosp_no
  JOIN equipment e ON h.hosp_no = e.hosp_no
  JOIN doctor doc ON h.hosp_no = doc.hosp_no
  JOIN doctor_dept dd ON doc.doctor_no = dd.doctor_no
  JOIN department dept ON dd.dept_id = dept.dept_id
  JOIN location l ON h.hosp_no = l.hosp_no
  JOIN worktime w ON doc.doctor_no = w.doctor_no
  WHERE f.facility_name = "수술실" AND e.equipment_name = "MRI" AND dept.dept_name = "외과";
  ```
  </div>
  </details>

  #### 병원 예약 
  <details>
  <summary>환자 본인 진료 예약</summary>
  <div>
   
  * 해당 예약 시간에 선택한 담당의가 active이면 예약 가능 

  ```sql

  INSERT INTO appointment (appt_date, appt_symptom, user_no, hosp_no, doctor_no)
  SELECT '2024-06-02 08:30:00', '복통', 6, 1, 1
  WHERE EXISTS (
    SELECT 1 
    FROM time_interval 
    WHERE half_hour = '2024-06-02 08:30:00'
      AND doctor_no = 1
      AND onactive = 'active'
  ) AND EXISTS (SELECT 1 FROM doctor WHERE hosp_no=1 AND doctor_no=1);
  ```
  </div>
  </details>
  <details>
  <summary>보호자가 피보호자의 진료 예약</summary>
  <div>
   
  * 해당 예약 시간에 선택한 담당의가 active이고, 보호자 관계가 성립하면 예약 가능 

  ```sql

  DELIMITER //

  CREATE PROCEDURE AddAppointmentByGuardian (
      IN in_guard_no INT,
      IN in_ward_no INT,
      IN appt_date DATETIME,
      IN appt_symptom VARCHAR(50),
      IN hosp_no INT,
      IN doctor_no INT,
      IN guard_ID VARCHAR(20),
      IN ward_ID VARCHAR(20)
  )
  BEGIN
      DECLARE guard_exist INT;
      DECLARE doctor_time INT;

      -- 보호자와 피보호자 관계 확인
      SELECT COUNT(*) INTO guard_exist
      FROM `guardian`
      WHERE `guard_no` = in_guard_no
        AND `ward_no` = in_ward_no;

      -- 의사의 활성화된 시간 확인
      SELECT COUNT(*) INTO doctor_time
      FROM time_interval 
      WHERE half_hour = appt_date
        AND doctor_no = doctor_no
        AND onactive = 'active';

      IF guard_exist > 0 AND doctor_time > 0 THEN
          -- 피보호자를 대신하여 예약 신청
          INSERT INTO `appointment` 
          (appt_date, appt_status, appt_symptom, user_no, hosp_no, doctor_no, guard_ID, ward_ID)
          VALUES
          (appt_date, 'waiting', appt_symptom, in_ward_no, hosp_no, doctor_no, guard_ID, ward_ID);
      ELSEIF doctor_time = 0 THEN
          SIGNAL SQLSTATE '45000'
          SET MESSAGE_TEXT = 'Deactive time';
      ELSEIF guard_exist = 0 THEN 
          SIGNAL SQLSTATE '45000'
          SET MESSAGE_TEXT = 'Invalid guardian or ward relationship.';
      END IF;
  END //

  DELIMITER ;

  -- CALL 예시 
  CALL AddAppointmentByGuardian(6, 1, '2024-06-02 08:30:00', '고혈압 증상', 1, 1, 'user06', 'user01');
  ```
  </div>
  </details>
  <details>
  <summary>병원이 진료 예약 확인</summary>
  <div>
   
  * 병원 ID를 통해 예약 내역 확인 
  ```sql

  SELECT h.hosp_name AS "병원명",
	    a.appt_date AS "예약일시",
	    u.user_name AS "환자명",
	    a.appt_symptom AS "증상",
	    u.user_phone AS "환자 전화번호",
	    u.user_disease AS "기저질환",
	    a.appt_status AS "예약상태" 
  FROM hospital h
  JOIN appointment a ON h.hosp_no = a.hosp_no
  JOIN user u ON u.user_no = a.user_no
  WHERE h.hosp_id = 'hosp03';
  ```
  </div>
  </details>

  #### 예약 취소 
  <details>
  <summary>환자 본인 진료 예약 취소</summary>
  <div>
   
  * 예약 번호, 회원 아이디, 비밀번호가 일치하면 예약 취소 허용
  ```sql

  DELETE FROM `appointment`
  WHERE `appt_no` = 7
    AND `appt_status` = 'waiting' OR `appt_status` = 'accepted'
    AND `user_no` = (SELECT `user_no` 
                    FROM `user` 
                    WHERE `user_name` = '박민형' 
                      AND `user_id` = 'user03' 
                      AND `user_pwd` = 'password3');
  SELECT * FROM appointment;
  ```
  </div>
  </details>
  <details>
  <summary>보호자가 피보호자의 진료 예약 취소</summary>
  <div>
  
  * 보호자 아이디, 보호자 비밀번호를 입력받고 해당 예약 내역에 대해 보호자 관계가 성립하면 예약 취소 허용
  ```sql

  DELIMITER //

  CREATE PROCEDURE CancelAppointmentByGuardian (
      IN guard_no INT,
      IN guard_id VARCHAR(50),
      IN guard_password VARCHAR(50),
      IN ward_no INT,
      IN appt_no INT
  )
  BEGIN
      DECLARE guard_exist INT;
      DECLARE appointment_exist INT;

      -- 보호자 자격 및 ID와 비밀번호 확인
      SELECT COUNT(*) INTO guard_exist
      FROM `guardian` g
      JOIN `user` u ON g.guard_no = u.user_no
      WHERE g.guard_no = guard_no
        AND u.user_id = guard_id
        AND u.user_pwd = guard_password
        AND g.ward_no = ward_no
        AND g.guard_allowed = 'completed';

      -- 예약이 존재하는지 확인
      SELECT COUNT(*) INTO appointment_exist
      FROM `appointment`
      WHERE appt_no = appt_no
        AND user_no = ward_no;
        
      IF guard_exist > 0 AND appointment_exist > 0 THEN
          -- appointment 테이블에서 해당 예약에 대한 레코드 삭제
          DELETE FROM `appointment`
          WHERE appt_no = appt_no
            AND user_no = ward_no
        AND appt_status = 'waiting';
      ELSE
          SIGNAL SQLSTATE '45000'
          SET MESSAGE_TEXT = 'Invalid guardian credentials or relationship, or appointment does not exist.';
      END IF;
  END //

  DELIMITER ;

  -- CALL 예시
  CALL CancelAppointmentByGuardian(6, 'user06', 'password6', 1, 10);
  ```
  </div>
  </details>
<details>
  <summary>병원이 진료 예약 거절</summary>
  <div>
  
  * appointment 테이블에서 appt_status가 rejected로 변경되면 rejection 테이블에 해당 거절 내역 추가 
  ```sql

  DELIMITER $$

  CREATE TRIGGER after_appointment_update
  AFTER UPDATE ON appointment
  FOR EACH ROW
  BEGIN
      IF NEW.appt_status = 'rejected' AND OLD.appt_status = 'waiting' THEN
          INSERT INTO rejection (rejection_result, appt_no)
          VALUES ('Reservation cancelled by hospital', NEW.appt_no);
      END IF;
  END $$

  DELIMITER ;

  -- UPDATE 예시 
  UPDATE appointment 
  SET appt_status = "rejected"
  WHERE appt_status = "waiting" AND hosp_no = 1 AND appt_no = 8;
  ```
  </div>
  </details>

  #### 예약 변경
  <details>
  <summary>환자 본인 진료 예약 변경</summary>
  <div>
   
  * 예약 번호, 회원 아이디, 비밀번호가 일치하면 예약 변경 허용
  ```sql

  -- 사용자가 자신의 예약 정보 변경 
  update appointment a
  inner join user u
  on u.user_no=a.user_no
  set a.appt_symptom = '오늘 아침까지 열이났어요'
  where  a.appt_no = 1 -- 특정 예약 식별값
          and u.user_id='user01'   -- 유저 id
          and u.user_pwd='password1' -- 유저 pwd
          and u.user_no=1     -- 유저 고유식별값
          and appt_status ='waiting'; -- 예약 상태
  ```
  </div>
  </details>
  <details>
  <summary>보호자가 피보호자의 진료 예약 변경</summary>
  <div>
   
  * 보호자 아이디, 보호자 비밀번호를 입력받고 해당 예약 내역에 대해 보호자 관계가 성립하면 예약 변경 허용
  ```sql

  update appointment
  set appt_symptom = '오늘 아침까지 열이났어요'
  where appt_status ='waiting' -- 예약 상태
        and appt_no=1  -- 특정 예약 식별값
        and user_no in (select ward_no    -- 특정 보호자의 피보호자 리스트
                        from guardian g
                        join user u on g.guard_no=u.user_no
                        where u.user_id='user06'    -- 보호자 id
                              and user_pwd='password6' -- 보호자 pwd
                              and g.guard_allowed='completed') ; -- 예약 상태

  ```
  </div>
  </details>

  #### 예약 수락 / 거절
  <details>
  <summary>병원이 예약 수락 / 거절</summary>
  <div>
   
  * 예약 번호를 입력받아 수락/거절 여부 결정 
  ```sql

  -- 예약 상태 변경 프로시저
  DELIMITER $$
  CREATE PROCEDURE change_appointment_status (
      IN p_appt_no INT,
      IN p_status enum('rejected','accepted'),
      IN p_reason TEXT
  )
  BEGIN
      -- 예약 상태 업데이트
      UPDATE appointment
      SET appt_status = p_status
      WHERE appt_no = p_appt_no;

      -- 거절인 경우 거절 사유 추가
      IF p_status = 'rejected' THEN
          INSERT INTO rejection (rejection_result, appt_no)
          VALUES (p_reason, p_appt_no);
      END IF;
  END $$
  DELIMITER ;


  -- call 예시
  call change_appointment_status(1,'accepted',NULL);

  call change_appointment_status(1,'rejected','담당의사가 개인사정으로 오늘 휴진합니다.');

  ```
  </div>
  </details>

#### 진료기록 확인 
  <details>
  <summary>진료가 완료된 진료 기록 확인</summary>
  <div>
   
  * 회원 아이디, 비밀번호를 입력받아 진료 기록 확인 
  ```sql

  select appt_date as '방문 날짜',
       u.user_name as '사용자명',
       h.hosp_name as '병원 이름',
       d.doctor_name as '의사 이름',
       ifnull(appt_symptom,'없음') as '증상',
       m.record_diagnosis as '진단 내용',
       m.record_treatment as '처방 내용'
  from user u
  left join appointment app on u.user_no=app.user_no
  join hospital h on app.hosp_no = h.hosp_no
  join doctor d on app.doctor_no=d.doctor_no
  join medical_record m on app.appt_no = m.appt_no
  where appt_status= 'complete' and u.user_id = 'user08' and u.user_pwd='password8';

  ```
  </div>
  </details>
  

### 테스트 케이스
<details>
  <summary>Tables</summary>

<details>
  <summary> User Table </summary>
  
  | user_id     | user_pwd     | user_name     | user_birthdate | user_addr         | user_phone   | user_disease   | user_medicine  |
|-------------|--------------|---------------|----------------|-------------------|--------------|----------------|----------------|
| john_doe    | password123  | John Doe      | 1985-02-15     | 1234 Broadway St  | 01012345678  | Asthma         | Ventolin       |
| jane_smith  | password123  | Jane Smith    | 1990-08-25     | 2345 Maple Ave    | 01098765432  | Diabetes       | Metformin      |
| susan_lee   | password789  | Susan Lee     | 1975-05-22     | 7890 Elm St       | 0105556677   | Hypertension   | Lisinopril     |
| mike_brown  | mike1234     | Mike Brown    | 1988-11-16     | 4567 Pine St      | 0108765432   | None           | NULL           |
| lisa_ray    | lisa9876     | Lisa Ray      | 1992-03-30     | 321 Oak St        | 0102345678   | Allergies      | Cetirizine     |
| alex_gray   | alexpass     | Alex Gray     | 1983-09-12     | 1579 River Rd     | 0105647382   | None           | NULL           |
| emma_white  | emma1234     | Emma White    | 1995-07-20     | 2020 Sunset Blvd  | 0104321567   | Eczema         | Hydrocortisone |
| noah_wilson | noahpass     | Noah Wilson   | 1980-01-05     | 450 Mountain View | 0109876543   | Anxiety        | Zoloft         |
| olivia_harris | oliviah123 | Olivia Harris | 1992-11-10     | 789 East Dr       | 0106667778   | Asthma         | Ventolin       |
| james_lopez | jamesl456    | James Lopez   | 1979-08-23     | 321 West St       | 0102223334   | Diabetes       | Insulin        |

</details>
<details>
  <summary> Guardian Table</summary>
  
  | guard_no | ward_no | guard_relationship | guard_allowed |
|----------|---------|--------------------|---------------|
| 1        | 2       | Parent             | completed     |
| 2        | 3       | Sibling            | completed     |
| 1        | 4       | Child              | waiting       |
| 4        | 5       | Parent             | completed     |
| 6        | 7       | Spouse             | completed     |
| 8        | 9       | Child              | waiting       |

</details>
<details>
  <summary>Hospital Table</summary>
  
  | hosp_id    | hosp_pwd    | hosp_name                 | hosp_phone |
|------------|-------------|---------------------------|------------|
| bestcare   | hosp1234    | Best Care Medical Center  | 021234567  |
| cityhealth | citypass    | City Health Clinic        | 023456789  |
| medicore   | secure1234  | MediCore Facility         | 024567890  |
| greenmed   | green2023   | Green Medical Services    | 027891011  |
| bluestar   | blue1234    | Blue Star Hospital        | 028765432  |
</details>

<details>
  <summary> Location Table </summary>

  | loc_addr            | loc_lat | loc_long  | hosp_no |
|---------------------|---------|-----------|---------|
| 6789 Hospital Rd    | 37.7749 | -122.4194 | 1       |
| 123 Health Blvd     | 40.7128 | -74.0060  | 2       |
| 456 Clinic Rd       | 34.0522 | -118.2437 | 3       |
| 500 Clinic Center Dr| 39.9042 | -75.1698  | 4       |
| 1200 Health Park    | 33.6844 | -117.8265 | 5       |

</details>

<details>
  <summary> Notice Table</summary>

  | notice_datetime | notice_body                        | hosp_no |
|-----------------|------------------------------------|---------|
| NOW()           | Please wear a mask.                | 1       |
| NOW()           | Flu shots available.               | 2       |
| NOW()           | New COVID-19 guidelines updated.   | 3       |
| NOW()           | Annual health checkup discount event.| 4    |
| NOW()           | COVID-19 vaccination available.    | 5       |

</details>

<details>
  <summary>Facility Table</summary>

  | facility_name       | hosp_no |
|---------------------|---------|
| Emergency Room      | 1       |
| Intensive Care Unit | 2       |
| Pediatrics Wing     | 3       |
| Maternity Ward      | 4       |
| Oncology Center     | 5       |

  
</details>

<details>
  <summary>Equipment Table</summary>
  
  | equipment_name | hosp_no |
|----------------|---------|
| MRI Scanner    | 1       |
| Ultrasound     | 2       |
| X-Ray Machine  | 3       |
| CT Scanner     | 4       |
| ECG Machine    | 5       |

</details>
<details>
  <summary>Department Table</summary>

  | dept_id | dept_name   |
|---------|-------------|
| cardio  | Cardiology  |
| gynae   | Gynecology  |
| ortho   | Orthopedics |

</details>
<details>
  <summary>Doctor Table</summary>

  | hosp_no | doctor_name       | doctor_gender |
|---------|-------------------|---------------|
| 1       | Dr. Alice Johnson | F             |
| 2       | Dr. Emily White   | F             |
| 3       | Dr. Robert Green  | M             |
| 4       | Dr. Charlotte Johnson | F         |
| 5       | Dr. Henry Martinez| M             |

</details>
<details>
  <summary>Doctor Departmentw Table</summary>

  | doctor_no | dept_id | docdept_room |
|-----------|---------|--------------|
| 1         | cardio  | 101A         |
| 2         | gynae   | 202B         |
| 3         | ortho   | 303C         |
| 4         | gynae   | 403D         |
| 5         | ortho   | 505E         |

</details>
<details>
  <summary>Worktime Table</summary>  

  | worktime_start       | worktime_end         | doctor_no |
|----------------------|----------------------|-----------|
| 2023-01-01 08:00:00  | 2023-01-01 16:00:00  | 1         |
| 2023-01-02 09:00:00  | 2023-01-02 17:00:00  | 2         |
| 2023-01-03 10:00:00  | 2023-01-03 18:00:00  | 3         |
| 2023-01-04 08:00:00  | 2023-01-04 14:00:00  | 4         |
| 2023-01-05 12:00:00  | 2023-01-05 18:00:00  | 5         |

</details>
<details>
  <summary>Appointment Table</summary>

  | appt_date            | appt_symptom      | user_no | hosp_no | doctor_no |
|----------------------|-------------------|---------|---------|-----------|
| 2023-12-15 10:00:00  | Cough and fever   | 1       | 1       | 1         |
| 2023-12-20 11:00:00  | Headache          | 2       | 2       | 2         |
| 2023-12-21 12:00:00  | Broken leg        | 3       | 3       | 3         |
| 2023-12-22 14:00:00  | Regular checkup   | 4       | 4       | 4         |
| 2023-12-23 15:00:00  | Chemotherapy session | 5   | 5       | 5         |

</details>
<details>
  <summary>Medical Record Table</summary>

  | record_diagnosis | record_treatment    | appt_no |
|------------------|---------------------|---------|
| Flu              | Rest and medication | 1       |
| Migraine         | Prescribed pain relief | 2     |
| Leg fracture     | Surgery required    | 3       |
| General checkup  | All clear           | 4       |
| Cancer treatment | Chemotherapy        | 5       |

</details>
<details>
  <summary>Rejection Table</summary>

  | rejection_result                  | appt_no |
|-----------------------------------|---------|
| Doctor unavailable on requested date | 1     |
| Unavailable for requested time    | 2       |
| Doctor on leave                   | 3       |

</details>
</details>

## 👫 CO-OP

### WBS
<details>
  <summary>접기/펼치기
    
  </summary>
  https://docs.google.com/spreadsheets/d/1hpVTMaa_74JfIQDtYtLpZEWX7O0yWWgvPrazUaNrMxc/edit#gid=1835326347
  
  ![wbs](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/6ed5b4dd-06af-4889-93bd-82d9ee2614ea)

</details>

### 회고
|팀원|회고록|
|-----|-----|
|김도하|    |
|김은경|    |
|이지정|    |
|황지우|    |

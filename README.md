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

---

## ✨ 프로젝트 개요
### 1. 프로젝트 소개 
파인닥은 사용자들이 병원을 신속하고 편리하게 찾고 예약할 수 있도록 돕는 `종합 병원 예약 시스템`입니다. 사용자는 현재 위치 및 시간에 기반한 병원 검색부터, 다양한 조건을 활용한 병원 탐색, 실시간 예약 및 관리 기능을 이용할 수 있습니다. 파인닥은 병원 이용의 불편함을 최소화하고, 보다 효율적인 의료 서비스를 제공하는 것을 목표로 합니다.
### 2. 프로젝트 필요성 
![프로젝트_필요성](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/28796063/099759b4-0509-4d3e-bd20-49a8f6b8cacb)

> - 갑작스런 아픔이나 질병으로 병원에 가야 할 때, 어디로 가야 할지 막막했죠?
> - 특히 휴일이나 밤늦게 아플 경우, 더욱 어려움을 겪으셨을 거예요.
> - 어렵게 병원을 찾는다고 해도, 진료 대기 시간이 길어 답답하지 않으셨나요?

파인닥은 바로 이런 불편함을 해결하기 위해 만들어진 서비스입니다. 파인닥을 사용하면 지금 당장 또는 원하는 날짜에 이용 가능한 병원을 쉽게 찾을 수 있고, 예약해서 기다리지 않고 바로 진료를 볼 수 있어요. 파인닥과 함께, 갑작스런 아픔에도 당황하지 않고 건강한 삶을 위한 첫걸음을 내딛으세요!

### 3. 프로젝트 주요 기능 
#### 1. 병원 찾기
- 영업 시간, 점심 시간 기반 지금 갈 수 있는 병원 탐색
- 병원 정보, 진료 과목 등 다양한 조건으로 병원 검색
#### 2. 병원 예약
- 환자 정보와 증상을 기재하고 진료 예약 신청
- 병원 시스템에서 예약 관리(예약 확정, 취소 등)
#### 3. 회원 관리
- 회원가입, 로그인 등 기본적인 회원 관리
- 예약/진료기록 열람
- 기저질환, 복용 중인 약 설정
#### 4. 보호자 대리 예약
- 보호자-피보호자 시스템으로 피보호자의 진료 대리 예약 가능

---

## 📊 WBS
[WBS 바로가기](https://docs.google.com/spreadsheets/d/1hpVTMaa_74JfIQDtYtLpZEWX7O0yWWgvPrazUaNrMxc/edit#gid=1835326347)

---

## 📍 요구사항 명세
[요구사항 명세서 바로가기](https://docs.google.com/spreadsheets/d/1-901JV0erwZaMJBfVRsbWhYAnOgtMyhiOb7uzIzZk0g/edit#gid=0)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/492654b2-0f72-496c-994e-8edd8dcbb1f5)

---

## 🗂️ DB 모델링 
### 1. 개념 모델링
![erd_gn](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/edbb8c5c-306c-4dd6-978a-e0291d34e5a2)
### 2. 논리 모델링
![ERD_logical_findoc](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/2e084a49-68a6-4191-96d7-06a3a5583527)
### 3. 물리 모델링 
![ERD_physical_findoc](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/d4f78a01-21e8-408e-8340-bde06e37b678)

---

## 📝 DDL
  <details>
  <summary>DDL</summary>
  <div>
	  
  ```sql	
  #회원 테이블
		CREATE TABLE `user` (
			`user_no`	INT AUTO_INCREMENT PRIMARY KEY,											-- 유저 식별번호
			`user_id`	VARCHAR(20)	NOT NULL UNIQUE,										-- 유저 ID
			`user_pwd`	VARCHAR(20)	NOT NULL,											-- 유저 Password
			`user_name`	VARCHAR(20)	NOT NULL,											-- 유저 이름
			`user_birthdate`	DATE NOT NULL,												-- 유저 생년월일
			`user_addr`	VARCHAR(50)	NULL,												-- 유저 주소
			`user_phone`	VARCHAR(15)	NOT NULL UNIQUE,										-- 유저 전화번호
			`user_enrolldate`	DATE	NOT NULL	DEFAULT CURDATE(),								-- 유저 회원가입일
			`user_disease`	VARCHAR(50),													-- 유저 기저질환
			`user_medicine`	VARCHAR(50),													-- 유저 복용중인 약
			`user_secession`	ENUM('activate', 'deactivate', 'suspended')	NOT NULL	DEFAULT 'activate',			-- 유저 탈퇴여부(활성화, 비활성화, 정지)
			`secession_date`	DATETIME												-- 유저 탈퇴날짜
		);
		
		
		#보호자/피보호자 테이블
		CREATE TABLE `guardian` (
			`guard_no`	INT	NOT NULL REFERENCES user(`user_no`)								-- 보호자(유저 식별번호)
			`ward_no`	INT	NOT NULL REFERENCES user(`user_no`),								-- 피보호자(유저 식별번호)
			`guard_relationship`	VARCHAR(20)	NULL,										-- 보호자/피보호자 관계
			`guard_allowed`	ENUM('waiting', 'completed', 'rejected')	NOT NULL	DEFAULT 'waiting',			-- 보호자/피보호자 신청 상태(대기, 연결완료, 거절)
			PRIMARY KEY(guard_no, ward_no)
		);
		
		
		# 병원 테이블
		CREATE TABLE `hospital` (
			`hosp_no`	INT AUTO_INCREMENT	PRIMARY KEY,									-- 병원 유저 식별번호
			`hosp_id`	VARCHAR(20)	NOT NULL UNIQUE,									-- 병원 유저  ID
			`hosp_pwd`	VARCHAR(20)	NOT NULL,										-- 병원 유저 Password
			`hosp_name`	VARCHAR(30)	NOT NULL,										-- 병원 이름
			`hosp_phone`	VARCHAR(15)	NULL UNIQUE,										-- 병원 전화번호
			`hosp_secession` ENUM('activate', 'deactivate', 'suspended') NOT NULL	DEFAULT 'activate',				-- 병원 유저 탈퇴 여부(활성화, 비활성화, 정지)
			`secession_date` DATETIM												-- 병원 유저 탈퇴날짜
		);
		
		
		# 병원 위치정보 테이블
		CREATE TABLE `location` (
		   `loc_no` INT AUTO_INCREMENT PRIMARY KEY,							-- 병원 위치정보 식별 번호
			`loc_addr`	VARCHAR(100)	NOT NULL UNIQUE,					-- 병원 주소
			`loc_lat`	DOUBLE	NOT NULL,							-- 병원 위도
			`loc_long`	DOUBLE	NOT NULL,							-- 병원 경도
			`hosp_no`	INT	NOT NULL REFERENCES `hospital`(`hosp_no`)			-- 병원 식별 번호
		);
		
		
		
		# 병원 공지사항 테이블
		CREATE TABLE `notice` (
		   `notice_no` INT AUTO_INCREMENT PRIMARY KEY,								-- 공지사항 식별 번호
			`notice_datetime`	DATETIME	NOT NULL	DEFAULT CURTIME(),			-- 공지사항 입력 날짜
			`notice_body`	VARCHAR(300)	NOT NULL,							-- 공지사항 내용
			`hosp_no`	INT	NOT NULL REFERENCES hospital(`hosp_no`)					-- 병원 식별번호
		);
		
		
		# 병원 시설 테이블
		CREATE TABLE `facility` (
		   `facility_no` INT AUTO_INCREMENT PRIMARY KEY,					-- 시설 식별 번호
			`facility_name`	VARCHAR(50)	NOT NULL,					-- 병원 시설 이름
			`hosp_no`	INT	NOT NULL REFERENCES hospital(`hosp_no`)			-- 병원 식별 번호
		);
		
		
		# 병원 장비 테이블
		CREATE TABLE `equipment` (
		   `equipment_no` INT AUTO_INCREMENT PRIMARY KEY,				-- 테이블 pk
			`equipment_name`	VARCHAR(50)	NOT NULL,			-- 병원 장비 이름
			`hosp_no`	INT	NOT NULL REFERENCES hospital(`hosp_no`)		-- 병원 식별 번호
		);
		
		
		# 의사 진료과목 목록 테이블
		CREATE TABLE `department` (
			`dept_id`	VARCHAR(10)	PRIMARY KEY,		-- 진료과 ID
			`dept_name`	VARCHAR(30)	NOT NULL		-- 진료과 이름
		);
		
		
		# 의사 테이블
		CREATE TABLE `doctor` (
		    `doctor_no`    INT AUTO_INCREMENT PRIMARY KEY,				-- 의사 식별 번호
		    `hosp_no`    INT    NOT NULL REFERENCES `hospital`(`hosp_no`),		-- 병원 식별 번호
		    `doctor_name` VARCHAR(40) NOT NULL,						-- 의사 이름
		    `doctor_gender` ENUM('F', 'M') NULL						-- 의사 성별
		);
		
		
		# 의사의 진료과목, 진료실 테이블
		CREATE TABLE `doctor_dept` (
			`docdept_no`	INT	AUTO_INCREMENT	PRIMARY KEY,					-- 의사_진료과 식별 번호
			`doctor_no`	INT	NOT NULL REFERENCES doctor(`doctor_no`),			-- 의사 식별번호
			`dept_id`	VARCHAR(10)	NOT NULL	REFERENCES department(`dept_id`),	-- 진료과목 ID
			`docdept_room` VARCHAR(20)								-- 의사 진료실명
		);
		
		# 의사 진료시간 테이블
		CREATE TABLE `worktime` (
		   `worktime_no` INT AUTO_INCREMENT PRIMARY KEY,					-- 테이블 pk
			`worktime_start`	DATETIME	NOT NULL,				-- 의사 진료 시작 시간
			`worktime_end`	DATETIME	NOT NULL,					-- 의사 진료 종료 시간
			`doctor_no`	INT	NOT NULL REFERENCES doctor(`doctor_no`)			-- 의사 식별 번호
		);
		
		
		# 예약 테이블
		CREATE TABLE `appointment` (
			`appt_no`	INT AUTO_INCREMENT	PRIMARY KEY,								-- 예약 식별 번호
			`appt_date`	DATETIME	NOT NULL,									-- 예약 날짜시간
			`appt_status`	ENUM('waiting', 'accepted', 'rejected', 'complete')	NOT NULL	DEFAULT 'waiting',	-- 예약 상태(대기, 승인, 거절, 진료완료)
			`appt_symptom`	VARCHAR(50)	NULL,										-- 예약 증상
			`user_no`	INT	NOT NULL REFERENCES `user`(`user_no`),							-- 유저 식별 번호
			`hosp_no`	INT	NOT NULL REFERENCES hospital(`hosp_no`),						-- 병원 식별 번호
			`doctor_no`	INT	NOT NULL REFERENCES doctor(`doctor_no`),						-- 의사 식별 번호
			`guard_ID` VARCHAR(20),												-- 보호자 ID
			`ward_ID` VARCHAR(20)												-- 피보호자 ID
		);
		
		
		# 진료기록 테이블
		CREATE TABLE `medical_record` (
			`record_no`	INT	AUTO_INCREMENT PRIMARY KEY,								-- 진료기록 식별 번호
			`record_diagnosis`	VARCHAR(100)	NULL,									-- 진료 진단 내용
			`record_treatment`	VARCHAR(100)	NULL,									-- 진료 치료 내용
			`appt_no`	INT	NOT NULL UNIQUE REFERENCES `appointment`(`appt_no`)					-- 예약 식별 번호
		);
		
		
		
		# 거절 사유 테이블
		CREATE TABLE `rejection`(
		   `rejection_no` INT	AUTO_INCREMENT PRIMARY KEY,		            	-- 거절 사유 식별 번호
			`rejection_result` VARCHAR(100) NOT NULL,				-- 거절 이유
			`appt_no` INT NOT NULL REFERENCES `appointment`(`appt_no`)		-- 예약 식별 번호
		);
		
		
		# 의사 스케줄  테이블
		CREATE TABLE schedule(
			schedule_no INT AUTO_INCREMENT PRIMARY KEY,				-- 스케줄 식별 번호
		   schedule_time DATETIME NOT NULL,						-- 스케줄 시간목록
		   schedule_onactive ENUM('active', 'deactive') NOT NULL DEFAULT 'active',	-- 예약 가능여부
		   doctor_no INT NOT NULL REFERENCES doctor(doctor_no)				-- 의사 식별번호
		);
		
		
		# 병원 점심시간 테이블
		CREATE TABLE launch(
			launch_no INT AUTO_INCREMENT PRIMARY KEY,				-- 점심시간 식별 번호
		   launch_start TIME NOT NULL,							-- 점심 시작 시간
		   launch_end TIME NOT NULL,							-- 점심 정료시간
		   hosp_no INT NOT NULL REFERENCES hospital(hosp_no)				-- 병원 식별번호
		);
  ```
  </div>
  </details>

---

## 📃 테스트 케이스
### 1. 테스트 케이스 문서
[테스트케이스 문서 바로가기](https://docs.google.com/spreadsheets/d/1DoapmkEKpczNL8F6J-lcrC_b0urFBHJlQDxQQlvPRBc/edit#gid=0)

![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/822f16ce-7bf4-4703-8579-3cbb70888abc)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/e7fe6906-238e-4061-98b6-f0914f2883c6)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/1602fca4-aeba-4111-a2ca-6420aafb5a7f)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/081b978d-a87e-4f7a-89bd-b953d3ef19ff)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/bfbd0636-1f8b-4bcb-b769-5b74241fe35b)


### 2. 테스트 데이터
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

---

## 🔑 주요 쿼리


  ### 1. 회원
  <details>
  <summary>회원 가입</summary>
  <div>

  ```sql
  INSERT INTO user(user_id, user_pwd, user_name, user_birthdate, user_addr, user_phone)
  VALUE('user11', 'password11', '이장선', '1996-06-03', '경기도김포시 장기동 123-43', '01099487826');

  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/dba7ccd2-5015-46b3-bc24-bcead2f37d82)
  </div>
  </details>
  <details>
  <summary>회원 탈퇴</summary>
  <div>

  ```sql
  UPDATE user
  SET user_secession = 'deactivate',
      secession_date = NOW()
  WHERE user_no = 11;
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/92adac65-f0ae-424e-8a71-06693978dc94)

  </div>
  </details>
  
  ### 2. 병원 
  <details>
  <summary>병원 가입</summary>
  <div>

  ```sql
  INSERT INTO hospital(hosp_id, hosp_pwd, hosp_name, hosp_phone)
  VALUES('hosp11', 'password11', '바른 병원', '01011223345');
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/28ad5a2f-1bfd-4c89-9efd-e37f323ed6e6)

  </div>
  </details>
  <details>
  <summary>병원 탈퇴</summary>
  <div>

  ```sql
  UPDATE hospital
  SET hosp_secession = 'deactivate',
      secession_date = NOW()
  WHERE hosp_no = 11;
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/a632201b-40dc-454e-bc01-8a59087bb7ae)

  </div>
  </details>

  <details>
  <summary> 의사 스케줄 </summary>
  <div>
      <p align="center">
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/0910f3fc-4b46-4968-b307-1809f2039b99" alt="Description of first image" width="300"/>
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/ccaed4d3-bcc1-403a-aa5b-266084773362" alt="Description of second image" width="300"/>
    </p>

 
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
	https://github.com/beyond-sw-camp/be08-1st-primary-findoc/blob/main/README.md
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


  </div>
  </details>

  ### 3. 병원 검색 
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/51b6fe1e-1914-44cd-87d3-54e74e7d59a8)
  </div>
  </details>

  ### 4. 병원 예약 
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/45ceab65-5430-4d9c-b5ef-bbdcfcdae6e5)
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/44601597-23e3-473c-ad1f-c0c7016179e0)
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/aa23006f-8840-4522-8495-1c0f56575784)
  </div>
  </details>

  ### 5. 예약 취소 
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/5985d2fc-14a1-4d29-99b2-689f0a1f3f26)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/cc285ea9-61dc-49e8-89f1-c8861484ee86)
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
  CALL CancelAppointmentByGuardian(6, 'user06', 'password6', 1, 9);
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/6a95345e-1384-4190-8a79-73f2577c678c)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/d7792eaf-1712-443a-88ac-2798f19acb34)
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/aebaa8a5-71e4-41bf-986d-733d37568828)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/74a4f2c3-4a01-4455-a3ea-76da633b66f5)
  </div>
  </details>

  ### 6. 예약 변경
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/78b09603-67f5-46b7-88ad-a32fb2b3b0d4)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/d930cb68-8763-4763-a3fa-0d6e1d120588)
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/76814e3c-cf25-419e-98ee-22ba80e46bc6)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/cd5ca216-f597-41ec-baa1-99f9c97f6572)
  </div>
  </details>

  ### 7. 예약 수락 / 거절
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/03e64a62-39a7-49b0-87ce-3a9fdd2ee2d4)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/c5e3be0f-a02b-420a-b181-bdbc154c494d)
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/d920de2b-538b-4323-af2e-473d66f77fcc)
  </div>
  </details>

### 8. 진료기록
  <details>
  <summary>회원이 진료 기록 확인</summary>
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
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/ec52be40-6351-4a59-8f24-f3cacf108119)
  </div>
  </details>
  <details>
  <summary>병원이 회원의 진료 기록 저장</summary>
  <div>
   
  * 병원이 진료가 완료된 진료 정보 저장
  ```sql
  INSERT INTO medical_record(record_diagnosis, record_treatment, appt_no, hosp_no, user_no, docdept_no)
  VALUES('위염', '약 처방', 8, 1, 6, 1);
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/63641939/faf3383c-417a-4760-934c-06589b0e8c42)
  </div>
  </details>

### 9. 보호자 / 피보호자 설정 
  <details>
  <summary>보호자 신청</summary>
  <div>
   
  * 보호자 회원 번호, 피보호자 회원 번호, 관계를 입력 받아 보호자 신청 
  ```sql

  INSERT INTO guardian(guard_no, ward_no, guard_relationship)
  VALUES(1, 2, '부모');
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/1468c121-3f4e-47db-851b-0e7c666a8294)

  </div>
  </details>
  <details>
  <summary>보호자 신청 수락</summary>
  <div>
   
  * 보호자 허가 상태를 completed로 변경  
  ```sql

  UPDATE guardian
  SET guard_allowed = 'completed'
  WHERE guard_no = 1 AND ward_no = 2;
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/c30a22c8-e574-4ab7-89a3-5152439a1623)

  </div>
  </details>
  <details>
  <summary>보호자 신청 거절</summary>
  <div>
   
  * 보호자 허가 상태를 rejected로 변경 
  ```sql

  UPDATE guardian
  SET guard_allowed = 'rejected'
  WHERE guard_no = 1 AND ward_no = 2;
  ```
![image](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/80452437/a180ac2c-4544-4511-be43-4c53be77f655)

  </div>
  </details>
  
---

## 👫 CO-OP
### 회고
|팀원|회고록|
|-----|-----|
|김도하|배웠던 내용을 프로젝트를 통해 구현해 볼 수 있어서 매우 좋은 경험이 되었습니다. 팀원들이 각자의 역할을 잘 수행하고 많은 회의를 통해 서로의 의견을 공유하면서 원하는 방향의 결과물을 낼 수 있었습니다. 당분간 다른 프로젝트를 진행하면서 다른 팀원을 만나고 다른 프로젝트를 진행하게 될 텐데 첫 프로젝트를 잘 마무리하게 되어 앞으로의 프로젝트들도 많이 기대가 됩니다.|
|김은경|DB로만 프로젝트를 한다는 것에 쉽게 생각했었지만, 주제 선정부터 테이블 설계와 연관관계 설정까지 고민할 부분이 많았습니다. db에 초점을 맞춰서 프로젝트를 진행할 수 있어서 다양한 SQL 질의문을 적용해 볼 수 있었습니다. 특히 이번 프로젝트를 통해서 DB 자체에서 스케줄러를 구현해 볼 수 있었고 DB 단에서 성능을 향상할 수 있는 부분이 무엇인지에 대해서 고민할 수 있는 시간을 가질 수 있었습니다.|
|이지정|팀프로젝트도 처음이었고 데이터베이스에 대해 아는 것이 많이 없는데 처음부터 끝까지 설계를 해야된다는 점에서 걱정이 많았습니다. 그래서 이번 프로젝트를 진행하면서 조금이라도 더 참여해 배워가는 부분이 많았으면 좋겠다고 생각했는데, 팀원들이 잘 이끌어준 덕분에 프로젝트를 통해서 많은 것을 배워가는 시간이 되었던 것 같습니다. 이번 경험으로 팀 프로젝트에 대해 느낀 것이 많고 다음에는 더 잘하고 싶다는 욕심이 생겼습니다. |
|황지우|서비스 아이디어를 구상하고 Maria DB, SQL을 활용해 데이터베이스를 구축하는 프로젝트를 진행했습니다. 기존에는 SQL 문법을 복습하고 단순한 쿼리를 작성해보았는데, 프로젝트에서는 서비스의 기능까지 고려해 데이터베이스를 설계하고 쿼리문을 구현해보았습니다. 예약이나 취소 기능을 구현할 때 프로시저나 트리거를 활용하는 등 SQL의 다양한 기능을 데이터에 적용해 볼 수 있었습니다. 또한 혼자서 DB를 구현하고 테스트했을 때보다 더 다양한 경험을 할 수 있었습니다. 특히 요구사항 명세와 테이블을 작성할 때 서로 생각하는 방향이 다를 수 있기 때문에 커뮤니케이션이 중요하다고 생각하게 되었습니다. 부족한 부분이 많았는데 팀원분들 덕분에 프로젝트를 잘 마무리 할 수 있었습니다.|

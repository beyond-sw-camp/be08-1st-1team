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
<details>
### DB 모델링
  <summary> 접기/펼치기 </summary>
</details>
<details>
### DDL 및 주요 쿼리
  <summary> 접기/펼치기 </summary>
  <details>
    <summary> time_interval</summary>
  </details>
  <details>
    <summary> time_interval</summary>
      <p align="center">
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/0910f3fc-4b46-4968-b307-1809f2039b99" alt="Description of first image" width="300"/>
      <img src="https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/ccaed4d3-bcc1-403a-aa5b-266084773362" alt="Description of second image" width="300"/>
    </p>




```
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
</details>

### 테스트 케이스

## 👫 CO-OP
### WBS
<details>
  <summary>접기/펼치기
    
  </summary>
  https://docs.google.com/spreadsheets/d/1hpVTMaa_74JfIQDtYtLpZEWX7O0yWWgvPrazUaNrMxc/edit#gid=1835326347
  ![wbs](https://github.com/beyond-sw-camp/be08-1st-primary-findoc/assets/96649881/6ed5b4dd-06af-4889-93bd-82d9ee2614ea)

</details>

### 회고

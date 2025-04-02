# UART 실습

### ✔️ 개발 환경
- **개발 도구**
  - STMCubeIDE 
  - Xshell 5   
- **BOM**
  - STM32F103C8T6 개발보드
  - FT232R(FTDI)
    - UART 통신 신호를 받아 USB 데이터로 전환
  - ST-LINK/V2  
    - STM32의 디버깅 위함
- **Circuit**
<img src="https://github.com/user-attachments/assets/7767d35e-aca0-4355-ac6c-4f72cd04b926" width="80%">  

- **Pinout** **&** **Configuration**
<img src="https://github.com/user-attachments/assets/8e227f02-6971-491e-be51-16c566079523" width="80%">
<img src="https://github.com/user-attachments/assets/736d46b7-f1bb-4c41-98c5-48d97108c34c" width="80%"> 

---

### ✔️ UART(Universal Asynchronous Receiver/Transmiiter)
- 비동기 직렬(Serial)통신
- Clock을 사용하지 않기 때문에 송수신측에서 사전에 baudrate, data size 등을 설정
- TX(송신), RX(수신)의 2개선 활용  

  <img src="https://github.com/user-attachments/assets/41918c0a-0b8d-49b7-abf3-bb917ce766ac" width="80%">

- 데이터 패킷 구조

  <img src="https://github.com/user-attachments/assets/d0c0a9f8-e1ad-471e-973f-4b06b2b96f3f" width="80%">
    
  - Start bit: 통신 시작을 의미하는 비트  
  - Data bit: 데이터를 표현하는 비트  
  - Parity bit: 오류 검증을 위한 비트  
  - Stop bit: 통신 종료를 의미하는 비트  

---

### ✔️ Xshell 5 설정
<img src="https://github.com/user-attachments/assets/0f39eafc-ce08-4b35-9478-3c1e52f0cf8b" width="80%">
<img src="https://github.com/user-attachments/assets/558e3850-7153-4992-90c0-ac6c241e5619" width="80%">

- 장치관리자 - 포트에서 확인한 USB Serial Port와 CubeIDE에서 설정한 Configuration을 바탕으로 새 세션 등록

---

### ✔️ 코드 분석
- UART
  ```c
  // UART 전송 메서드
  HAL_StatusTypeDef HAL_UART_Transmit(UART_HandleTypeDef *huart, const uint8_t *pData, uint16_t Size, uint32_t Timeout)
  ```
  - *huart: UART 모듈에 대한 구성 정보를 포함하는 구조체 포인터
  - *pData: 데이터 버퍼를 가리키는 포인터
  - Size: 전송할 데이터의 크기
  - Timeout: 데이터 전송 작업의 시간 제한 

- printf()를 활용한 UART
  ```c
  // printf() 내부의 _write()를 재정의 해서 활용
  int _write(int file, char* p, int len){
	HAL_UART_Transmit(&huart1, (uint8_t*)p, len, 10);
	return len;
  }
  ```
  - main.c에 위 코드를 입력하면 printf()의 인자로 송신하고자 하는 데이터를 가능

---

### ✔️ 결과 확인
- UART

  <img src="https://github.com/user-attachments/assets/6ded5e1c-e6a0-475e-874d-6711d97c96c7" width="80%">

- printf()를 활용한 UART

  <img src="https://github.com/user-attachments/assets/89578f18-4bca-4f76-8b5f-56f1a424d2bb" width="80%">

---
- window의 줄바꿈은 CRLF(\r\n) => \r은 커서를 헌재 줄 앞의 맨 앞으로 이동, \n 커서를 한 줄 아래로 이동 
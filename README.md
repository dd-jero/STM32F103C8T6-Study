# STM32F103C8T6 개발보드를 활용한 임베디드 실습

### ✔️ 개발 환경

1. STM32F103C8T6 개발보드
<img src="https://github.com/user-attachments/assets/ca0c47c6-9886-4312-b473-41e18b1d8e68" width=300>

2. STM32CubeIDE release v1.17.0  

3. ST-LINK/V2
---

### ✔️ 프로젝트 생성

1. File - STM32 Project - STM32F103C8T6 선택 
    - Help - STM32Cube updates - Connection to myST 에서 로그인을 해야 정상적으로 프로젝트 생성됨!
---

### ✔️ 첫 디버깅 시

<img src="https://github.com/user-attachments/assets/7c2c0bf2-b967-4bfd-a7d8-032a24f29479" width=500> 

---

### ✔️ ST-LINK 펌웨어 업데이트 

1. 첫번째 방법: 실패 
    - https://www.st.com/en/development-tools/stsw-link004.html - software 다운 - 실행

    - Target - Connect
    <img src="https://github.com/user-attachments/assets/cd488cf3-1d50-4d9a-b0a6-f85981e6f15d" width=500>

    - ST-LINK - Firmware Update (아래와 같이 뜨면 USB 뽑았다가 다시 연결)
    <img src="https://github.com/user-attachments/assets/528db62f-b7a8-4227-9f0e-6dad5780fd5d" width=500>

    - 다시 연결하면 연결됨 - Yes 누르면 업데이트 완료! (STMCubeIDE 재실행 하면 적용됨)
    <img src="https://github.com/user-attachments/assets/f910f9d8-86fd-47e3-9409-55ff4c19c6aa" width=500>

2. 두번째 방법: 성공

    - Yes - Open in update mode 
    <img src="https://github.com/user-attachments/assets/7c2c0bf2-b967-4bfd-a7d8-032a24f29479" width=500>

    - "ST-Link is not in the DFU mode. Please restart it." 뜨면 USB 재연결 - Open in update mode 다시 클릭 (그래도 이상하면 Refresh device list 클릭)
    <img src="https://github.com/user-attachments/assets/722a3165-fe6d-4be6-b6b0-db4c3f6acc91" width=500>
    <img src="https://github.com/user-attachments/assets/d67e0647-5070-4547-a9de-e678b2f94bde" width=500>

    - 위 Upgrade 끝나면 STMCubeIDE 재실행. 정상적으로 breakpoint에 걸리는 것을 확인할 수 있음.
    <img src="https://github.com/user-attachments/assets/19935352-ca3f-46ba-b529-0dfafc464d64" width=500>
--- 

### ✔️ pin setting 

- ioc 파일 - System Core - SYS - Debug 항목의 Serial Wire 선택 - Ctrl + S로 저장 
- Serial Wire는 2-pin만 사용해서 pin의 사용을 최소화할 수 있음.

<img src="https://github.com/user-attachments/assets/973ce044-6189-45e0-ad91-6b1dde8d62c0" width=500>

---

### ✔️ 간단한 LED 제어

- PC13 pin을 GPIO_Output으로 setting

<img src="https://github.com/user-attachments/assets/9e4b1fbd-6b1e-4379-83f0-1374260c79ec" width=500>
<img src="https://github.com/user-attachments/assets/2eb79c47-a1cb-4160-bfc0-a588e1385c10" width=300>

- main.c에 MX_GPIO_Init(); 생성됨  
- User label을 GPIO_LED로 지정해서 main.h에 자동 define됨

<img src="https://github.com/user-attachments/assets/73b531bd-4ad3-477d-a921-19b3a564a9b5" width=300>

```c
// main.c에 개발 보드 상의 D2 점멸 코드 
/* USER CODE BEGIN WHILE */
while (1)
  {
	  HAL_GPIO_WritePin(GPIO_LED_GPIO_Port, GPIO_LED_Pin, 1);
	  HAL_Delay(100);
	  HAL_GPIO_WritePin(GPIO_LED_GPIO_Port, GPIO_LED_Pin, 0);
	  HAL_Delay(100);
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
```
---

### ✔️ 사용자 입력으로 LED 제어

- 이어서 PA0 pin을 GPIO_Input으로 setting
- S2 버튼을 누를 때마다 LED(D2) 켜짐!

<img src="https://github.com/user-attachments/assets/874866ae-fd76-409c-adf0-80640f953d15" width=500>
<img src="https://github.com/user-attachments/assets/25a65b61-8e25-4cff-9b93-fac7679eaa93" width=300>

```c
  /* USER CODE BEGIN WHILE */
  while (1)
  {
	  if(!HAL_GPIO_ReadPin(GPIO_SW_GPIO_Port, GPIO_SW_Pin)){
		  HAL_GPIO_WritePin(GPIO_LED_GPIO_Port, GPIO_LED_Pin, 0);
	  }else{
		  HAL_GPIO_WritePin(GPIO_LED_GPIO_Port, GPIO_LED_Pin, 1);
	  }

	  HAL_Delay(100);
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
```

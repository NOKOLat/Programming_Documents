# 7. USBメモリ(上級編)
## 目標
- USBメモリにログを取れるようになる

## CubeMXの設定
USBメモリ(初級編)に加えて`System Core`の`NVIC`で`EXTI line[15:10] interrupts`にチェックを入れる

## 期待される動作
ヌクレオボード上のボタン(F411だと青いボタン)を押した回数をUSBメモリ上のファイルに書き込む

## プログラミング

## サンプルコード
### USBH_UserProcess
```c++
/* USER CODE BEGIN Includes */
# include "fatfs.h"
/* USER CODE END Includes */
```

```c++
static void USBH_UserProcess  (USBH_HandleTypeDef *phost, uint8_t id)
{
  /* USER CODE BEGIN CALL_BACK_1 */
  switch(id)
  {
  case HOST_USER_SELECT_CONFIGURATION:
  break;

  case HOST_USER_DISCONNECTION:
  Appli_state = APPLICATION_DISCONNECT;
  break;

  case HOST_USER_CLASS_ACTIVE:
  Appli_state = APPLICATION_READY;
  f_mount(&USBHFatFS, (TCHAR *)USBHPath, 0);
  f_open(&USBHFile, "test.txt", FA_CREATE_ALWAYS | FA_WRITE);
  break;

  case HOST_USER_CONNECTION:
  Appli_state = APPLICATION_START;
  break;

  default:
  break;
  }
  /* USER CODE END CALL_BACK_1 */
}
```

### wrapper.cpp
```c++
#include <fatfs.h>
#include <usb_host.h>
#include "WRAPPER.hpp"

uint16_t pushCount=0;
uint8_t pushFlg=0;
UINT bw;

void loop(void){
	if(pushFlg){
		f_printf(&USBHFile, "%u",pushCount);
	   	f_sync(&USBHFile);
       	pushFlg--;
	}
}

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
	pushCount++;
	pushFlg++;
}

```

### main.c
```c++
 /* USER CODE BEGIN 2 */

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
    MX_USB_HOST_Process();

    /* USER CODE BEGIN 3 */
    loop();
  }
  /* USER CODE END 3 */
```
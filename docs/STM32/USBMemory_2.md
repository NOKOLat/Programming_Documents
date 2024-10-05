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

uint8_t pushCount=0x30;
uint8_t pushFlg=0;
char insertText[2];
UINT bw;

void loop(void){
	if(pushFlg){
		insertText[0]=pushCount;
	   	insertText[1]='\n';
	   	f_open(&USBHFile, "/test.txt", FA_OPEN_APPEND|FA_WRITE);
	   	f_write(&USBHFile, &insertText, sizeof(insertText), &bw);
	    f_close(&USBHFile);
       	pushFlg--;
	}
}

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
	pushCount++;
	pushFlg++;
}
```
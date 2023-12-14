# 5. I2C
I2C通信の解説については[こちら](../../communication/)をご覧ください
## 目標
- I2C通信の使い方を習得する

## CubeMXの設定
1. 使用するピンを設定する
>PB8およびPB9をそれぞれSCL, SDAに設定する
>![](_res/I2C_Pinout.png)
2. パラメータを設定する
>`primary slave address`の項目を任意の値$x$に設定する．
>$$ x \in\mathbb{R}| 1<x<255,  x\bmod 2 = 0$$
>![](_res/I2C_config.png)
3. 通信するピンのGPIO設定
>2つのピンを両方ともPull-Upに設定した．  
>**CATION:通信相手や回路によってはデバイスの故障につながるので注意してください．**
>![](_res/I2C_gpioConfig.png)
## Source Code
```c++
#include "wrapper.hpp"

#include <string>
#include <array>

#include <usart.h>
#include <gpio.h>
#include <i2c.h>

#define MASTER
//#define SLAVE

uint16_t number = 0;
const uint8_t slaveAddress = 0;

void init(void){
	HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
	HAL_Delay(1000);
	HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);
#ifdef MASTER
	std::string str = "this is master\n";
#endif
#ifdef SLAVE
	std::string str = "this is slave\n";
#endif
	HAL_UART_Transmit(&huart2, (uint8_t *)str.c_str(), str.length(), 100);
}

void loop(void){
#ifdef MASTER
	std::string str = "transmit : " + std::to_string(number) + "\n";
	HAL_I2C_Master_Transmit(&hi2c1, slaveAddress<<1, (uint8_t *)&number, sizeof(number), 100);
    HAL_Delay(500);
	number += 1;
#endif
#ifdef SLAVE
	HAL_I2C_Slave_Receive(&hi2c1, slaveAddress, (uint8_t *)&number, sizeof(number), 1000);
	std::string str = "transmit : " + std::to_string(number) + "\n";
#endif
    HAL_UART_Transmit(&huart2, (uint8_t *)str.c_str(), str.length(), 100);

}
```

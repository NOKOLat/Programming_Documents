# LEDの点滅

# スイッチと同期したLEDの点灯

# source code
## LEDの点滅
```c++
void setup() {
  // put your setup code here, to run once:
  pinMode(13, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(13, HIGH);
  delay(200);
  digitalWrite(13, LOW);
  delay(200);
}
```
## スイッチと同期したLEDの点灯
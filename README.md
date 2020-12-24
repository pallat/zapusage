# วิธีใช้ uber/zap

## ติดตั้ง

> go get -u go.uber.org/zap

## zap มี 2 types

- Logger
- Sugar Logger

ต่างกันตรงที่ Sugar Logger จะทำ format ได้แบบเดียวกับ Printf
ส่วน Logger จะเข้มงวดเรื่อง type ทำให้ความเร็วจะสูงกว่า

## new ได้ 3 แบบ

- zap.NewProduction()
    แบบ json
    ไม่ log ระดับ debug
    print stack trace ที่ Error กับ DPanic แต่ไม่ทำที่ Warn
    มี caller พร้อมบรรทัด
    มี timestamp แบบ epoch
- zap.NewDevelopment()
    แบบ console อ่านง่าย
    print stack trace ตั้งแต่ level Warn ขึ้นไป
    มี caller พร้อมบรรทัด
    มี timestamp
- zap.Example()
    มีแค่ level กับ message แบบ json

## ใส่ lumberjack เพื่อทำ rotate log ได้

ถ้าต้องการ log ลงไฟล์ตรงๆ แนะนำใช้กับ lumberjack ไปเลย

> go get -u github.com/natefinch/lumberjack

```go
rotation := &lumberjack.Logger{
    Filename: "./app.log",
    MaxSize: 5,
    MaxBackups: 3,
    MaxAge: 7,
    Compress: true,
}
zapcore.AddSync(rotation)
```

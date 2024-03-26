# 自作部分
SMS_STS.h  
69 virtual int WriteTorque(u8 ID, s16 Torque, u8 ACC = 0);//トルク制御  
70 virtual void SyncWriteSpe(u8 ID[], u8 IDN, s16 Speed[], u8 ACC[]);//同期速度制御

SMS_STS.cppの対応箇所(WriteTorque,SyncWriteSpe)

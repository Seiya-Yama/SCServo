# 自作部分
SMS_STS.h  
69 virtual int WriteTorque(u8 ID, s16 Torque, u8 ACC = 0);//トルク制御  
70 virtual void SyncWriteSpe(u8 ID[], u8 IDN, s16 Speed[], u8 ACC[]);//同期速度制御  

SMS_STS.cpp  
int SMS_STS::WriteTorque(u8 ID, s16 Torque, u8 ACC)
{
	if(Torque<0){
		Torque = -Torque;
		Torque |= (1<<15);
	}
	u8 bBuf[2];
	bBuf[0] = ACC;
	genWrite(ID, SMS_STS_ACC, bBuf, 1);
	Host2SCS(bBuf+0, bBuf+1, Torque);
	
	return genWrite(ID, SMS_STS_GOAL_TIME_L, bBuf, 2);
}

void SMS_STS::SyncWriteSpe(u8 ID[], u8 IDN, s16 Speed[], u8 ACC[])
{
    u8 offbuf[7*IDN];
    for(u8 i = 0; i<IDN; i++){
        s16 V;
        if(Speed[i]<0){
            Speed[i] = -Speed[i];
            Speed[i] |= (1<<15);
            V = Speed[i];
        }else{
            V = Speed[i];
        }
        if(ACC){
            offbuf[i*7] = ACC[i];
        }else{
            offbuf[i*7] = 0;
        }
        Host2SCS(offbuf+i*7+3, offbuf+i*7+4, 0);
        Host2SCS(offbuf+i*7+5, offbuf+i*7+6, V);
    }
    syncWrite(ID, IDN, SMS_STS_ACC, offbuf, 7);
}

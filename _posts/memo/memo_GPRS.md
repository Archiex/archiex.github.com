---
layout: post
title: ·��GPRS��������
category: memo
description: �й�·��GPRS�Ĳ��������ļ�¼�ļ���
---
# [{{ page.title }}][1]
2012-01-16 By {{ site.author_info }}

##NetStatus == 0
	TurnOnGSM:
		if(GPRS_STATUS_Pin == 1)
			TurnOffGSM 
				C7clear-delay500-C7set-delay1000-C7clear-delay2000
				AT+CPOWD=1	// ���״̬�޷��ػ�
				delay2000-C5clear(pwrChipCtrl)
		C5set(pwrChip)-delay500-C7clear-delay500-C7set-delay2300-C7clear-delay4000

	FOR 3	AT\r\n
	AT+ATE1\r\n	delay500
	AT+ATE0\r\n	delay500
	AT+ATE0\r\n	delay500

	AT+CCID\r\n
	AT+CGSN\r\n

	SMS_Init
		AT+CSCS="GSM"\r\n
		AT+CMGF=1\r\n
		AT+CNMI=2,1,0,0,0\r\n
	AT+CIPMODE=1\r\n		// ʹ��͸��ģʽ
	AT+CIPCSGP=1,"UNINET"\r\n
	AT+CIPHEAD=1\r\n	=> \r\nOK\r\n
	AT+CIPSRIP=0\r\n
	AT+CIPHEAD=1\r\n
	AT+CIPSPRT=2\r\n

	AT+CMGDA="DEL ALL"\r\n => \r\nOK\r\n

	FOR 100
		AT+CBC\r\n
		AT+CREG?\r\n	=> \r\n+CREG:0,1\r\n \r\nOK\r\n
			==> SUCCESS: NetStatus = 1
			==> FAILURE: TurnOffGSM-NetStatus = 0

##NetStatus == 1
	AT+CMGL="ALL"\r\n
		==> SUCCESS: AT+CMGDA="DEL ALL"\r\n
	AT+CBC\r\n
	AT+CSQ\r\n
	AT+CGATT=1\r\n
		delay500
	AT+CSTT="UNINET"\r\n
		delay1500
	FOR 2
		AT+CBC\r\n
		AT+CSQ\r\n
			delay500
		AT+CBC\r\n
		AT+CIPSHUT\r\n
			delay500
		AT+CDNSGIP="XY.DEV.NET"\r\n		// �˴��ܷ���ERROR����ֱ���������������ܳɹ������
			// ����һ����DNS���ӶϿ�������NetStatus=0��ʼ����
		AT+CIPSTART="TCP","XY.DEV.NET",3800\r\n
		�� AT+CIPSTART="TCP","124.187.33.20",3800\r\n
			==> SUCCESS: NetStatus = 2
			==> FAILURE: if Retry<20 NetStatus = 1 else NetStatus = 0

##NetStatus == 2
	// ע��ɹ�
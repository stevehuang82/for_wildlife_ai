1. allon_sensor_tflm_freertos : dpd mode sample code
sleep_mode.c : call app_pmu_enter_dpd() to enter dpd mode and wake up via PA0

2. we2_image_gen_local_dpd : bootloader for dpd mode
execute following command to generate dpd mode firmware image

./we2_local_image_gen project_case1_blp_wlcsp_rc24m.json

Please make sure bootloader message as below.

1st BL Modem Build DATE=Jan  7 2025, Version: 2.12

20250306
1. allon_sensor_tflm_freertos :
   a. Fine-tune the HM0360 motion detection sensitivity setting
   b. Support RTC timer in dpd mode
   c. Create SD card image folder names by time
2. add \doc\HM0360_Motion_Detection_Setting_20250306.pdf

20250408
1. Configure HM0360 to 640x480 10 fps(Context A) when WE2 is in all-on mode.
2. Configure HM0360 to 320x240 2 fps (Context B) when WE2 is in DPD mode to save power.
3. User can fine-tune the sleep time counter in the file ~\cis_hm0360\cisdp_sensor.c to control the frame rate of HM0360.
	HX_CIS_SensorSetting_t  HM0360_md_stream_on[] = {
			{HX_CIS_I2C_Action_W, 0x3024, 0x01},	// select context B
			{HX_CIS_I2C_Action_W, 0x3029, 0x40},	// 2fps sleep count H
			{HX_CIS_I2C_Action_W, 0x302A, 0x20},	// 2fps sleep count L
			{HX_CIS_I2C_Action_W, 0x3510, 0x00},	// disable parallel output
			{HX_CIS_I2C_Action_W, 0x0100, 0x02},
	};

20250423
1. Change the motion detection interrupt to level mode and read HM0360_INT_INDC_REG bit3 to determine whether WE2 is awakened by HM0360 or BLE_WAKE.
2. After HM0360 enters the motion detection I2C low-speed mode, WE2 needs to change the I2C master clock to 100K hz low-speed mode to avoid HM0360 register reading errors.
   hx_drv_i2cm_init(USE_DW_IIC_1, HX_I2C_HOST_MST_1_BASE, DW_IIC_SPEED_STANDARD);
3. Initialize all HM0360 registers during cold boot, and only set context A or B registers during warm boot.


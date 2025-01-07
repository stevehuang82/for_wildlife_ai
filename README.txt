1. allon_sensor_tflm_freertos : dpd mode sample code
sleep_mode.c : call app_pmu_enter_dpd() to enter dpd mode and wake up via PA0

2. we2_image_gen_local_dpd : bootloader for dpd mode
execute following command to generate dpd mode firmware image

./we2_local_image_gen project_case1_blp_wlcsp_rc24m.json

Please make sure bootloader message as below.

1st BL Modem Build DATE=Jan  7 2025, Version: 2.12

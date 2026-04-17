
# AT32-硬件crc16

```c
void wk_crc_init(void)
{
	crm_periph_clock_enable(CRM_CRC_PERIPH_CLOCK, TRUE);
	crc_init_data_set(0xFFFF);
	crc_poly_size_set(CRC_POLY_SIZE_16B);
	crc_poly_value_set(0x8005);
	crc_reverse_input_data_set(CRC_REVERSE_INPUT_BY_BYTE);
	crc_reverse_output_data_set(CRC_REVERSE_OUTPUT_DATA);
	crc_data_reset();
}


uitn16_t crc16cal(uint8_t *buf, uint16_t len)
{
	uint16_t i = 0;
	crc_init_data_set(0xFFFF);
	crc_data_reset();
	
	for (i = 0; i < len; i++)
	{
		(*(uint8_t *)&CRC->dt) = buf[i];
	}
	
	return CRC->dt;
}


```
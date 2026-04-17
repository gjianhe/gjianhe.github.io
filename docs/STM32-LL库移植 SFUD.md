# STM32-LL库移植 SFUD

```c
/*
 * This file is part of the Serial Flash Universal Driver Library.
 *
 * Copyright (c) 2016-2018, Armink, <armink.ztl@gmail.com>
 *
 * Permission is hereby granted, free of charge, to any person obtaining
 * a copy of this software and associated documentation files (the
 * 'Software'), to deal in the Software without restriction, including
 * without limitation the rights to use, copy, modify, merge, publish,
 * distribute, sublicense, and/or sell copies of the Software, and to
 * permit persons to whom the Software is furnished to do so, subject to
 * the following conditions:
 *
 * The above copyright notice and this permission notice shall be
 * included in all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
 * EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
 * MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
 * IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
 * CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
 * TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
 * SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 *
 * Function: Portable interface for each platform.
 * Created on: 2016-04-23
 */

#include <sfud.h>
#include <stdarg.h>
#include "main.h"

static char log_buf[256];

void sfud_log_debug(const char *file, const long line, const char *format, ...);


typedef struct
{
    SPI_TypeDef *spix;
    GPIO_TypeDef *cs_gpiox;
    uint16_t cs_gpio_pin;
} spi_user_data, *spi_user_data_t;


static void rcc_configuration(spi_user_data_t spi)
{
    if (spi->spix == SPI1)
    {
        LL_APB2_GRP1_EnableClock(LL_APB2_GRP1_PERIPH_SPI1);
        LL_APB2_GRP1_EnableClock(LL_APB2_GRP1_PERIPH_GPIOB);
    }
    else if (spi->spix == SPI2)
    {
    }
    else
    {
    }
}

static void gpio_configuration(spi_user_data_t spi)
{
    LL_GPIO_InitTypeDef GPIO_InitStruct = {0};

    if (spi->spix == SPI1)
    {
        /**SPI1 GPIO Configuration
        PB3   ------> SPI1_SCK
        PB4   ------> SPI1_MISO
        PB5   ------> SPI1_MOSI
        */
        GPIO_InitStruct.Pin = LL_GPIO_PIN_3|LL_GPIO_PIN_5;
        GPIO_InitStruct.Mode = LL_GPIO_MODE_ALTERNATE;
        GPIO_InitStruct.Speed = LL_GPIO_SPEED_FREQ_HIGH;
        GPIO_InitStruct.OutputType = LL_GPIO_OUTPUT_PUSHPULL;
        LL_GPIO_Init(GPIOB, &GPIO_InitStruct);
        GPIO_InitStruct.Pin = LL_GPIO_PIN_4;
        GPIO_InitStruct.Mode = LL_GPIO_MODE_FLOATING;
        LL_GPIO_Init(GPIOB, &GPIO_InitStruct);
        LL_GPIO_AF_EnableRemap_SPI1();
        /* CS: PB0 */
        LL_GPIO_SetOutputPin(GPIOB, LL_GPIO_PIN_0);
        GPIO_InitStruct.Pin = LL_GPIO_PIN_0;
        GPIO_InitStruct.Mode = LL_GPIO_MODE_OUTPUT;
        GPIO_InitStruct.Speed = LL_GPIO_SPEED_FREQ_HIGH;
        GPIO_InitStruct.OutputType = LL_GPIO_OUTPUT_PUSHPULL;
        GPIO_InitStruct.Pull = LL_GPIO_PULL_UP;
        LL_GPIO_Init(GPIOB, &GPIO_InitStruct);
    }
    else if (spi->spix == SPI2)
    {
        /* you can add SPI2 code here */
    }
    else
    {
    }
}

static void spi_configuration(spi_user_data_t spi)
{
    LL_SPI_InitTypeDef SPI_InitStruct = {0};
    
    if (spi->spix == SPI1)
    {
        
        SPI_InitStruct.TransferDirection = LL_SPI_FULL_DUPLEX;
        SPI_InitStruct.Mode = LL_SPI_MODE_MASTER;
        SPI_InitStruct.DataWidth = LL_SPI_DATAWIDTH_8BIT;
        SPI_InitStruct.ClockPolarity = LL_SPI_POLARITY_LOW;
        SPI_InitStruct.ClockPhase = LL_SPI_PHASE_1EDGE;
        SPI_InitStruct.NSS = LL_SPI_NSS_SOFT;
        SPI_InitStruct.BaudRate = LL_SPI_BAUDRATEPRESCALER_DIV4;
        SPI_InitStruct.BitOrder = LL_SPI_MSB_FIRST;
        SPI_InitStruct.CRCCalculation = LL_SPI_CRCCALCULATION_DISABLE;
        SPI_InitStruct.CRCPoly = 10;
        LL_SPI_Init(SPI1, &SPI_InitStruct);
        LL_SPI_Enable(SPI1);
    }
}


/* about 100 microsecond delay */
static void retry_delay_100us(void)
{
    LL_uDelay(95);
}

static void spi_lock(const sfud_spi *spi)
{
    __disable_irq();
}

static void spi_unlock(const sfud_spi *spi)
{
    __enable_irq();
}

/**
 * SPI write data then read data
 */
static sfud_err spi_write_read(const sfud_spi *spi, const uint8_t *write_buf, size_t write_size, uint8_t *read_buf, size_t read_size)
{
    sfud_err result = SFUD_SUCCESS;
    uint8_t send_data, read_data;
    spi_user_data_t spi_dev = (spi_user_data_t) spi->user_data;

    if (write_size)
    {
        SFUD_ASSERT(write_buf);
    }
    
    if (read_size)
    {
        SFUD_ASSERT(read_buf);
    }

    LL_GPIO_ResetOutputPin(spi_dev->cs_gpiox, spi_dev->cs_gpio_pin);
    /* 开始读写数据 */
    for (size_t i = 0, retry_times; i < write_size + read_size; i++) {
        /* 先写缓冲区中的数据到 SPI 总线，数据写完后，再写 dummy(0xFF) 到 SPI 总线 */
        if (i < write_size)
        {
            send_data = *write_buf++;
        } else {
            send_data = SFUD_DUMMY_DATA;
        }
        /* 发送数据 */
        retry_times = 1000;
        
        while (LL_I2S_IsActiveFlag_TXE(spi_dev->spix) == RESET) {
            SFUD_RETRY_PROCESS(NULL, retry_times, result);
        }
        if (result != SFUD_SUCCESS) {
            goto exit;
        }
        LL_SPI_TransmitData8(spi_dev->spix, send_data);
        /* 接收数据 */
        retry_times = 1000;
        while (LL_I2S_IsActiveFlag_RXNE(spi_dev->spix) == RESET) {
            SFUD_RETRY_PROCESS(NULL, retry_times, result);
        }
        if (result != SFUD_SUCCESS) {
            goto exit;
        }
        read_data = LL_SPI_ReceiveData8(spi_dev->spix);
        /* 写缓冲区中的数据发完后，再读取 SPI 总线中的数据到读缓冲区 */
        if (i >= write_size) {
            *read_buf++ = read_data;
        }
    }

exit:
    LL_GPIO_SetOutputPin(spi_dev->cs_gpiox, spi_dev->cs_gpio_pin);

    return result;
}

#ifdef SFUD_USING_QSPI
/**
 * read flash data by QSPI
 */
static sfud_err qspi_read(const struct __sfud_spi *spi, uint32_t addr, sfud_qspi_read_cmd_format *qspi_read_cmd_format,
        uint8_t *read_buf, size_t read_size) {
    sfud_err result = SFUD_SUCCESS;

    /**
     * add your qspi read flash data code
     */

    return result;
}
#endif /* SFUD_USING_QSPI */

static spi_user_data spi1 = { .spix = SPI1, .cs_gpiox = GPIOB, .cs_gpio_pin = LL_GPIO_PIN_0 };

sfud_err sfud_spi_port_init(sfud_flash *flash) {
    sfud_err result = SFUD_SUCCESS;

    switch (flash->index)
    {
        case SFUD_W25QXX_DEVICE_INDEX:
        {
            /* RCC 初始化 */
            rcc_configuration(&spi1);
            /* GPIO 初始化 */
            gpio_configuration(&spi1);
            /* SPI 外设初始化 */
            spi_configuration(&spi1);
            /* 同步 Flash 移植所需的接口及数据 */
            flash->spi.wr = spi_write_read;
            flash->spi.lock = spi_lock;
            flash->spi.unlock = spi_unlock;
            flash->spi.user_data = &spi1;
            /* about 100 microsecond delay */
            flash->retry.delay = retry_delay_100us;
            /* adout 60 seconds timeout */
            flash->retry.times = 60 * 10000;

            break;
        }
    }

    /**
     * add your port spi bus and device object initialize code like this:
     * 1. rcc initialize
     * 2. gpio initialize
     * 3. spi device initialize
     * 4. flash->spi and flash->retry item initialize
     *    flash->spi.wr = spi_write_read; //Required
     *    flash->spi.qspi_read = qspi_read; //Required when QSPI mode enable
     *    flash->spi.lock = spi_lock;
     *    flash->spi.unlock = spi_unlock;
     *    flash->spi.user_data = &spix;
     *    flash->retry.delay = null;
     *    flash->retry.times = 10000; //Required
     */

    return result;
}

/**
 * This function is print debug info.
 *
 * @param file the file which has call this function
 * @param line the line number which has call this function
 * @param format output format
 * @param ... args
 */
void sfud_log_debug(const char *file, const long line, const char *format, ...) {
    va_list args;

    /* args point to the first variable parameter */
    va_start(args, format);
    printf("[SFUD](%s:%ld) ", file, line);
    /* must use vprintf to print */
    vsnprintf(log_buf, sizeof(log_buf), format, args);
    printf("%s\n", log_buf);
    va_end(args);
}

/**
 * This function is print routine info.
 *
 * @param format output format
 * @param ... args
 */
void sfud_log_info(const char *format, ...) {
    va_list args;

    /* args point to the first variable parameter */
    va_start(args, format);
    printf("[SFUD]");
    /* must use vprintf to print */
    vsnprintf(log_buf, sizeof(log_buf), format, args);
    printf("%s\n", log_buf);
    va_end(args);
}

void sfud_demo(uint32_t addr, size_t size, uint8_t *data)
{
    sfud_err result = SFUD_SUCCESS;
    sfud_init();
    const sfud_flash *flash = sfud_get_device(SFUD_W25QXX_DEVICE_INDEX);
    //const sfud_flash *flash = sfud_get_device_table() + 0;
    size_t i;

    /* prepare write data */
    for (i = 0; i < size; i++)
    {
        data[i] = i;
    }

    /* erase test */
    result = sfud_erase(flash, addr, size);

    if (result == SFUD_SUCCESS)
    {
        printf("Erase the %s flash data finish. Start from 0x%08X, size is %u.\r\n", flash->name, addr, size);
    }
    else
    {
        printf("Erase the %s flash data failed.\r\n", flash->name);
        return;
    }

    /* write test */
    result = sfud_write(flash, addr, size, data);

    if (result == SFUD_SUCCESS)
    {
        printf("Write the %s flash data finish. Start from 0x%08X, size is %u.\r\n", flash->name, addr, size);
    }
    else
    {
        printf("Write the %s flash data failed.\r\n", flash->name);
        return;
    }
    
    for (i = 0; i < size; i++)
    {
        data[i] = 0;
    }

    /* read test */
    result = sfud_read(flash, addr, size, data);

    if (result == SFUD_SUCCESS)
    {
        printf("Read the %s flash data success. Start from 0x%08X, size is %u. The data is:\r\n", flash->name, addr, size);
        printf("Offset (h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F\r\n");

        for (i = 0; i < size; i++)
        {
            if (i % 16 == 0)
            {
                printf("[%08X] ", addr + i);
            }

            printf("%02X ", data[i]);

            if (((i + 1) % 16 == 0) || i == size - 1)
            {
                printf("\r\n");
            }
        }

        printf("\r\n");
    }
    else
    {
        printf("Read the %s flash data failed.\r\n", flash->name);
    }

    /* data check */
    for (i = 0; i < size; i++)
    {
        if (data[i] != i % 256)
        {
            printf("Read and check write data has an error. Write the %s flash data failed.\r\n", flash->name);
            break;
        }
    }

    if (i == size)
    {
        printf("The %s flash test is success.\r\n", flash->name);
    }
}

```
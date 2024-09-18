# STM32 MCP2515 CAN interface library

STM32 HAL MCP2515  src code for STM32CubeIDE C++

This port was built using [arduino-mcp2515](https://github.com/autowp/arduino-mcp2515).

As IS no pull requests pls.


CAN-BUS is a common industrial bus because of its long travel distance, medium communication speed and high reliability. It is commonly found on modern machine tools and as an automotive diagnostic bus. This CAN-BUS Shield gives your Arduino/Seeeduino CAN-BUS capability. With an OBD-II converter cable added on and the OBD-II library imported, you are ready to build an onboard diagnostic device or data logger.

* Implements CAN V2.0B at up to 1 Mb/s
* SPI Interface up to 10 MHz
* Standard (11 bit) and extended (29 bit) data and remote frames
* Two receive buffers with prioritized message storage

Example how init SPI3 on stm32f4x1 cortexm4 (black pill plate) Please only:
for informational purposes only, thise auto generated in STM32CubeMX!

spi.c
```
/* SPI3 init function */
void MX_SPI3_Init(void)
{

  /* USER CODE BEGIN SPI3_Init 0 */

  /* USER CODE END SPI3_Init 0 */

  /* USER CODE BEGIN SPI3_Init 1 */

  /* USER CODE END SPI3_Init 1 */
  hspi3.Instance = SPI3;
  hspi3.Init.Mode = SPI_MODE_MASTER;
  hspi3.Init.Direction = SPI_DIRECTION_2LINES;
  hspi3.Init.DataSize = SPI_DATASIZE_8BIT;
  hspi3.Init.CLKPolarity = SPI_POLARITY_LOW;
  hspi3.Init.CLKPhase = SPI_PHASE_1EDGE;
  hspi3.Init.NSS = SPI_NSS_SOFT;
  hspi3.Init.BaudRatePrescaler = SPI_BAUDRATEPRESCALER_4;
  hspi3.Init.FirstBit = SPI_FIRSTBIT_MSB;
  hspi3.Init.TIMode = SPI_TIMODE_DISABLE;
  hspi3.Init.CRCCalculation = SPI_CRCCALCULATION_DISABLE;
  hspi3.Init.CRCPolynomial = 10;
  if (HAL_SPI_Init(&hspi3) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN SPI3_Init 2 */

  /* USER CODE END SPI3_Init 2 */

}
```
```
void HAL_SPI_MspInit(SPI_HandleTypeDef* spiHandle)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
  if(spiHandle->Instance==SPI3)
  {
  /* USER CODE BEGIN SPI3_MspInit 0 */

  /* USER CODE END SPI3_MspInit 0 */
    /* SPI3 clock enable */
    __HAL_RCC_SPI3_CLK_ENABLE();

    __HAL_RCC_GPIOB_CLK_ENABLE();
    /**SPI3 GPIO Configuration
    PB3     ------> SPI3_SCK
    PB4     ------> SPI3_MISO
    PB5     ------> SPI3_MOSI
    */
    GPIO_InitStruct.Pin = GPIO_PIN_3|GPIO_PIN_4|GPIO_PIN_5;
    GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
    GPIO_InitStruct.Alternate = GPIO_AF6_SPI3;
    HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

  /* USER CODE BEGIN SPI3_MspInit 1 */

  /* USER CODE END SPI3_MspInit 1 */
  }
}
```

main.cpp (c++) - this initial code an real project MAZDA_CAN
```
MCP2515 ms_mcp2515(&hspi3, 10, CAN_CS_GPIO_Port, CAN_CS_Pin);

void initMCP2115() {
  ms_mcp2515.reset();
  ms_mcp2515.setBitrate(CAN_125KBPS, MCP_8MHZ);
  ms_mcp2515.setFilterMask(MCP2515::MASK(0), 0, 0x7FF);
  ms_mcp2515.setFilterMask(MCP2515::MASK(1), 0, 0x7FF);
  ms_mcp2515.setFilter(MCP2515::RXF(0), 0, 0x401); // 
  ms_mcp2515.setFilter(MCP2515::RXF(1), 0, 0x423); // 
  ms_mcp2515.setFilter(MCP2515::RXF(3), 0, 0x3BC); // 
  ms_mcp2515.setFilter(MCP2515::RXF(2), 0, 0x73B); // 
  ms_mcp2515.setFilter(MCP2515::RXF(4), 0, 0x39E); // 
  ms_mcp2515.setFilter(MCP2515::RXF(5), 0, 0x728); // 
  ms_mcp2515.setNormalMode();
}
```
№All functions for working with CAN are similar to [arduino-mcp2515](https://github.com/autowp/arduino-mcp2515).

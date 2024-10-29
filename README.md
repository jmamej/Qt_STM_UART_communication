# Qt app that transmits and receives data to and from STM32 through UART

This approach allows you to send commands from Qt to STM32 and receive information/ feedback.


![image](https://github.com/user-attachments/assets/bfa28933-da43-451e-bc96-276aa4e7d5b3)

*Qt app and STM32 variables showing full duplex communication*

 ### Simplified STM32 code:

```
uint8_t uart_tx;
uint8_t uart_rx;

int main(void)
{
  MX_USART2_UART_Init();
  HAL_UART_Receive_IT(&huart2, &uart_rx, 1);

  while (1)
  {

  }
}

void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
	if(GPIO_Pin == B1_Pin) {
		uart_tx++;
		HAL_UART_Transmit(&huart2, &uart_tx, 1, 100);
	}
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
	if(huart == &huart2) {
		HAL_UART_Receive_IT(&huart2, &uart_rx, 1);
	}
}
```

*In this instance interrupt caused by a button increments uart_tx and sends it via uart2. Receiving uart_rx happens inside uart's rx callback.*


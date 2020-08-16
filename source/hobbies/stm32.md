# STM32系のtips

## Boot pinの取り扱い
- ちゃんとpullup/downしておかないとハマる

- BOOT0/BOOT1の選択表は下記
  - http://miqn.net/introduction/48.html
  - http://stm32f4.web.fc2.com/RAMboot.html
- https://arcadiaplus.jp/news/64

## sprintfとかでfloatを使うとき

## HAL API Refence
- [UM1785 - Description of STM32F0 HAL and low-layer drivers](https://www.st.com/resource/en/user_manual/dm00122015-description-of-stm32f0-hal-and-lowlayer-drivers-stmicroelectronics.pdf)
- [UM1725 - Description of STM32F4 HAL and LL drivers](https://www.st.com/resource/en/user_manual/dm00105879-description-of-stm32f4-hal-and-ll-drivers-stmicroelectronics.pdf)

## Referenses
- [STM32 - STM32 Memo](https://www.minokasago.org/STM32wiki/)

## printfの出力先をuartにする
- [マイクロマウス研修（kora編）[23] STM32マイコンでシリアル通信 | アールティ　移動型ロボットブログ](https://rt-net.jp/mobility/archives/7801)

## eclipseで一括インデント
- `ctrl + i`
- [Eclipseで一括インデント | ハックノート](https://hacknote.jp/archives/2270/)

## STM32 HAL ADCの基礎の基礎
- DMAで連続変換するときの鍵
  - clockはsystem clock devidedなものを入れると楽．Async clockは別途記述が必要．
  - continuous conversion enableにしてDMA continuous request enableにしておくとDMA割り込みが入りまくることに注意．ADCが終わるたびにDMA走る．
    - これを防ぐにはcontinuous conversion disableとして普通にタイマ割り込みでADCを回すのが良さそう．DMA continuous requestはenableのまま．
  - DMAはcircularを選ぶ
  - DMAのmemory はincrementにしておく
  - DMA continuous request enable
  - 上記をやっておかないと連続変換されなかったり，DMAに連続的に変換結果が入らずにハマる．
    - もちろん理解して変更すればいいが，基本上記でいいかなぁという気持ち．

- code examples
```cpp
constexpr  adcChannels = 3; //number of using ADC channels
static uint16_t adcData[adcChannels];
if (HAL_ADC_Start_DMA(&hadc,(uint32_t *)adcData, adcChannels) != HAL_OK)
  Error_Handler();
```
これで逐次clockに応じてADCが動作しDMAで `adcData[]`に変換結果が転送される．
変換完了後にcallbackしたい場合は下記等が便利．変換完了時にcallbackされる．
```
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef *AdcHandle){
  //do something
}
```
- キャリブレーションが必要な場合は下記を呼び出すことでADCのキャリブレーションが可能らしい．
しかしこれは通常起動時に実施済みであるため明示的には不要の場合が多いが，長時間駆動する機器の場合定期的にキャリブレーションを実施することにより精度の高い測定が可能となるよう．
```cpp
if(HAL_ADCEx_Calibration_Start(&hadc) != HAL_OK)
    Error_Handler();
```

- タイマ割り込みにする場合
  - CubeMXでADCのconfigを下記のようにする．ADCのチャネル，DMA転送など基本的なconfigはできている前提で
    - `ADC_Regular_ConversionMode`->`External Trigger Conversion Source`を`Timer1 Trigger Out event`に．
```eval_rst
.. image:: ../resources/images/adc01.png
```
  - さらにCubeMXでTIM Timer1のconfigをする．プリスケーラやカウンタ値等のタイマ割り込みを発生させるconfigは書けている前提で
    - `Trigger Output(TRGO) Parameters` -> `Trigger Event Selection`を`Update Event`に．
```eval_rst
.. image:: ../resources/images/adc02.png
```
  これでタイマ割り込みでADCがトリガされるはず．

- ADCのキャネルとデータの入り方
`ch1, ch2, ch3, ch4, ch5, ch6, ch7` を有効にしていたとして
```cpp
constexpr  adcChannels = 8; //number of using ADC channels
static uint16_t adcData[adcChannels];
if (HAL_ADC_Start_DMA(&hadc,(uint32_t *)adcData, adcChannels) != HAL_OK)
  Error_Handler();
```
このようなコードを書いた場合，変換結果は
```cpp
ch1: adcData[0];
ch2: adcData[1];
ch3: adcData[2];
ch4: adcData[3];
ch5: adcData[4];
ch6: adcData[5];
ch7: adcData[6];
ch8: adcData[7];
```
に対応して格納される．

`ch1`, `ch3`, `ch5`, `ch7`を有効(`ch2`, `ch4`, `ch6`は無効)にしていたとして
```cpp
constexpr  adcChannels = 4; //number of using ADC channels
static uint16_t adcData[adcChannels];
if (HAL_ADC_Start_DMA(&hadc,(uint32_t *)adcData, adcChannels) != HAL_OK)
  Error_Handler();
```
このようなコードを書くと
```cpp
ch1: adcData[0];
ch3: adcData[1];
ch5: adcData[2];
ch7: adcData[3];
```
仮にここで
```
constexpr  adcChannels = 3; //number of using ADC channels
```
としてしまった場合，書くchの変換結果が同一の添字(配列index)で取得できなくなる．
具体的にはリングバッファのようにずれてしまう．
したがって`HAL_ADC_Start_DMA()`に渡すbufはchに対し十分余裕を持ちlengthは有効化しているadcチャネルと同一値とすることが大変重要である．
有効化しているadcチャネル数よりも少なくても多くてもずれが生じる．
この状態で気づかずに変換結果を取得すると逐次値が変動してハマる．ノイズのよう見見えてしまうことがあるかもしれない．
念の為adcの各chが同一の添字で同じものが取り続けられているかをserial(uart)等を利用し事前に確認しておくことが望ましい．

- ちなみにプロトタイプはこんな感じになっている: `HAL_StatusTypeDef HAL_ADC_Start_DMA(ADC_HandleTypeDef* hadc, uint32_t* pData, uint32_t Length)`

## STM32 HAL 外部割り込みの基礎の基礎
- 外部割り込みをするには
  - GPIOをEXTI Interrupt modeにする．
```eval_rst
.. image:: ../resources/images/exti01.png
```
  - NVICで該当のEXTI Interruptをenableにする．
```eval_rst
.. image:: ../resources/images/exti02.png
```
    - これを忘れるとEXTIされないので注意．ハマりがち．
    - 外部割り込みに限らずなんだけれども，割り込みの優先順位は適切につける．
      - 例えば外部割り込みの中で`HAL_Delay()`を呼んだりしたいときは`Time base: System tick timer`より外部割り込みの優先順位を下げないと処理が戻ってこなかったりする．

実際のコードをみて割り込みの流れをみる

- `./Core/Src/stm32f0xx_it.c`
```c
/******************************************************************************/
/* STM32F0xx Peripheral Interrupt Handlers                                    */
/* Add here the Interrupt Handlers for the used peripherals.                  */
/* For the available peripheral interrupt handler names,                      */
/* please refer to the startup file (startup_stm32f0xx.s).                    */
/******************************************************************************/

/**
  * @brief This function handles EXTI line 0 and 1 interrupts.
  */
void EXTI0_1_IRQHandler(void)
{
  /* USER CODE BEGIN EXTI0_1_IRQn 0 */

  /* USER CODE END EXTI0_1_IRQn 0 */
  HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_1);
  /* USER CODE BEGIN EXTI0_1_IRQn 1 */

  /* USER CODE END EXTI0_1_IRQn 1 */
}
```
- 複数のEXTIピンを定義した場合でも同じ`HAL_GPIO_EXTI_IRQHandler()`がcallされる．
```c
/******************************************************************************/
/* STM32F0xx Peripheral Interrupt Handlers                                    */
/* Add here the Interrupt Handlers for the used peripherals.                  */
/* For the available peripheral interrupt handler names,                      */
/* please refer to the startup file (startup_stm32f0xx.s).                    */
/******************************************************************************/

/**
  * @brief This function handles EXTI line 0 and 1 interrupts.
  */
void EXTI0_1_IRQHandler(void)
{
  /* USER CODE BEGIN EXTI0_1_IRQn 0 */

  /* USER CODE END EXTI0_1_IRQn 0 */
  HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_1);
  /* USER CODE BEGIN EXTI0_1_IRQn 1 */

  /* USER CODE END EXTI0_1_IRQn 1 */
}

/**
  * @brief This function handles EXTI line 2 and 3 interrupts.
  */
void EXTI2_3_IRQHandler(void)
{
  /* USER CODE BEGIN EXTI2_3_IRQn 0 */

  /* USER CODE END EXTI2_3_IRQn 0 */
  HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_3);
  /* USER CODE BEGIN EXTI2_3_IRQn 1 */

  /* USER CODE END EXTI2_3_IRQn 1 */
}
```

- `HAL_GPIO_EXTI_IRQHandler()`がcallされると割り込みビットの処理が実行され，`HAL_GPIO_EXTI_Callback()`がcallされる．
  - これがユーザが実際に定義することを想定しているcallback functionである．
  - `stm32f0xx_hal_gpio.c`ですでに`__weak void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)`という形で定義されているが，`__weak`指定子がついているので，ユーザが`void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)`を別で定義した場合それが優先される．
- `./Drivers/STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_gpio.c`
```c
/**
  * @brief  Handle EXTI interrupt request.
  * @param  GPIO_Pin Specifies the port pin connected to corresponding EXTI line.
  * @retval None
  */
void HAL_GPIO_EXTI_IRQHandler(uint16_t GPIO_Pin)
{
  /* EXTI line interrupt detected */
  if(__HAL_GPIO_EXTI_GET_IT(GPIO_Pin) != 0x00u)
  {
    __HAL_GPIO_EXTI_CLEAR_IT(GPIO_Pin);
    HAL_GPIO_EXTI_Callback(GPIO_Pin);
  }
}

/**
  * @brief  EXTI line detection callback.
  * @param  GPIO_Pin Specifies the port pin connected to corresponding EXTI line.
  * @retval None
  */
__weak void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
  /* Prevent unused argument(s) compilation warning */
  UNUSED(GPIO_Pin);

  /* NOTE: This function should not be modified, when the callback is needed,
            the HAL_GPIO_EXTI_Callback could be implemented in the user file
   */
}
```
- ユーザは実際に`void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)`を定義して利用することができる．
  - 引数として`GPIO_PIN`が渡されるので，これを見てどのピンのEXTI割り込みかを判断する．
```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin){
  HAL_Delay(1);
  printf("EXTI callback called!\r\n");
  if (GPIO_Pin == GPIO_PIN_3)
    printf("gpio3 pushed!\r\n");
  if (GPIO_Pin == GPIO_PIN_1)
    printf("gpio1 released!\r\n");
}
```
- [STM32 external interrupt and tick timer - Programmer Soughte](http://www.programmersought.com/article/7374878661/)
  - EXTI内部で`HAL_Delay()`を叩くとすべての処理が停止することがあった．
  - これは割り込み優先度がすべて`0`となっていたため．
  - `HAL_Delay()`は`systick`を利用するためこの割り込みの優先度をあげ，`EXTI`割り込みの優先度を下げる必要がある．
  - これにより無限ループによる処理停止が引き起こされない．

## STM32 HAL タイマ割り込みの基礎の基礎
- タイマ割り込みを使えるようにする．
  - CubeMXで利用したいタイマを有効にする．
  - プリスケーラ，カウンタを適切に計算してセット．
    - 基本的にtimer clock sourceはinternal clockならAPB系がつかわれるが，どの系でくるかはdatasheet要確認．
  - auto preloadをenableにしておくと楽．
    - overしたときのcnt resetを自動でやってくれる．
```eval_rst
.. image:: ../resources/images/timitr01.png
```
- タイマ割り込みをenableにする．
  - 忘れるとハマる．
```eval_rst
.. image:: ../resources/images/timitr02.png
```
- どこかで`HAL_TIM_Base_Start_IT(&htimx);` をcallしてtimer interruptをenableにする．

コードを見ていく

- `./Drivers/STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_tim.c`
```c
HAL_StatusTypeDef HAL_TIM_Base_Start_IT(TIM_HandleTypeDef *htim)
{
  uint32_t tmpsmcr;

  /* Check the parameters */
  assert_param(IS_TIM_INSTANCE(htim->Instance));

  /* Enable the TIM Update interrupt */
  __HAL_TIM_ENABLE_IT(htim, TIM_IT_UPDATE);

  /* Enable the Peripheral, except in trigger mode where enable is automatically done with trigger */
  tmpsmcr = htim->Instance->SMCR & TIM_SMCR_SMS;
  if (!IS_TIM_SLAVEMODE_TRIGGER_ENABLED(tmpsmcr))
  {
    __HAL_TIM_ENABLE(htim);
  }

  /* Return function status */
  return HAL_OK;
}
```
- 基本的に`main.cpp` の`static void MX_TIMx_Init(void)`あたりの最後で呼び出せばいいと思う．
  - もちろんmain関数の頭らへん(`MX_TIM3_Init()` が呼ばれたあと)で呼んでももちろん動くはず．
  - ex.
```c
/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 39999;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 199;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */
  HAL_TIM_Base_Start_IT(&htim3);
  /* USER CODE END TIM3_Init 2 */

}
```
- `main.cpp`にcallbackを書く
  - 複数あるときでも引数で渡される`&htimx`を見て比較して分岐する．
  - `./Core/Src/main.cpp`
```cpp
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim){
  if(htim == &htim3)
    HAL_GPIO_TogglePin(GPIOF, GPIO_PIN_0);
  if(htim == &htim14)
    printf("htim14 callback!\r\n");
}
```

- 大まかな流れと実装．
  - `./Core/Src/stm32f0xx_it.c`
    - それぞれの割り込みハンドラがここにいる．
    - タイマの割り込みハンドラは共通の`HAL_TIM_IRQHandler(&htimx)` をcallする．
```c
/**
  * @brief This function handles TIM3 global interrupt.
  */
void TIM3_IRQHandler(void)
{
  /* USER CODE BEGIN TIM3_IRQn 0 */

  /* USER CODE END TIM3_IRQn 0 */
  HAL_TIM_IRQHandler(&htim3);
  /* USER CODE BEGIN TIM3_IRQn 1 */

  /* USER CODE END TIM3_IRQn 1 */
}
```

  - `./Drivers/STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_tim.c`
    - `HAL_TIM_IRQHandler(TIM_HandleTypeDef *htim)`は共通のタイマハンドラ．
    - ここでタイマの各種イベントに応じてcallback先を選んでいる．
    - タイマの時間切れ(elasped)の場合update eventとなり，タイマ共通の`HAL_TIM_PeriodElapsedCallback(htim)`がcallされるが，htimはそれぞれのIRQHandlerで引数として渡したタイマのオブジェクト(address)が入っているのでhtimを見ることでどのタイマによって発生したイベントかを判断できる．
```c
/**
  * @}
  */
/** @defgroup TIM_Exported_Functions_Group7 TIM IRQ handler management
  *  @brief    TIM IRQ handler management
  *
@verbatim
  ==============================================================================
                        ##### IRQ handler management #####
  ==============================================================================
  [..]
    This section provides Timer IRQ handler function.

@endverbatim
  * @{
  */
/**
  * @brief  This function handles TIM interrupts requests.
  * @param  htim TIM  handle
  * @retval None
  */
void HAL_TIM_IRQHandler(TIM_HandleTypeDef *htim)
{
  ...(snip)...
  /* TIM Update event */
  if (__HAL_TIM_GET_FLAG(htim, TIM_FLAG_UPDATE) != RESET)
  {
    if (__HAL_TIM_GET_IT_SOURCE(htim, TIM_IT_UPDATE) != RESET)
    {
      __HAL_TIM_CLEAR_IT(htim, TIM_IT_UPDATE);
#if (USE_HAL_TIM_REGISTER_CALLBACKS == 1)
      htim->PeriodElapsedCallback(htim);
#else
      HAL_TIM_PeriodElapsedCallback(htim);
#endif /* USE_HAL_TIM_REGISTER_CALLBACKS */
    }
  }
  ...(snip)...
}
```

## STM32 HAL PWMの基礎の基礎
- PWMを出すには
  - CubeMXで利用したいタイマを選択．channelの設定でどのピンをPWM出力として使うかを指定．
  - 普通のタイマの設定と同様にプリスケーラ，カウンタを設定．
  - cubeMXでgenerateされるコードはconfigまでは入るがそのままではPWMがstartしない．
    - `HAL_TIM_PWM_Start();`をcallする必要がある．`MX_TIMx_Init()`の最後で呼んでおくと良さそう．
    - 別にmainで利用時にstartするのでも問題ない．

コードを見ていく

```cpp
/**
  * @brief TIM1 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM1_Init(void)
{

  /* USER CODE BEGIN TIM1_Init 0 */

  /* USER CODE END TIM1_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};
  TIM_BreakDeadTimeConfigTypeDef sBreakDeadTimeConfig = {0};

  /* USER CODE BEGIN TIM1_Init 1 */

  /* USER CODE END TIM1_Init 1 */
  htim1.Instance = TIM1;
  htim1.Init.Prescaler = 9;
  htim1.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim1.Init.Period = 799;
  htim1.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim1.Init.RepetitionCounter = 0;
  htim1.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_ENABLE;
  if (HAL_TIM_Base_Init(&htim1) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim1, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim1) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim1, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 100;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCNPolarity = TIM_OCNPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  sConfigOC.OCIdleState = TIM_OCIDLESTATE_RESET;
  sConfigOC.OCNIdleState = TIM_OCNIDLESTATE_RESET;
  if (HAL_TIM_PWM_ConfigChannel(&htim1, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.Pulse = (uint32_t)htim1.Init.Period*0.4;
  if (HAL_TIM_PWM_ConfigChannel(&htim1, &sConfigOC, TIM_CHANNEL_4) != HAL_OK)
  {
    Error_Handler();
  }
  sBreakDeadTimeConfig.OffStateRunMode = TIM_OSSR_DISABLE;
  sBreakDeadTimeConfig.OffStateIDLEMode = TIM_OSSI_DISABLE;
  sBreakDeadTimeConfig.LockLevel = TIM_LOCKLEVEL_OFF;
  sBreakDeadTimeConfig.DeadTime = 0;
  sBreakDeadTimeConfig.BreakState = TIM_BREAK_DISABLE;
  sBreakDeadTimeConfig.BreakPolarity = TIM_BREAKPOLARITY_HIGH;
  sBreakDeadTimeConfig.AutomaticOutput = TIM_AUTOMATICOUTPUT_DISABLE;
  if (HAL_TIMEx_ConfigBreakDeadTime(&htim1, &sBreakDeadTimeConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM1_Init 2 */

  HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);
  HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_4);
  /* USER CODE END TIM1_Init 2 */
  HAL_TIM_MspPostInit(&htim1);

}
```
- 例えばこんな関数を作っておくとdutyをtimer/channelごとに更新できる．
```cpp
void setPWM(TIM_HandleTypeDef *htim, uint32_t Channel, float duty)
{
  TIM_OC_InitTypeDef sConfigOC = {0};

  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = (uint32_t)htim1.Init.Period*duty;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCNPolarity = TIM_OCNPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  sConfigOC.OCIdleState = TIM_OCIDLESTATE_RESET;
  sConfigOC.OCNIdleState = TIM_OCNIDLESTATE_RESET;

  if (HAL_TIM_PWM_ConfigChannel(htim, &sConfigOC, Channel) != HAL_OK)
  {
    Error_Handler();
  }

  HAL_TIM_PWM_Start(htim, Channel);
}
```
- 相補PWMするときは`HAL_TIMEx_PWMN_Start(&htim1, TIM_CHANNEL_1);` をcallしなければ動かない
  - [STM32F303k8+HALで相補PWM生成を試した - Qiita](https://qiita.com/r_u__r_u/items/c71ce07071a8c8673428)

## STM32 HAL GPIOの基礎の基礎
- tips: GPIOのlabelはただのエイリアス．
- `./Core/Inc/main.h`
```cpp
#define LCD_RS_Pin GPIO_PIN_6
#define LCD_RS_GPIO_Port GPIOA
```
- エイリアスの先はちゃんとdriverのheaderで定義されている．なのでlabelをつけたからといって`GPIOA`や`GPIO_PIN_X`などで指定できなくなるというわけではない．
  - `./Drivers/STM32F0xx_HAL_Driver/Inc/stm32f0xx_hal_gpio.h`
```c
...(snip)...
/* Exported constants --------------------------------------------------------*/
/** @defgroup GPIO_Exported_Constants GPIO Exported Constants
  * @{
  */
/** @defgroup GPIO_pins GPIO pins
  * @{
  */
#define GPIO_PIN_0                 ((uint16_t)0x0001U)  /* Pin 0 selected    */
#define GPIO_PIN_1                 ((uint16_t)0x0002U)  /* Pin 1 selected    */
#define GPIO_PIN_2                 ((uint16_t)0x0004U)  /* Pin 2 selected    */
#define GPIO_PIN_3                 ((uint16_t)0x0008U)  /* Pin 3 selected    */
#define GPIO_PIN_4                 ((uint16_t)0x0010U)  /* Pin 4 selected    */
#define GPIO_PIN_5                 ((uint16_t)0x0020U)  /* Pin 5 selected    */
#define GPIO_PIN_6                 ((uint16_t)0x0040U)  /* Pin 6 selected    */
#define GPIO_PIN_7                 ((uint16_t)0x0080U)  /* Pin 7 selected    */
#define GPIO_PIN_8                 ((uint16_t)0x0100U)  /* Pin 8 selected    */
#define GPIO_PIN_9                 ((uint16_t)0x0200U)  /* Pin 9 selected    */
#define GPIO_PIN_10                ((uint16_t)0x0400U)  /* Pin 10 selected   */
#define GPIO_PIN_11                ((uint16_t)0x0800U)  /* Pin 11 selected   */
#define GPIO_PIN_12                ((uint16_t)0x1000U)  /* Pin 12 selected   */
#define GPIO_PIN_13                ((uint16_t)0x2000U)  /* Pin 13 selected   */
#define GPIO_PIN_14                ((uint16_t)0x4000U)  /* Pin 14 selected   */
#define GPIO_PIN_15                ((uint16_t)0x8000U)  /* Pin 15 selected   */
#define GPIO_PIN_All               ((uint16_t)0xFFFFU)  /* All pins selected */
...(snip)...
```

## STM32でSWCLK, SWDIOをGPIOとしてoutputに使ったとき．
- いつもどおりflashからbootすると，起動直後にGPIOがoutになってしまってそのままでは書き込めなくなる．
- いったんBOOT0をHighにしてシステムメモリor RAM起動にするとから書き込むと書き込める．
  - BOOT0の状態BOOT1=Highならシステムメモリ，LowならRAM boot.
  - BOOT0=Lowだとflash bootだからね．
- [ST Community](https://community.st.com/s/question/0D50X0000BMHt1TSQT/reuse-swd-pins-on-stm32f0)
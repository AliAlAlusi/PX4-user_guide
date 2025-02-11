# Заводське калібрування IMU/компаса

Виробники PX4 OEM можуть виконувати IMU і компасувати завод для збереження значень для акселерометра, калібрування гіроскопа та магнітометра у постійне сховище (зазвичай ЕЗПМ). Це забезпечує можливість завжди скидати конфігурації та налаштування транспортного засобу до безпечного стану для польоту.

Ця процедура запише такі параметри в `/fs/mtd_caldata`: [CAL_ACC\*](../advanced_config/parameter_reference.md#CAL_ACC0_ID), [CAL_GYRO\*](../advanced_config/parameter_reference.md#CAL_GYRO0_ID), [CAL_MAG\*](../advanced_config/parameter_reference.md#CAL_MAG0_ID) . Ці дані будуть використані, коли параметри будуть встановлені (або скинуті) до їхніх значень за замовчуванням.

:::warning
Ця функція спирається на FMU наявності спеціального архіву EEPROM або супроводжуючого IMU PCBA, який має достатньо місця для даних. PX4 збереже дані у `/fs/mtd_caldata`, створивши файл у разі необхідності.
:::

:::note
Ці значення не можуть бути збережені в [конфігурації кадрів](../dev_airframes/adding_a_new_frame.md) оскільки вони відрізняються від пристрою в пристрої (конфігурація рамки визначає набір параметрів, які застосовуються на всіх автомобілях того ж типу, такі, як увімкнені датчики, [обертання автопілота](../config/flight_controller_orientation.md) і настроювання PID).
:::

## Виконання заводського калібрування

1. Встановіть параметр [SYS_FAC_CAL_MODE](../advanced_config/parameter_reference.md#SYS_FAC_CAL_MODE) на 1.
1. Виконайте всі калібрування IMU: [акселерометр](../config/accelerometer.md#performing-the-calibration), [гіроскоп](../config/gyroscope.md#performing-the-calibration) і [магнітометр](../config/compass.md#performing-the-calibration).
1. Перезавантажте пристрій. Це запише всі параметри `CAL_ACC*`, `CAL_GYRO*` і `CAL_MAG*` в `/fs/mtd_caldata`.
1. Знову встановіть параметр `SYS_FAC_CAL_MODE` на 0 (за замовчуванням).

:::note
Якщо ви хочете провести заводську калібрування лише акселерометра та гіроскопа, ви можете встановити [SYS_FAC_CAL_MODE](../advanced_config/parameter_reference.md#SYS_FAC_CAL_MODE) на 2, у цьому випадку магнітометр виключається.
:::

Подальші корекції користувача будуть враховані як зазвичай (дані заводського калібрування використовуються лише для значень за замовчуванням параметрів).

## Подальша інформація

- [QGroundControl User Guide > Sensors](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/setup_view/sensors_px4.html)

---
title: "Подключение Raspberry Pi (Node) к Интернету вещей Azure. Приступая к работе | Документация Майкрософт"
description: "Начните работу с устройством Raspberry Pi 3, создайте свой Центр Интернета вещей Azure и подключите устройство Pi к этому центру."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Центр Интернета вещей Azure, приступая к работе с Интернетом вещей, инструментарий Интернета вещей"
experimental: true
experiment_id: xshi-happypathemu-20161202
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 358a2a4fd448660c3a8ef0d11d7d373dd7d9a569


---
# <a name="get-started-with-raspberry-pi-3-nodejs"></a>Приступая к работе с Raspberry Pi 3 (Node.js)
> [!div class="op_single_selector"]
> * [Node.JS](iot-hub-raspberry-pi-kit-node-get-started.md)
> * [C](iot-hub-raspberry-pi-kit-c-get-started.md)

В этом учебнике описано, как начать работу с устройством Raspberry Pi 3 под управлением Raspbian. Также вы узнаете, как можно легко подключать устройства к облаку с помощью [Центра Интернета вещей Azure](iot-hub-what-is-iot-hub.md). Примеры для Windows 10 IoT Базовая представлены в [Центре разработки для Windows](http://www.windowsondevices.com/).

Нет начального набора? Начните [отсюда](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="lesson-1-configure-your-device"></a>Урок 1. Настройка устройства
![Урок 1. Сквозная схема](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

В ходе этого урока вы настроите операционную систему на устройстве Raspberry Pi 3, а затем настроите среду разработки и развернете приложение на устройстве Pi.

### <a name="configure-your-device"></a>Настройка устройства
Настройте устройство Raspberry Pi 3 для первого использования и установите Raspbian. Raspbian — это бесплатная операционная система, которая оптимизирована для оборудования Raspberry Pi.

*Предполагаемое время выполнения: 30 минут*

Перейдите к статье [Настройка устройства](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).

### <a name="get-the-tools"></a>Получить инструменты
Скачайте инструменты и программное обеспечение для сборки и развертывания приложения на устройстве Raspberry Pi 3.

*Предполагаемое время выполнения: 20 минут*

Перейдите к статье [Get the tools](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md) (Получение инструментов).

### <a name="create-and-deploy-the-blink-application"></a>Создание и развертывание приложения для включения индикатора
Клонируйте пример приложения Node.js для включения индикатора из Github и разверните его с помощью средства Gulp на плате Raspberry Pi 3. Это приложение будет каждые две секунды включать и выключать светодиодный индикатор на компьютере.

*Предполагаемое время выполнения: 5 минут*

Перейдите к статье [Создание и развертывание приложения для включения индикатора](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md).

## <a name="lesson-2-create-your-iot-hub"></a>Урок 2. Создание Центра Интернета вещей
![Урок 2. Сквозная схема](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

В ходе этого урока вы создадите бесплатную учетную запись Azure, подготовите Центр Интернета вещей Azure и создадите в нем первое устройство.

Прежде чем продолжать, завершите урок 1.

### <a name="get-the-azure-tools"></a>Получение средств Azure
Установите интерфейс командной строки Azure (Azure CLI).

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Получение инструментов Azure (Windows&7; и более поздние версии)](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md).

### <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Создание Центра Интернета вещей и регистрация Raspberry Pi 3
Создайте группу ресурсов, подготовьте Центр Интернета вещей Azure и добавьте в него первое устройство, используя интерфейс командной строки Azure.

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Создание Центра Интернета вещей и регистрация Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).

## <a name="lesson-3-send-device-to-cloud-messages"></a>Урок 3. Отправка сообщений с устройства в облако
![Урок 3. Сквозная схема](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

В ходе этого урока вы отправите сообщения c устройства Pi в Центр Интернета вещей. Также вы создадите приложение-функцию Azure, которое получает входящие сообщения из Центра Интернета вещей и записывает их в хранилище таблиц Azure.

Прежде чем продолжать, завершите уроки 1 и 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Создание учетной записи хранения Azure и приложения-функции Azure
С помощью шаблона Azure Resource Manager создайте приложение-функцию Azure и учетную запись хранения Azure.

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Создание приложения-функции Azure и учетной записи хранения Azure](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Запуск примера приложения для отправки сообщений с устройства в облако
Разверните и запустите на устройстве Raspberry Pi 3 пример приложения, которое отправляет сообщения в Центр Интернета вещей.

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Запуск примера приложения для отправки сообщений с устройства в облако](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

### <a name="read-messages-persisted-in-azure-storage"></a>Чтение сообщений, сохраненных в службе хранилища Azure
Отслеживайте сообщения, передаваемые с устройства в облако, по мере их записывания в хранилище Azure.

*Предполагаемое время выполнения: 5 минут*

Перейдите к статье [Чтение сообщений, сохраненных в службе хранилища Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md).

## <a name="lesson-4-send-cloud-to-device-messages"></a>Урок 4. Отправка сообщений из облака на устройство
![Урок 4. Сквозная схема](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

В ходе этого урока демонстрируется способ отправки сообщений из Центра Интернета вещей на устройство Raspberry Pi 3. С помощью этих сообщений вы будете управлять включением и отключением светодиодного индикатора, который подключен к устройству Pi. Для выполнения этой задачи подготовлен пример приложения.

Прежде чем продолжать, завершите уроки 1, 2 и 3.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Запуск примера приложения для получения сообщений из облака на устройство
Пример приложения из урока 4 выполняется на устройстве Pi и отслеживает входящие сообщения от Центра Интернета вещей. Новая задача Gulp отправляет из Центра Интернета вещей сообщения на устройство Pi для управления состоянием индикатора.

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Run the sample application to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md) (Запуск примера приложения для получения сообщений из облака на устройство).

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Дополнительный раздел. Изменение режима включения и отключения светодиодного индикатора
Настроив сообщения, вы сможете изменять режим включения и отключения светодиодного индикатора.

*Предполагаемое время выполнения: 10 минут*

Перейдите к статье [Дополнительный раздел. Изменение режима включения и отключения светодиодного индикатора](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md).

## <a name="troubleshooting"></a>Устранение неполадок
Если в ходе какого-либо урока у вас возникнут проблемы, то решение можно найти в статье [Troubleshooting](iot-hub-raspberry-pi-kit-node-troubleshooting.md) (Устранение неполадок).




<!--HONumber=Jan17_HO4-->



# H2U Mbed
[Full API list](https://os.mbed.com/docs/mbed-os/v6.15/apis/index.html)
## Serial Commnicate
document : [BufferedSerial](https://os.mbed.com/docs/mbed-os/v6.15/apis/serial-uart-apis.html)

- include item
    ```
    #include "mbed.h"
    ```
- commnicate handler
    ```
    static BufferedSerial serial_port(USBTX, USBRX);
    ```
- init
    ```
    serial_port.set_baud(115200);
    serial_port.set_format(
        /* bits */ 8,
        /* parity */ BufferedSerial::None,
        /* stop bit */ 1
    );
    ```

- receive
    Data written of buf(Buffer). num is data size.
    ```
    uint32_t num = serial_port.read(buf, sizeof(buf))
    ```

- send
    buf is sending item. num is sending item size.
    ```
    serial_port.write(buf, num);
    ```

example code :

```
/*
 * Copyright (c) 2020 Arm Limited and affiliates.
 * SPDX-License-Identifier: Apache-2.0
 */

#include "mbed.h"

// Maximum number of element the application buffer can contain
#define MAXIMUM_BUFFER_SIZE  32

// Create a DigitalOutput object to toggle an LED whenever data is received.
static DigitalOut led(LED1);

// Create a BufferedSerial object with a default baud rate.
static BufferedSerial serial_port(USBTX, USBRX);

int main(void)
{
    // Set desired properties (9600-8-N-1).
    serial_port.set_baud(115200);
    serial_port.set_format(
        /* bits */ 8,
        /* parity */ BufferedSerial::None,
        /* stop bit */ 1
    );

    // Application buffer to receive the data
    char buf[MAXIMUM_BUFFER_SIZE] = {0};

    while (1) {
        if (uint32_t num = serial_port.read(buf, sizeof(buf))) {
            // Toggle the LED.
            led = !led;

            // Echo the input back to the terminal.
            serial_port.write(buf, num);
        }
    }
}
```

## PWM

document : [PwmOut](https://os.mbed.com/docs/mbed-os/v6.15/apis/pwmout.html)

- init 
    ```
    PwmOut led(LED1);
    ```
- set freq(preiod)
    ```
    led.period_us(100); 
    ```
- output

    50% duty cycle
    ```
    led.write(0.5f);
    ```


## Digital output

document : [DigitalOut](https://os.mbed.com/docs/mbed-os/v6.15/apis/digitalout.html)

- init 
    ```
    DigitalOut led(LED1);
    ```

- output
    - High
        ```
        led.write(1);
        ```
    - Low
        ```
        led.write(0);
        ```

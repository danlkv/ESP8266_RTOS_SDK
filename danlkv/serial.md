## Play with serial connection

Branch: `danlkv/serial`
Example: [`hello_world`](../examples/get-started/hello_world/)

Use the hello world exampe and change the baud rate to 115200.

ESP has 2 UART ports.

These constants specify the port id:[include/driver/uart.h](https://github.com/espressif/ESP8266_RTOS_SDK/blob/release/v3.4/components/esp8266/include/driver/uart.h#L69)


The function to set baud rate: [`uart_set_baud_rate`](https://github.com/espressif/ESP8266_RTOS_SDK/blob/release/v3.4/components/esp8266/include/driver/uart.h#L220)

See the [`hello_world_main.c`](../examples/get-started/hello_world/main/hello_world_main.c) for implementation.


## Problem: corrupt output

If switch happens right after print, the output gets corrupted, when reading on both new and old baud rates.

To introduce delay/synchronisation after the last old-baud print:

- `fflush(stdout);` does not help.
- `vTaskDelay(30 / portTICK_PERIOD_MS);` works

Printing some additional characters at new baud helps to clean the screen from
previous old-baud gibberish.


Testing:

- `python -m serial.tools.miniterm /dev/serial0 74880 --raw`
- `python -m serial.tools.miniterm /dev/serial0 115200 --raw`

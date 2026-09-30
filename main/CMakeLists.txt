idf_component_register(SRCS "DistanceMonitor.c"
                    INCLUDE_DIRS "."
                    REQUIRES oled_driver ultrasonic_sensor sd_card_spi)


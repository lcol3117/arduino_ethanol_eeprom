# arduino_ethanol_eeprom
using arduino uno to read data from the I2C bus of an SGP30 ethanol sensor and save it into the internal EEPROM memory to be retrieved later. 

Roughly every 5 seconds, data is read from the SGP30 and saved into a byte of memory, 

Each byte represents concentration in PPM

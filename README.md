# Lontium LT8911EXB Driver
This is the Linux driver for Lontium LT8911EXB, which is used to convert MIPI to eDP output.  

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be a reference to you, when you are integrating the Lontium's LT8911EXB IC into your system, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

## Usage
1. Copy lt8911exb to kernel/drivers/video/ directory.
2. Modify the kernel/drivers/video/Kconfig as follows
```
source "drivers/video/lt8911exb/Kconfig"
```
3. Modify the kernel/drivers/video/Makefile as follows
```
obj-$(CONFIG_LT8911EXB_DRIVER) += lt8911exb/
```
4. Modify the dts as follows
```
	&i2c2 {  
		status = "okay";
		lt8911exb@29 {
			compatible = "lontium,lt8911exb";
			reg = <0x29>;
			power-gpio = <&gpio1 GPIO_B4 GPIO_ACTIVE_HIGH>;
			reset-gpio = <&gpio0 GPIO_A2 GPIO_ACTIVE_HIGH>;

			lontium,pclk = <152000000>;
			lontium,hact = <1920>;			 
			lontium,vact = <1080>;			  
			lontium,hbp = <192>;		  	  
			lontium,hfp = <48>;		  
			lontium,vbp = <71>;			
			lontium,vfp = <3>;		  
			lontium,hs = <32>;			 
			lontium,vs = <6>;
			
			lontium,mipi_lane = <4>;
			lontium,lane_cnt = <2>;
			lontium,color = <1>; //Color Depth 0:6bit 1:8bit
			lontium,test = <0>;
		};
	};
```

## Notes:
1. IIC address of LT8911EXB:  
  A) If LT8911EXB's number 13 (S_ADR) is low, the I2C address of LT8911EXB is 0x52, and bit0 is the read-write mark bit.  
  If it is a Linux system, the I2C address of LT8911EXB is 0x29, and bit7 is the read and write flag bit.  
  B) If LT8911EXB's 13th foot (S_ADR) is high, then LT8911EXB's I2C address is 0x5a, and bit0 is the read-write mark bit.  
  If it is a Linux system, the I2C address of LT8911EXB is 0x2d, and bit7 is the read and write flag bit.
2. The IIC rate should not exceed 100KHz. 
3. To ensure that LVDS signal is given to LT8911EXB, then initialize LT8911EXB. 
4. The front-end master control GPIO is required to reply to LT8911EXB. Before the register, the LT8911EXB is reset. Use GPIO to lower LT8911EXB's reset foot 100ms, then pull up and maintain 100ms.

## Developed By
* ayst.shen@foxmail.com

## License
	Copyright 2018-2019 Shen Haibo

	Licensed under the Apache License, Version 2.0 (the "License");
	you may not use this file except in compliance with the License.
	You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

	Unless required by applicable law or agreed to in writing, software
	distributed under the License is distributed on an "AS IS" BASIS,
	WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	See the License for the specific language governing permissions and
	limitations under the License.

# ili9481-raspberrypi-driver
Use ili9481 with 8 bit parallel interface on Raspberry Pi uses Frame buffer driver(notro/fbtft).

Connect to raspberrypi with 8-bit parallel interface and uses framebuffer on the screen.

Uses Notro/fbtft on ili9481.

Wiring:
```
dc:18,reset:7,wr:17,cs:4,db00:22,db01:23,db02:24,db03:10,db04:25,db05:9,db06:11,db07:8
```
If your display manual have no dc wire marked, Use ```rs```instead, it's same. Remeber it's not rst. For other wires follow instruction from your vendor.

In your console type:

```
sudo modprobe flexfb width=480 height=320 buswidth=8 init=-1,0x01,-2,5,-1,0xD0,0x07,0x42,0x18,-1,0xD1,0x00,0x07,0x18,-1,0xD2,0x01,0x02,-1,0xC0,0x10,0x3B,0x00,0x02,0x11,-1,0xC5,0x03,-1,0x36,0x28,-1,0x21,0x21,-1,0x3A,0x55,-1,0x11,-2,120,-1,0x29,-2,20,-3
```

```
sudo modprobe fbtft_device name=flexpfb gpios=dc:18,reset:7,wr:17,cs:4,db00:22,db01:23,db02:24,db03:10,db04:25,db05:9,db06:11,db07:8
```

To let the screen shows your console(FB), you need to execute following code.

```
fbset -i -fb /dev/fb1
con2fbmap 1 1
```

Same code will work in /etc/modules and /etc/modprobe.d/fbtft.conf

To make driver loads on boot, Edit /etc/modules and add ```fbtft``` and ```flexfb``` and ```spi-bcm2835``` in the file, one per line.

Then create a new file under ```/etc/modprobe.d``` name it as fbtft.conf and add following lines into it.

```
options fbtft_device name=flexpfb gpios=dc:18,reset:7,wr:17,cs:4,db00:22,db01:23,db02:24,db03:10,db04:25,db05:9,db06:11,db07:8
```

```
options flexfb width=480 height=320 buswidth=8 init=-1,0x01,-2,5,-1,0xD0,0x07,0x42,0x18,-1,0xD1,0x00,0x07,0x18,-1,0xD2,0x01,0x02,-1,0xC0,0x10,0x3B,0x00,0x02,0x11,-1,0xC5,0x03,-1,0x36,0x28,-1,0x21,0x21,-1,0x3A,0x55,-1,0x11,-2,120,-1,0x29,-2,20,-3
```

You also need to edit /boot/config.txt and uncomment the line with spi to allow spi interface.


Important:

If your screen shows reverse color,remove ```-1,0x21,0x21,``` from the init code. So the following will be added or execute instead.

For execute:
```
sudo modprobe flexfb width=480 height=320 buswidth=8 init=-1,0x01,-2,5,-1,0xD0,0x07,0x42,0x18,-1,0xD1,0x00,0x07,0x18,-1,0xD2,0x01,0x02,-1,0xC0,0x10,0x3B,0x00,0x02,0x11,-1,0xC5,0x03,-1,0x36,0x28,-1,0x3A,0x55,-1,0x11,-2,120,-1,0x29,-2,20,-3
```

```
sudo modprobe fbtft_device name=flexpfb gpios=dc:18,reset:7,wr:17,cs:4,db00:22,db01:23,db02:24,db03:10,db04:25,db05:9,db06:11,db07:8
```

For fbtft.conf:

```
options fbtft_device name=flexpfb gpios=dc:18,reset:7,wr:17,cs:4,db00:22,db01:23,db02:24,db03:10,db04:25,db05:9,db06:11,db07:8
```

```
options flexfb width=480 height=320 buswidth=8 init=-1,0x01,-2,5,-1,0xD0,0x07,0x42,0x18,-1,0xD1,0x00,0x07,0x18,-1,0xD2,0x01,0x02,-1,0xC0,0x10,0x3B,0x00,0x02,0x11,-1,0xC5,0x03,-1,0x36,0x28,-1,0x3A,0x55,-1,0x11,-2,120,-1,0x29,-2,20,-3
```

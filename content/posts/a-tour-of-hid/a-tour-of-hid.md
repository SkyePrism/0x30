---
title: "How to parse a HID report descriptor"
date: 2025-01-29T22:21:15+01:00
draft: false
toc: true
images:
  - /images/HIDUsage.png
tags:
  - HID
---

## Introduction

HID stands for **H**uman **I**nterface **D**evice. It's a device class definition to replace PS/2, was designed for USB but is bus-agnostic.

The HID specification specifies the structure of hid reports, which will be sent from the *device* (probably your microcontroller) to the *host* (probably a PC).

A HID report is structured like so:

```
0x05, 0x0d,                         // USAGE_PAGE (Digitizers)          
0x09, 0x04,                         // USAGE (Touch Screen)             
0xa1, 0x01,                         // COLLECTION (Application)         
0x85, 0x01,                         //   REPORT_ID (Touch)              
0x09, 0x22,                         //   USAGE (Finger)                 
0xa1, 0x02,                         //     COLLECTION (Logical)  
0x09, 0x42,                         //       USAGE (Tip Switch)           
0x15, 0x00,                         //       LOGICAL_MINIMUM (0)          
0x25, 0x01,                         //       LOGICAL_MAXIMUM (1)          
0x75, 0x01,                         //       REPORT_SIZE (1)              
0x95, 0x01,                         //       REPORT_COUNT (1)             
0x81, 0x02,                         //       INPUT (Data,Var,Abs) 
0x95, 0x07,                         //       REPORT_COUNT (7)  
0x81, 0x03,                         //       INPUT (Cnst,Ary,Abs)
0x75, 0x08,                         //       REPORT_SIZE (8)
0x09, 0x51,                         //       USAGE (Contact Identifier)
0x95, 0x01,                         //       REPORT_COUNT (1)             
0x81, 0x02,                         //       INPUT (Data,Var,Abs) 
0x05, 0x01,                         //       USAGE_PAGE (Generic Desk..
0x26, 0xff, 0x0f,                   //       LOGICAL_MAXIMUM (4095)         
0x75, 0x10,                         //       REPORT_SIZE (16)             
0x55, 0x0e,                         //       UNIT_EXPONENT (-2)           
0x65, 0x13,                         //       UNIT(Inch,EngLinear)                  
0x09, 0x30,                         //       USAGE (X)                    
0x35, 0x00,                         //       PHYSICAL_MINIMUM (0)         
0x46, 0xb5, 0x04,                   //       PHYSICAL_MAXIMUM (1205)
0x95, 0x02,                         //       REPORT_COUNT (2)         
0x81, 0x02,                         //       INPUT (Data,Var,Abs)         
0x46, 0x8a, 0x03,                   //       PHYSICAL_MAXIMUM (906)
0x09, 0x31,                         //       USAGE (Y)                    
0x81, 0x02,                         //       INPUT (Data,Var,Abs)
0x05, 0x0d,                         //       USAGE_PAGE (Digitizers)
0x09, 0x48,                         //       USAGE (Width)                
0x09, 0x49,                         //       USAGE (Height)               
0x81, 0x02,                         //       INPUT (Data,Var,Abs)
0x95, 0x01,                         //       REPORT_COUNT (1)
0x55, 0x0C,                         //       UNIT_EXPONENT (-4)           
0x65, 0x12,                         //       UNIT (Radians,SIROtation)        
0x35, 0x00,                         //       PHYSICAL_MINIMUM (0)         
0x47, 0x6f, 0xf5, 0x00, 0x00,       //       PHYSICAL_MAXIMUM (62831)      
0x15, 0x00,                         //       LOGICAL_MINIMUM (0)      
0x27, 0x6f, 0xf5, 0x00, 0x00,       //       LOGICAL_MAXIMUM (62831)        
0x09, 0x3f,                         //       USAGE (Azimuth[Orientation]) 
0x81, 0x02,                         //       INPUT (Data,Var,Abs)  
0xc0,                               //     END_COLLECTION
[...] AND SO ON...
0xc0,                               // END_COLLECTION
```

This HID report looks intimidating at first, so let's break this down.

## Analyzing the first three rows of this HID report

![HID Usage](/images/HIDUsage.png)


### 0x05, 0x0d

The first byte describes the tag, type, and size of the rest of the data in the line.

These tags, types, and sizes are defined in the HID specification.

Take, for example, 0x05. We break it down into binary form (0000 01 01).

Firstly we have to look at the 0th bit and the 1st bit, which define the size of the data upcoming in bytes. The size in this case is 0b01, which equates to 1, meaning we are expecting 1 byte of information following. (NOTE: Keep in mind that the size of the data is restricted to 0, 1, 2, or 4 (yes 4) bytes long. This also has the implication that 0b11 means FOUR bytes following, not THREE). 

Then, the 2nd and 3rd bit define the *type* of the upcoming data. These are defined in the HID specification like so:

| Value | Type Meaning |
|-------|-------------|
| 00    | Main Item   |
| 01    | Global Item |
| 10    | Local Item  |
| 11    | Reserved    |

Pretty simple, right?

The next four bits in this byte indicate the *tag*. So, let's take our type in this example (01 == Global Item), and manually look through the ``Device Class Definition for HID 1.11`` to see what our tag means.

In section ``6.2.2.7 Global Items`` of the ``Device Class Definition for HID 1.11``, we see that our tag (0000) means "Usage Page". So what have we learned so far? We've learned that the first byte of HID report is 0x05, expanding into 0000 01 01, telling us that we are going to be reading 1 byte of data, which has type 01 (Global Item), and Tag 0000 (Usage Page).

Next up, let's read our data section. Thankfully, it's only 1 byte, which has the value *0x0D*. This is where we move to our next document, the HUT (HID Usage Tables).

Simply open up the HUT and CTRL+F to find 0x0D, which takes us to the Digitizers Page.

### 0x09, 0x04

Let's keep reading. Our next line in the HID report is 0x09, translating to 0000 10 01, meaning we have 1 byte of type local item, and tag 0000. If we follow the same process outlined above, we can see that the ``Device Class Definition for HID 1.11`` section ``6.2.2.8`` telling us that 0000 tag for a Local Item (10) means that we are about to read a "usage". Our usage is 0x04, and when moving to the HID Usage Tables Digitizers Page section, we can see that 0x04 is defined as a Touch Screen.

####0xa1, 0x01

One last time. Let's see what 0xA1 is in binary form; it's 1010 00 01.

Again we will have 1 byte of data (0b01), this time the type will be a Main Item (0b00), and our tag is 1010 (Looking in ``6.2.2.4 Main Items`` of the ``Device Class Definition for HID 1.11``, we can see that this means collection). Also in this section, describes that a collection with data byte 0x01 (which is what we have) is an "Application" type collection.

## References

Please see:
- [Device Class Definition for HID 1.11](https://www.usb.org/sites/default/files/hid1_11.pdf)
- [HID Usage Tables for USB](https://usb.org/sites/default/files/hut1_5.pdf)
- [Sample Report Descriptors](https://learn.microsoft.com/en-us/windows-hardware/design/component-guidelines/touchscreen-sample-report-descriptors)

Also, I want to mention a great tool for automatically parsing HID Report Descriptors, which is [here](https://eleccelerator.com/usbdescreqparser/). Try it out with this HID Report Descriptor:

``050115000904a1018530050105091901290a150025017501950a5500650081020509190b290e150025017501950481027501950281030b01000100a1000b300001000b310001000b320001000b35000100150027ffff0000751095048102c00b39000100150025073500463b0165147504950181020509190f2912150025017501950481027508953481030600ff852109017508953f8103858109027508953f8103850109037508953f9183851009047508953f9183858009057508953f9183858209067508953f9183c0``


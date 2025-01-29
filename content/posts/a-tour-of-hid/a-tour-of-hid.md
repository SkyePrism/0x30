---
title: "A Tour of HID"
date: 2024-07-07T23:10:15+01:00
draft: false
toc: false
images:
tags:
  - untagged
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

The first byte describes the tag, type, and size of the rest of the data in the line.

These tags, types, and sizes are defined in the HID specification.

Take, for example, 0x05. We break it down into binary form (<span style="color:blue">0000</span> <span style="color:red">01</span> <span style="color:green">01</span>).

Firstly we have to look at the 0th bit and the 1st bit, which define the size of the data upcoming in bytes. The size in this case is 0b01, which equates to 1, meaning we are expecting 1 byte of information following. (NOTE: Keep in mind that the size of the data is restricted to 0, 1, 2, or 4 (yes 4) bytes long. This also has the implication that 0b11 means FOUR bytes following, not THREE). 

Then, the 2nd and 3rd bit define the *type* of the upcoming data. These are defined in the HID specification like so:

| Value | Type Meaning |
|-------|-------------|
| 00    | Main Item   |
| 01    | Global Item |
| 10    | Local Item  |
| 11    | Reserved    |

Pretty simple, right?

The next four bits in this byte indicate the *tag*. So, let's take our type in this example (01 == Global Item), and manually look through the Device Class Definition for HID 1.11 to see what our tag means.

In section ``6.2.2.7 Global Items`` of the Device Class Definition for HID 1.11, we see that our tag (0000) means "Usage Page". So what have we learned so far? We've learned that the first byte of HID report is 0x05, expanding into 0000 01 01, telling us that we are going to be reading 1 byte of data, which has type 01 (Global Item), and Tag 0000 (Usage Page).

Next up, let's read our data section. Thankfully, it's only 1 byte, which has the value *0x0D*. This is where we move to our next document, the HUT (HID Usage Tables).

Simply open up the HUT and CTRL+F to find 0x0D, which takes us to the Digitizers Page.

Let's keep reading. Our next line in the HID report is 0x09, translating to 0000 10 01, meaning we have 1 byte of type local item, and tag 0000. If we follow the same process outlined above, we can see that the Device Class Definition for HID 1.11 section 6.2.2.8 telling us that 0000 tag for a Local Item (10) means that we are about to read a "usage". Our usage is 0x04, and when moving to the HID Usage Tables Digitizers Page section, we can see that 0x04 is defined as a Touch Screen.



# Govee-H6072-Reverse-Engineering
My attempt at reverse engineering the Govee H6072 BLE commands.

------
# A Message to Govee

>In the U.S., Section 103(f) of the Digital Millennium Copyright Act (DMCA) [(17 USC § 1201 (f) - Reverse Engineering)](https://www.law.cornell.edu/uscode/text/17/1201) specifically states that it is legal to reverse engineer and circumvent the protection to achieve interoperability between computer programs (such as information transfer between applications). Interoperability is defined in paragraph 4 of Section 103(f).
>
>It is also often lawful to reverse-engineer an artifact or process as long as it is obtained legitimately. If the software is patented, it doesn't necessarily need to be reverse-engineered, as patents require a public disclosure of invention. It should be mentioned that, just because a piece of software is patented, that does not mean the entire thing is patented; there may be parts that remain undisclosed.


Govee I love your product, and I mean no harm in releasing this information. I only did this as a side project so I can control the lighting strips from my own app that runs in my car. I decided to publish my findings and protocol reverse engineering so that anyone else who is looking to do the same might have a place to start. Long story short, __please don't sue me, or DMCA this repo__. If you wish for me to take it down, __please email me or leave a issue on this repo stating that you would like it to be removed, and I will happily do so__.

With all that out of the way, on to the documentation!

## Characteristics findings 

Only tested with nrf. In my limited testing, could not get it to work with `gatttool`. But `gatttool` may not be the way to go due to deprecation - worth taking a look at `bluetoothctl` instead.

`gatttool -t random -b (mac) --char-write-req -a 0x0014 -n (command)`


| Type           | Unformatted UUID                 | Formatted UUID                       |
|----------------|----------------------------------|--------------------------------------|
| Service        | 000102030405060708090a0b0c0d1910 | 00010203-0405-0607-0809-0a0b0c0d1910 |
| Characteristic | 000102030405060708090a0b0c0d2b11 | 00010203-0405-0607-0809-0a0b0c0d2b11 |



### General 
| Command         | Value                                    | Notes
|-----------------|------------------------------------------|---------------------------|
| Keep Alive              | 3301010000000000000000000000000000000033 | Send every second.         |
| On              | 3301010000000000000000000000000000000033 |                           |
| Off             | 3301000000000000000000000000000000000032 |                           |
| Switch to color mode | 3305150100000000000000000000000000000022 | TODO: test out switching colors w/ and w/o gradients. |
| Switch to scene scene | 3305040500000000000000000000000000000037 | Switches to last scene set by user via app. |


### Scenes
| Scene name         | Value                                    
|-----------------|------------------------------------------|
| Nightlight | 3305040200000000000000000000000000000030 |
| Romantic | 3305040700000000000000000000000000000035 |

Thank you to BeauJBurroughs, egold555,Freemanium, and ddxtanx for the initial findings.

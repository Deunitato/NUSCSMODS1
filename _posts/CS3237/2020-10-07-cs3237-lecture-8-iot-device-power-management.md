---
layout: post
published: false
title: 'CS3237 - Lecture 8 : IoT Device Power management'
---
# Powering up IOT Devices
- Wired power delivery 
	- Energy is practically unlimited and little risk of being interrupted
    - Installation is difficult and costly
    - limited mobility of the sensors
- Battery power
	- offers greater mobility of the sensors
    - power budget limits the functionality of the sensors
    - Depending on the location and scale, replacing batteries might be challenging
- Wireless power delivery
	- Simplified installation and maintence of IOT devices
    - Delivered power can be more than what is available to devices
- Energy harvesting
	-  Process by which energy is derived from external sources
    - Energy source for energy harvesters is called ambient anergy whcih is present as ambient background and freely available
    - Energy havesting device converts the captured ambient energy into electrical energy and powers IOT devices


# Power vs Energy
Energy is very important for our battery power devices..
- Measured in Joules
- Power: Rate of energy consumption measured in watt
- Avg power = energy/execution time

- Reducing avg power reduces energy provided execution time is not increased


## Other metrics:
- Total energy and avg power are same in both cases
- Peak power: Can cause dmg if exceed treshold
- Temporal power profile: Varying power dissipations may reduce lifetime of a battery
- Sharp change in power dissipation may upset logic voltage levels causing erroneous circuit behavior

## CMOS
![CS3237-8-1.PNG]({{site.baseurl}}/img/CS3237-8-1.PNG)

Short circuit current:
From VDD to ground

Power
![CS3237-8-2.PNG]({{site.baseurl}}/img/CS3237-8-2.PNG)

- ACV^2f and AVIf is dynamic power
	- When the capacitor is charging
    - f: The frquency of the gate switching
    - AF : Some circuit might switch some might not, this capture the switch activity
    - AVI: Short circuit component
    
- VI is the static power
	- No matter what the transitors is always disppating power even if nothing is being done

### Static power: Leakage power
- Subtreshold leakage (dominates when inactive)
	- Caused by unwanted subthreshold current in the transitors channel when the treansitores is turned off
    - As supply voltage is reduce, the treshold volatage also reduce. As gate volatage is no loner significantly below threshold voltage in the off state, subtreshold conduction has increased substantially
    

![CS3237-8-3.PNG]({{site.baseurl}}/img/CS3237-8-3.PNG)
- Lower cell delay with a lower threshold cell voltage: It takes less time for the gate to charge up with lower threshold


## Power dissipation
- Active dynamic power dominated in the pass
- Leakage power has become mroe impt with decreasing feature size
- LEakage pwoer increases with increasing temperature

## Clock Gating: Dynamic power reduction
- Disable clock to blocks when not in use
- Save dyn power but not leakage power

![CS3237-8-4.PNG]({{site.baseurl}}/img/CS3237-8-4.PNG)

Power gating:
- Switch of power to the idel component
- Save btoh dynamic and leakage pwoer

![CS3237-8-5.PNG]({{site.baseurl}}/img/CS3237-8-5.PNG)
- the sleep controls


Voltage and frequency scaling:
- Reduce clock frequency
	- Decreases the avg power but increases execution time: Energy remains the same
    - Better metric for low power processor: MIPS/W === million instructions per sec per watt
- Reduce supply voltage
	- Reduction in voltage thereshold, this would lead to higher leakage power
    	![CS3237-8-7.PNG]({{site.baseurl}}/img/CS3237-8-7.PNG)

	- Reduction in f:
    	![CS3237-8-6.PNG]({{site.baseurl}}/img/CS3237-8-6.PNG)
        
- Reduce activity



### Dynamic voltage and Frequency scaling (DVFS)

- Take adv of the wide oepratung range of CMOS circuit
- Relationship between power suply voltage, operating voltage, freqyuency

![CS3237-8-8.PNG]({{site.baseurl}}/img/CS3237-8-8.PNG)


- The clock and power supply are generated by circuits that can supply a range of values
- These circuits generally operate at discrete points rather than continously varying value
- Both the clock generator and volt generator are operated by a controller that determines when the clock freq and voltage will change and by how much


# Device power management
- Wake up and shut down delay
- Shuting down and waking up takes up power (Ensure that we can sleep long enough to avoid the overhead)

![CS3237-8-9.PNG]({{site.baseurl}}/img/CS3237-8-9.PNG)

# Break even time
- The minimum idle period to save power
- If we know our break even period, try to predict when we can go to sleep
![CS3237-8-10.PNG]({{site.baseurl}}/img/CS3237-8-10.PNG)

- Eo: shut down energy + wake up energy

# TI-RTOS Power policy
- Make decision regarding power saving swgeb CPU is idel
- CPU is considered idle when the OS idel loop is executed, all applicaiton threads are blocked pening I/O, or blocked pending some other application event
- Factors considered in power policy:
	- Sleep states allowed under constrains declared to the power management
    - time until the next OS schedule processing
    - Transition latency in and out of allowed sleep states
- Power policy selects the deepest power saving state that meets all the considered criteria

## Power policy events
- pp triggers the appropriate sleep state
- Next interrupt wakes up the CPU
- Interrupt service routine runs
- ISR either performs the processing directly or makes ready an application trhead that was previously blocked
- Once the processing completes, OS idle loops runs again
- pp is called again to choose the next power saving state

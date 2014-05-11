# Report - Respitarion from ECG

## Goal

Our goal is to use Bitalino to acquire an electrocardiogram (ECG) and estimate the Respiratory Rate (RR) from that signal.

## Team

* Ana Esteves
* Débora Pera
* Gonçalo Telo

## Process

We passed by the several steps:

### Step 1 - First recording

During the first stage/step we recorded data twice, using different sampling frequencies (1000 and 100 samples per second).

This was the first signal extracted from bitalino, with a sampling frequnecy of 1000 samples per second:

![first signal](https://github.com/GTelo/ElPhy_ShortProject/blob/master/figure_1.png?raw=true)

This was the second signal extracted from bitalino, with a sampling frequnecy of 100 samples per second:

![first signal](https://github.com/GTelo/ElPhy_ShortProject/blob/master/figure_2.png?raw=true)


### Step 2 - Improved signal

...


### Step 3 - Defining the signal processing stages

In order to achieve our final goal, we followed the stages bellow:
1. Data conversion;
2. Low-Pass filter at 40 Hz;
3. ECG R-peak detection;
4. Interpolation of a baseline, created by the R-peaks;
5. Select the local maximums;
6. Calculate the time between maximums and define the RR value;
7. Estimate RR evolution.

### Step 4 - Real time tests

### Step 5 - The final outcome

## Code example


``` python
from pylab import *
from scipy import *
from novainstrumentation import *

data = loadtxt('teste.txt')

ECGb=data[:,-1]
figure()
plot(ECGb)
xlabel('Sample #')
ylabel('ADC')
show()

Fs = 1000.
t = arange(len(ECGb))/Fs

nbits=10
Vcc=3.3

G=1000.

ECGv = (ECGb/(2**nbits-1)-.5)*Vcc/2./G
ECGmv = (ECGv-ECGv.mean())*G

figure()
plot(t, ECGmv)
xlabel('t(s)')
ylabel('mV')
show()

#(...)

print "Respiratory Rate: ",
print mean(RR)


```




## Video of the experiment


## Take ways
The major discoveries made in this project were: 

* Scientific enlightment 
* Eureka moment
* Singularity vision
* Trasncendental perception


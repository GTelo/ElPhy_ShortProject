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

During the first step we recorded data twice, using different sampling frequencies (1000 and 100 samples per second).

This was the first signal extracted from bitalino, with a sampling frequency of 1000 samples per second:

![first signal](https://github.com/GTelo/ElPhy_ShortProject/blob/master/figure_1.png?raw=true)

This was the second signal extracted from bitalino, with a sampling frequency of 100 samples per second:

![first signal](https://github.com/GTelo/ElPhy_ShortProject/blob/master/figure_2.png?raw=true)



### Step 2 - Defining the signal processing stages

In order to achieve our final goal, we followed the stages bellow:

1. Data conversion;
2. Low-Pass filter at 40 Hz;
3. ECG R-peak detection;
4. Interpolation of a baseline, created by the R-peaks;
5. Select the local maximums;
6. Calculate the time between maximums and define the RR value;
7. Estimate RR evolution.

### Step 3 - The final outcome

![first signal](https://github.com/GTelo/ElPhy_ShortProject/blob/master/figure_2_processed.png?raw=true)

RR = 25.3669827656

## Code example


``` python
from pylab import *
from scipy import interpolate
from novainstrumentation import *

data = loadtxt('d1_e2.txt')

ECGb=data[:,-1]

Fs = 100.
t = arange(len(ECGb))/Fs

nbits=10
Vcc=3.3

G=1000.

ECGv = (ECGb/(2**nbits-1)-.5)*Vcc/2./G
ECGmv = (ECGv-ECGv.mean())*G

#figure()
#plot(t, ECGmv)
#xlabel('t(s)')
#ylabel('mV')
#show()

signal = ECGmv

#f,s = plotfft(signal,Fs)
#figure()
#plot(f,s)

nsignal = lowpass(signal, 40,order = 3,fs=Fs, use_filtfilt = False)

x = 0.8*max(nsignal)

R = []
pos = []

for r in peaks(nsignal):
    if nsignal[r] > x : 
        R.append(nsignal[r])
        pos.append(r)

curve = interpolate.spline(pos,R,arange(0,len(t),1),order=5)

i1 = pos[0]
i2 = pos[-1]
resp_signal = curve[i1:i2]
rt = t[i1:i2]


peaks = peakdelta(resp_signal,0.001,rt)
#Guarda apenas os máximos
tr = []
mr = []
for i in peaks[0]:
    tr.append(i[0])
    mr.append(i[1])
    
figure()
subplot(2,1,1)
plot(t,nsignal,'b',rt,resp_signal,'g',tr,mr,'ro')
subplot(2,1,2)
plot(rt,resp_signal,'g',tr,mr,'ro')

#Respiratory Rate
RR = []
RRt = []
for i in tr:
    if i == tr[0]:
        k = i
    else:
        RR.append(60/(i-k))
        RRt.append(i);
        k = i

#Interpolation of the evolution od the Respiration Rate
nfs = (1/(min(RR)/60))/10
newt = arange(RRt[0],max(RRt),nfs)
RRevo = interpolate.spline(RRt,RR,newt,order=3)
figure()
plot(newt, RRevo,'b')
title('Evolucao do ritmo respiratorio')
ylabel('RR (min)')
xlabel('Tempo')
grid()        
        

print "Respiratory Rate: ",
print mean(RR)


```




## Take ways
The major discoveries made in this project were: 

* Scientific enlightment 


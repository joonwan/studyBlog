
---

## How can we detect eddy using SLA?

 Eddies can be broadly categorized into two types as follows.

1. Warm Eddy
2. Cold Eddy

  First, Warm Eddies are eddies formed as a result of  clockwise ocean currents, driven by the currents, driven by the Coriolis force. Due to the influence of the Coriolis force, water is pushed toward the center of the eddy. As a result, sea level in the vicinity of Warm Eddies appears higher than the average.
  
  On the other hand, Cold Eddies rotate counterclockwise, causing water to move away from the center of the eddy due to the Coriolis force. This leads to lower sea levels measured in the vicinity of Cold Eddies compared to the surrounding area.

 So, let's use SLA data, which represents height differences, to identify Eddies!!


## Detecting Algorithm


### Thresholding for boundary value setting


#### 1. plot sla data

 Plotting the SLA data for a specific date yields the following result

![[Pasted image 20231017151811.png]]

In the image above, below Japan, you can see circular areas where the sea level is significantly lower or higher than the average. These circular areas can be inferred as Warm Eddies and Cold Eddies, as mentioned earlier. So, the blue areas below the average sea level represent Cold Eddies, while the high yellow areas represent Warm Eddies.


#### 2.  Setting the threshold value

To delineate the circular regions, boundary values need to be set. The SLA value for the blue boundary areas on the image is estimated to be around -3.5, and the boundary value for the yellow areas is estimated to be 3.

```matlab
threshhold_c = -0.35; %cold eddy
threshhold_w = 0.3;   %warm eddy
```


#### 3. Drawing contour lines

To use the previously set boundary values, we can utilize the 'contourm' function in MATLAB.

```matlab
data = [threshhold_w,threshhold_c]
contourm(yi,xi,sla(:,:,i),data,'LineColor','k','LineWidth',2);
```

The image below represents the boundary values as contour lines.

![[Pasted image 20231017154656.png]]

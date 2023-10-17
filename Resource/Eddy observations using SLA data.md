
---

## How can we detect eddy using SLA?

 Eddies can be broadly categorized into two types as follows.

1. Warm Eddy
2. Cold Eddy

  First, Warm Eddies are eddies formed as a result of  clockwise ocean currents, driven by the currents, driven by the Coriolis force. Due to the influence of the Coriolis force, water is pushed toward the center of the eddy. As a result, sea level in the vicinity of Warm Eddies appears higher than the average.
  
  On the other hand, Cold Eddies rotate counterclockwise, causing water to move away from the center of the eddy due to the Coriolis force. This leads to lower sea levels measured in the vicinity of Cold Eddies compared to the surrounding area.

 So, let's use SLA data, which represents height differences, to identify Eddies!!.


## Detecting Algorithm


### Thresholding for boundary value setting

 Plotting the SLA data for a specific date yields the following result

![[Pasted image 20231017151811.png]]

In the image above, below Japan, you can see circular areas where the sea level is significantly lower or higher than the average.
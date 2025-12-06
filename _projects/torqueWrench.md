---
layout: project
title: Torque Wrench Analysis
description: Advanced CAD Project
technologies: [Autodesk Fusion]
image: /assets/images/torquewrench.png
---

The goal of this project was to design and verify a basic torque wrench. The requirements to meet were that for a 600inlb torque, we needed to attain:<br>
-At least 1.0mV/V output at the strain gauge <br>
-A safety factor Xo of 4 for brittle failure and ductile failure <br>
-A Safety factor Xk of 2 for crack growth from a crack of .04in (1mm) <br>
-A fatigue stress safety factor Xs of 1.5 <br>
-A material that is only steel, aluminum, or titanium alloy. <br>
<br>
To do this, we designed a MATLAB script to calculate the safety factors given certain materials and geometry considerations. From the MATLAB script, we chose a design with the following factors to attain the requirements: <br>
-M = 600; <br>
-L = 16; <br>
-F = M / L;<br>
-ro=.4;<br>
-ri=.3;<br>
-c = ro;        % Distance from center of drive to center of strain gauge (inches)<br>
-E = 16.5E6;     % Young's modulus (psi)<br>
-ou = 138.0E3;   % Ultimate Tensile Strength (psi) - Use for safety factor<br>
-KIC = 68.3E3;   % Fracture toughness (psi * sqrt(in))<br>
-S = 74.0E3; % Fatigue strength for 10^6 cycles (psi)<br>
-name = 'Ti-6Al-4V'; % material name<br>
<br>
This provided us with the results:<br>
"Max Stress: "    "17461.5709"<br>
"Max Deflection: "    "0.22577"<br>
"Strength safety factor: "    "7.9031"<br>
"Fracture safety factor: "    "11.034"<br>
"Fatigue safety factor: "    "4.2379"<br>
"mV/V: "    "1.0583"<br>
<br>
With this, we CADed the wrench in Fusion 360, with a few changes to reduce stress concentration, including a thicker head and fillets where we assume clamped boundary conditions on the wrench head. 
<br>
![CAD 1]({{ "/assets/images/CAD1.png" | relative_url }}){: .inline-image-r style="width: 200px"}
![CAD Dimensions]({{ "/assets/images/CADDim.png" | relative_url }}){: .inline-image-r style="width: 400px"}
![CAD 3]({{ "/assets/images/CADfillet.png" | relative_url }}){: .inline-image-r style="width: 200px"}
![CAD 4]({{ "/assets/images/CADLong.png" | relative_url }}){: .inline-image-r style="width: 400px"}
<br>
The Material we chose (Ti-6Al-4V) is commonly used titanium alloy known for its exceptional strength to weight ratio, making it a top choice for demanding structural applications such as this torque wrench. Compared with the baseline M42 tool steel, it delivers similar strength at nearly half the density, but also with a much larger ductility (lower Youngs Modulus). This ductility allows us to fulfill the deflection requirement with a thicker cross section design, allowing us to maintain strength with deflection. 
<br>
Once we had the design, we were able to export it into ANSYS and perform a static structural analysis:
<br>
For the boundary conditions, we assumed a clamped fixed boundary at the socket, and applied a load at the end of the torque wrench, F=37.5lb, corresponding to the moment of 600inlb required. 
<br>
![Photo of ANSYS setup]({{ "/assets/images/boundary.png" | relative_url }}){: .inline-image-l}
<br>
Diagram of the normal strain contours: <br>
![Photo of normal strain contours]({{ "/assets/images/strains.png" | relative_url }}){: .inline-image-l}
<br>

Contour Plot of the maximum principal stresses: <br>
![Photo of principle stresses]({{ "/assets/images/principle.png" | relative_url }}){: .inline-image-l}
![Photo of maxnormal]({{ "/assets/images/maxnormal.png" | relative_url }}){: .inline-image-l}
<br>

<br>
Our maximum normal stress was 68952, which is much higher than the predicted stress from the hand calcs. This is likely due to a large stress concentration forming at the end of the clamped boundary condition. This creates the perhaps unrealistic load in ANSYS. Adding in a probe in the area around the linear concentration, even still where the concentration should be, shows a more expected normal stress of 21,335psi. This creates a factor of safety in the normal stress of 6.47, which is still above our design constraint of 4. 
<br>
The maximum deflection of the torque wrench was .27366in, which is slightly larger than our predicted deflection of .22577in. However, this is still quite close to what was expected from a simple cantilever beam calculation. 
<br>
![Photo of max deflection]({{ "/assets/images/maxdeflection.png" | relative_url }}){: .inline-image-l}
<br>
The increased deflection also should increase the measured strain at the strain probes. From a half bridge probe design on either side of the wrench head, it shows strains that read 3.9399e-004 and -3.9402e-004, which leads to a torque wrench sensitivity of .794mV/V. This is less than the allowable range, and is likely due to the increased material that we added towards the socket, leading to a lower strain in the area. 
<br>
![Photo of probes]({{ "/assets/images/probes.png" | relative_url }}){: .inline-image-l}
<br><br>
We chose a strain gauge with a slightly larger gauge factor (~2) than the baseline gauge. To get this, we chose the Omega SGD-3/350-LY11. It is a linear strain gauge with 3mm grids, and 350ohms. It is a general use foil strain gauge, which is reported to have the gauge factor ~2. Having 3mm grids, it is able to be attached easily onto either side of our cylindrial cross section on the outside. 
<br>
![gauge]({{ "/assets/images/gauge.png" | relative_url }}){: .inline-image-r style="width: 100px"}
---
layout: project
title: Torque Wrench Analysis
description: Advanced CAD Project
technologies: [Autodesk Fusion]
image: /assets/images/torquewrench.png
---

The goal of this project was to design and verify a basic torque wrench. The requirements to meet were that for a 600inlb torque, we needed to attain:
-At least 1.0mV/V output at the strain gauge
-A safety factor Xo of 4 for brittle failure and ductile failure
-A Safety factor Xk of 2 for crack growth from a crack of .04in (1mm)
-A fatigue stress safety factor Xs of 1.5
-A material that is only steel, aluminum, or titanium alloy.

To do this, we designed a MATLAB script to calculate the safety factors given certain materials and geometry considerations. From the MATLAB script, we chose a design with the following factors to attain the requirements:
-M = 600; 
-L = 16; 
-F = M / L;
-ro=.4;
-ri=.3;
-c = ro;        % Distance from center of drive to center of strain gauge (inches)
-E = 16.5E6;     % Young's modulus (psi)
-ou = 138.0E3;   % Ultimate Tensile Strength (psi) - Use for safety factor
-KIC = 68.3E3;   % Fracture toughness (psi * sqrt(in))
-S = 74.0E3; % Fatigue strength for 10^6 cycles (psi)
-name = 'Ti-6Al-4V'; % material name

This provided us with the results:
"Max Stress: "    "17461.5709"
"Max Deflection: "    "0.22577"
"Strength safety factor: "    "7.9031"
"Fracture safety factor: "    "11.034"
"Fatigue safety factor: "    "4.2379"
"mV/V: "    "1.0583"

With this, we CADed the wrench in Fusion 360, with a few changes to reduce stress concentration, including a thicker head and fillets where we assume clamped boundary conditions on the wrench head. 

![CAD 1]({{ "/assets/images/CAD1.png" | relative_url }}){: .inline-image-r style="width: 200px"}
![CAD Dimensions]({{ "/assets/images/CADDim.png" | relative_url }}){: .inline-image-r style="width: 200px"}
![CAD 3]({{ "/assets/images/CADfillet.png" | relative_url }}){: .inline-image-r style="width: 200px"}
![CAD 4]({{ "/assets/images/CADLong.png" | relative_url }}){: .inline-image-r style="width: 200px"}

The Material we chose (Ti-6Al-4V) is commonly used titanium alloy known for its exceptional strength to weight ratio, making it a top choice for demanding structural applications such as this torque wrench. Compared with the baseline M42 tool steel, it delivers similar strength at nearly half the density, but also with a much larger ductility (lower Youngs Modulus). This ductility allows us to fulfill the deflection requirement with a thicker cross section design, allowing us to maintain strength with deflection. 

Once we had the design, we were able to export it into ANSYS and perform a static structural analysis:

For the boundary conditions, we assumed a clamped fixed boundary at the socket, and applied a load at the end of the torque wrench, F=37.5lb, corresponding to the moment of 600inlb required. 

![Photo of ANSYS setup]({{ "/assets/images/old-radio.jpg" | relative_url }}){: .inline-image-l}

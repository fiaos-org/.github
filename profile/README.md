# FiaOS ETSci+

## Official GitHub

<img align="right" width="100" height="100" alt="umbrella-logo-clean-removebg-preview" src="https://github.com/user-attachments/assets/ea20be3a-9e4f-4322-be9f-7671ed063ae3" />

https://github.com/user-attachments/assets/4e026476-d13c-48b1-8e27-3bbbfdf65598

# The Use of Evapotranspiration Algorithm in FiaOS and the Penman-Monteith Equation

[Research Paper](https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-umbrella-project-and-the-penman-monteith-equation/)  
[Author: Neksha DeSilva](https://nekshadesilva.com)  
[Date: August 2025](https://nekshadesilva.com)

## Abstract

Traditional irrigation systems often wait until plants show signs of water stress before responding. The Umbrella Project takes a different approach by using predictive intelligence to anticipate water needs in advance. At the heart of this project is the Penman-Monteith evapotranspiration (ET) equation, which enables the system to deliver exactly the right amount of water based on the forecasted evaporation and transpiration in the next cycle.

This work presents the theoretical background, engineering decisions, and practical solutions that make autonomous irrigation possible through real-time environmental sensing and algorithmic prediction.

---

## Mathematical Derivation of the Simplified Penman-Monteith Equation

The main innovation in the Umbrella Project is the simplification of the FAO-56 Penman-Monteith equation, making it accessible and affordable while maintaining scientific accuracy.

### 1. Starting Point: The Full FAO-56 Penman-Monteith Equation

<img width="852" height="197" alt="image" src="https://github.com/user-attachments/assets/7cf658bb-f38f-4c9a-818e-54d52f5e16ef" />

This equation depends on several measured variables:
- **R_n**: Net Radiation
- **G**: Soil Heat Flux
- **T**: Temperature
- **u_2**: Wind Speed at 2m height
- **e_s, e_a**: Saturation and actual vapor pressures (from humidity and temperature)

---

### 2. Step 1: Negligible Daily Soil Heat Flux (G)

For daily calculations, soil heat flux is minimal because the energy absorbed during the day is almost equal to what is released at night.

**Simplification:**
<img width="170" height="93" alt="image" src="https://github.com/user-attachments/assets/bb34afff-6e86-496e-a752-6899370cdad4" />


So the equation becomes:
<img width="559" height="61" alt="image" src="https://github.com/user-attachments/assets/faeb829a-3751-45c7-aaf3-87f2a9edcdfd" />

---

### 3. Step 2: Wind Speed as a Constant

To avoid the cost of anemometers, wind speed is set as a historical average constant.

**Substitution:**
<img width="548" height="66" alt="image" src="https://github.com/user-attachments/assets/e4d72d1b-5f48-4e11-bc1f-1d70553d1c7b" />


**Code:**
```cpp
const float windSpeed = 1.39;  // m/s
```

Resulting in:
<img width="922" height="155" alt="image" src="https://github.com/user-attachments/assets/1fb412ee-849d-4d04-ba72-29a0f7a5f59a" />


---

### 4. Step 3: Net Radiation from Solar Voltage

Net radiation is estimated using the voltage from a solar panel, removing the need for expensive pyranometers.

**Substitution:**
<img width="336" height="92" alt="image" src="https://github.com/user-attachments/assets/86e08fb7-8eee-4687-9e77-faf90a6ac442" />

**Code:**
```cpp
float getRn(float solarVoltage) {
    return calibratedRadiationEstimate(solarVoltage);
}
```

So:
<img width="1022" height="150" alt="image" src="https://github.com/user-attachments/assets/cc1c9927-a5f0-47a7-aedf-815f62973f56" />

---

### 5. Step 4: Psychrometric Constant as a Pre-Calculated Value

The psychrometric constant depends on pressure, which is altitude-dependent. Using a pre-calculated constant eliminates the need for a barometer.

**Substitution:**
<img width="248" height="67" alt="image" src="https://github.com/user-attachments/assets/9e00da02-ff03-4b8b-9587-80ea58e730d5" />


**Code:**
```cpp
const float gamma = 0.000665 * pressure;  // kPa/°C
```

**Final equation:**
<img width="1098" height="150" alt="image" src="https://github.com/user-attachments/assets/897d9456-7c49-4304-9c0e-368ead4bfc62" />

---

### 6. Conclusion of Derivation

The final equation (above) is used in the Umbrella Project:

| Component           | Source                             |
|---------------------|------------------------------------|
| T, e_s, e_a, Δ      | Real-time sensors (DHT22/BME280)   |
| G                   | Set to 0 (daily average)           |
| u_2                 | Constant from historical data      |
| γ                   | Pre-calculated constant            |
| R_n                 | Estimated using solar panel voltage|

**Result:** About 95% reduction in hardware costs, with ±15% accuracy in ET prediction.

---

## Key Features

- Predictive irrigation: anticipates water needs before stress
- Affordable: total cost under $50 using simple sensors
- Virtual radiation sensor: solar panel + TEG replaces expensive equipment
- Real-time environmental intelligence
- Water savings: 30-40% compared to timer-based systems

---

## Technical Specifications

### Hardware
- Temperature/Humidity Sensor: DHT22 or BME280
- Pressure Sensor: BME280
- Virtual Radiation Sensor: Solar panel + Thermoelectric Generator (TEG)
- Microcontroller: Low-power, embedded
- Power: Solar-powered with battery backup

### Sensor Accuracy
- Temperature: ±0.5°C
- Humidity: ±2-3% RH
- Pressure: ±1 hPa
- Radiation Estimation: ±15%

### System Performance
- ET Prediction Accuracy: ±10-15%
- Irrigation Precision: Milliliter-level
- Water Savings: 30-40%
- Power Consumption: Runs sustainably on solar

---

## System Variants

### GI Variant (First Generation)
- Proof-of-concept
- DHT22 sensors
- Basic ET calculation
- Single-zone irrigation

### Sci+ Variant (Advanced Generation)
- 100+ environmental parameters
- BME280 sensors with pressure sensing
- Machine learning integration
- Multi-zone management
- IoT connectivity
- Adaptive algorithms

---

## Citation

If you use this research, please cite:

```bibtex
@article{desilva2025fiaos,
  title={The Use of Evapotranspiration Algorithm in FiaOS and the Penman-Monteith Equation},
  author={DeSilva, Neksha},
  journal={Research Documentation},
  year={2025},
  month={August},
  url={https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-fiaos-project-and-the-penman-monteith-equation/},
  note={A comprehensive exploration of environmental intelligence, predictive irrigation, and the engineering challenges of implementing the Penman-Monteith equation in autonomous systems}
}
```

**APA Format:**
```
DeSilva, N. (2025, August). The Use of Evapotranspiration Algorithm in fiaos Project and the Penman-Monteith Equation. Retrieved from https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-fiaos-project-and-the-penman-monteith-equation/
```

**IEEE Format:**
```
N. DeSilva, "The Use of Evapotranspiration Algorithm in fiaos Project and the Penman-Monteith Equation," Aug. 2025. [Online]. Available: https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-fiaos-project-and-the-penman-monteith-equation/
```

---

## Resources

- [Full Research Paper](https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-umbrella-project-and-the-penman-monteith-equation/)
- [Presentation PDF](https://nekshadesilva.com/assets/pdf/Precision-Under-Constraint.pdf)
- [Author Website](https://nekshadesilva.com)

---

## References

1. Allen, R. G., Pereira, L. S., Raes, D., & Smith, M. (1998). *Crop evapotranspiration - Guidelines for computing crop water requirements*. FAO Irrigation and drainage paper 56. Food and Agriculture Organization of the United Nations, Rome.

2. Monteith, J. L. (1965). Evaporation and environment. *Symposia of the Society for Experimental Biology, 19*, 205-234.

3. Penman, H. L. (1948). Natural evaporation from open water, bare soil and grass. *Proceedings of the Royal Society of London. Series A, Mathematical and Physical Sciences, 193*(1032), 120-145.

---

## License

This documentation is provided for academic and educational use. For commercial applications, please contact the author.

---

## Author

**Neksha DeSilva**  
Website: [nekshadesilva.com](https://nekshadesilva.com)  
Research Focus: Environmental Engineering, Autonomous Systems, Agricultural Technology

---

## Acknowledgments

This research bridges theoretical environmental science with practical agricultural automation, showing that precision and accessibility are possible through thoughtful engineering and mathematical rigor.

---

*For the full research paper with detailed analysis and technical diagrams, visit the [documentation](https://nekshadesilva.com/docs/the-use-of-evapotranspiration-algorithm-in-umbrella-project-and-the-penman-monteith-equation/).*

---

## Overview

The Umbrella Project is designed for sustainable living, scientific investigation, and autonomous technology. As a compact 10 cm module, it converts rainfall, humidity, temperature, and evapotranspiration into over 100 unique input parameters. With these, the system can generate more than a million real data points, supporting researchers, farmers, and innovators in testing algorithms, validating findings, and exploring the interactions between environment and life.

Version 1.0 marks the beginning. The system is built for interoperability and works across iOS, Windows, web applications, and services like IFTTT. Its communication range extends up to five kilometers, enabling remote connectivity and collaboration in fields such as agriculture, biology, climate science, and geography.

---

## Imagine

Imagine a system that not only irrigates plants but also dispenses nutrients through integrated liquid fertilizer control. Imagine a device that operates for a decade without needing intervention, collecting, processing, and sharing data across scientific networks. Picture a living organism—a plant, a fungus, or an experimental biological model—thriving under its protection even in extreme weather, when water elsewhere is scarce.

The fiaos Project stands for resilience and innovation. It is more than a tool; it is a companion for researchers, a safeguard for living systems, and a foundation for sustainable ecosystems.

---

If you have questions, including about how liquid fertilizer is produced inside the module, please reach out via email.

Email: nekshavs@gmail.com

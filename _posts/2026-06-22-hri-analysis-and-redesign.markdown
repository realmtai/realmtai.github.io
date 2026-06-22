---
layout: post
title: "Human–Robot Interaction Analysis and Redesign"
date: 2026-06-21
img: hospital_robot/oht_cover.png
author: Michael Tai
categories: [Robotics, HRI, Healthcare]
---

# Human–Robot Interaction Analysis and Redesign

## Summary
This post analyzes the Human-Robot Interaction (HRI) of the Moxi robot, an autonomous assistant deployed in healthcare settings to alleviate nursing workloads. Despite its design goals, the Moxi system encountered significant usability and reliability issues, often becoming a burden to the staff it was intended to support.

This study examines the failures in Moxi's HRI, particularly regarding trust, safety, and cognitive load. To address these shortcomings, I propose a next-generation HRI design: an integrated **Overhead Hoist Transport (OHT)** system. By moving logistics to the ceiling and implementing advanced intent recognition and reinforcement learning for adaptive behavior, the proposed system eliminates floor congestion and transforms the robot from a supervised tool into a reliable, invisible utility.

**Keywords:** Human-Robot Interaction, Healthcare Robotics, Autonomous Mobile Robots, Overhead Hoist Transport, Intent Recognition, Reinforcement Learning, Nurse Burnout.

---

## Introduction
In the high-pressure environment of modern healthcare, nursing staff are frequently overwhelmed by repetitive, nonclinical tasks that contribute to burnout and reduce the time available for direct patient care. To address this, Diligent Robotics introduced the Moxi robot, an autonomous mobile assistant designed to navigate hospital corridors and transport medical supplies, lab samples, and medications.

In such a critical domain, effective Human-Robot Interaction (HRI) is paramount; the robot must not only be functionally capable but also seamlessly integrated into the workflow to avoid adding to the cognitive and physical load of already stressed medical professionals. However, as evidenced by early deployments, a gap between the robot's social interface and its operational reliability can lead to a breakdown in trust and user acceptance.

## Existing System Description
The Moxi robot is an autonomous assistant designed to operate within the complex and dynamic environment of healthcare facilities. Its primary purpose is to serve as a support tool for nursing staff by handling repetitive, ancillary "backend" tasks, such as:
- Ferrying medical supplies
- Transporting lab samples and bloodwork
- Delivering medications
- Retrieving patient belongings or toiletries

The primary users are nursing staff and hospital administrators. Moxi employs several social and functional interfaces:
- **Expressive LED eyes:** Used to display symbols (e.g., a heart sign) to indicate task completion.
- **Auditory responses:** A speaker for communication.
- **Robotic arm:** Designed to physically manipulate environment controls like doors and elevator buttons.

Despite these capabilities, Moxi's autonomy in complex layouts has been limited, often requiring human handlers to assist with navigation.

![Moxi robot assisting in a hospital](/assets/img/hospital_robot/moxi1.jpg)

## Analysis of Current HRI Design
The current HRI design of the Moxi robot exhibits a significant gap between its intended purpose and its operational reality. While designed to reduce nursing workload, Moxi often becomes an additional "charge to care for," increasing the cognitive and physical workload of the staff who must act as handlers.

### Key Failures:
1. **Trust:** Trust is severely compromised by the robot's inability to guarantee the timely delivery of critical medical samples. Nurses have expressed concerns that delays in delivery could potentially delay life-saving care.
2. **Situational Awareness:** The robot lacks the ability to perform observational tasks, such as noticing a patient in distress, which is something human staff naturally do while moving.
3. **Safety and Accessibility:** Reports indicate Moxi has physically wedged staff into elevators, creating a hazardous presence in high-stress environments.
4. **User Acceptance:** Nurses may view the deployment of Moxi as a symbolic preference for expensive automation over necessary staffing investments.

Ultimately, Moxi's social interfaces (like the LED eyes) prove superficial when faced with systemic operational failures.

## Proposed Next Generation HRI Design: Overhead Hoist Transport (OHT)
To address these issues, I propose a transition from a floor-based mobile assistant to an integrated **Overhead Hoist Transport (OHT)** system. In a hospital, floor space is a critical resource. By mounting the robotic transport system in the ceiling using a network of monorails and automated hoists, the hospital can reclaim its floor space for patient care and emergency movement.

### System Overview:
- **Infrastructure:** Materials are transported in secure, standardized containers (similar to FOUPs used in semiconductor manufacturing) along optimized overhead routes.
- **Automation:** The system automates the core duties of medical material handlers, ensuring supplies are moved efficiently without needing human handlers.
- **Control:** A central integrated control system manages traffic flow and monitors vehicle health in real-time.

![Conceptual diagram of the Overhead Hoist Transport (OHT) system](/assets/img/hospital_robot/oht.png)

### Enhanced HRI through Safety Barriers:
To ensure safety, the system incorporates a **laser-based robot-human space barrier**. As the OHT system lowers a payload, a virtual laser perimeter is established. The system employs **intent recognition** to analyze the trajectory and speed of any person entering the zone:
- **Intentional Retrieval:** A direct, decelerating approach prompts the system to maintain position and finalize delivery.
- **Accidental Entry:** A high-velocity trajectory triggers an immediate safety pause and auditory alert.

This shift redefines the robot from a "charge to care for" into a reliable, invisible utility.

## Human Intent Recognition and Prediction
To be truly seamless, the OHT system must move from reactive operation to proactive prediction.

### Systemic Intention Inference:
By integrating with Electronic Health Records (EHR) and appointment systems, the OHT can preemptively deliver required supplies to a treatment room before an appointment begins, reducing latency.

### Real-time Action Recognition:
At the point of physical interaction, the system analyzes the trajectory, velocity, and acceleration of humans entering the delivery zone. By employing probabilistic models to handle uncertainty (e.g., hesitations), the system dynamically adjusts its behavior, ensuring safety without inducing operational friction.

## Intelligent Adaptation Using RL and HITL Learning
The long-term efficacy of the OHT system depends on its ability to adapt to the evolving demands of a hospital.

### Human-in-the-Loop (HITL) Framework:
Every interaction at the laser barrier is logged. Staff members can provide qualitative ratings (positive/negative) of these interactions. These ratings serve as reward signals in a **Reinforcement Learning (RL)** loop, allowing the system to refine its delivery policies. For example, if ICU staff find early deliveries obstructive, the RL agent will shift to "just-in-time" arrival for that ward.

### Personalization and Error Correction:
The system can recognize individual user preferences for payload orientation or approach speed. When an anomaly occurs, the system generates a postmortem analysis combining sensor data and human feedback to identify the root cause, ensuring the same mistake is not repeated.

## Expected Impact on Efficiency and User Experience
The proposed OHT system is expected to:
- **Reduce Workload:** By automating the responsibilities of Medical Material Handlers, the physical and cognitive burden on nursing staff is reduced.
- **Improve Safety:** Removing robots from the floor eliminates congestion and the risk of accidents in emergency scenarios.
- **Restore Trust:** Predictable interactions through laser barriers and intent recognition remove the anxiety associated with ground-based robots.
- **Optimize Resources:** By shifting transport tasks to an overhead infrastructure, hospitals can refocus human resources on high-value clinical activities.

## Conclusion
The deployment of Moxi highlights a critical lesson: social interfaces cannot compensate for systemic operational failures. By shifting the paradigm from a visible mobile companion to an integrated, intelligent overhead utility, healthcare facilities can optimize their logistics while empowering medical staff to focus on their primary mission: patient care.

## Final Thoughts

While the technical implementation of an OHT system addresses the operational failures of current mobile robots, it is necessary to ask a more fundamental question: will automation actually solve the chronic labour shortages in healthcare?

The answer is likely more complex than a simple "yes." Hospitals, ideally, operate with the primary goal of improving patient outcomes rather than maximizing profit. Because of this, the incentive structure for adopting expensive, infrastructure-heavy automation differs significantly from that of a highly optimized semiconductor fab or an Amazon fulfillment center. In many healthcare settings, the "bottleneck" is not just the movement of supplies, but the shortage of qualified clinical staff to administer care.

Furthermore, we must consider the broader macroeconomic impact of automation. Robotics will likely penetrate profit-driven industries first, where the ROI is clearest. As these sectors automate, they may displace a significant amount of labour. According to the basic laws of supply and demand, an oversupply of labour in the general market could lead to a decrease in the cost of human labour. Paradoxically, this might make hiring more human staff for hospitals more affordable than investing in multimillion-dollar robotic infrastructure.

Ultimately, the goal of HRI in healthcare should not be to replace humans, but to remove the friction that prevents humans from performing their highest-value work. If automation is used as a tool to lower the cognitive load of nurses rather than a means to cut headcount, it can truly support the mission of patient care.

***

### References
- Moynihan, K. (2026). *Robots, already in hospitals, are ready to roll in other industries.* CBS News.
- Bansal, V. (2026). *Meet the Robot That Nurses Unplugged.* Proof News.
- Nodell, B. (2026). *Saying goodbye to Moxi, the AI-powered robot.* WSNA Newsletter.
- MHI. (2020). *How Overhead Lifting Solutions Improve Safety.* MHI Solutions.
- Samsung Electronics. *Overhead Hoist Transport Devices Drive Materials in Manufacturing Facility.* Samsung News.
- Lakeridge Health. *Material Handler Job Description.*
- Government of Canada Job Bank. *Medical Material Handler in Ontario.*

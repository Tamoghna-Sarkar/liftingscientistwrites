# Fedstellar: A Platform for Decentralized Federated Learning

**Authors**: Enrique Tomás Martínez Beltrán, Ángel Luis Perales Gómez, Chao Feng, Pedro Miguel Sánchez Sánchez, Sergio López Bernal, Gérôme Bovet, Manuel Gil Pérez, Gregorio Martínez Pérez, Alberto Huertas Celdrán  
**Affiliations**: University of Murcia, University of Zurich, armasuisse  
**arXiv**: [2306.09750v4](https://arxiv.org/abs/2306.09750)  


## Contributions

- **Design and Implementation**: Developed Fedstellar, a comprehensive platform for training federated learning models in decentralized, semi-decentralized, and centralized settings across diverse federations of physical or virtualized devices. The platform extends the p2pfl library and introduces enhanced features for collaborative model training.

- **Flexible Federation Management**: Enables the creation and management of federations with diverse devices, network topologies, and algorithms. Provides sophisticated tools for federation management and performance monitoring through extensible modules that offer data storage, asynchronous capabilities, and efficient mechanisms for model training and communication.

- **Deployment Scenarios**: Demonstrated the platform's effectiveness through two distinct scenarios:
  - A physical deployment involving Raspberry Pi 4 and Rock64 boards for detecting cyberattacks.
  - A virtualized deployment using MNIST and CIFAR-10 datasets to perform image classification tasks in a decentralized manner.

- **Performance Evaluation**: Assessed the platform's performance using various Key Performance Indicators (KPIs), including model F1 score, training time, communication latency, and resource usage. The evaluations showed consistent performance and adaptability, achieving F1 scores of 91%, 98%, and 91.2% for cyberattack detection, MNIST, and CIFAR-10 classification tasks, respectively, and reducing training time by 32% compared to centralized approaches.

## Motivation

Traditional Centralized Federated Learning (CFL) approaches rely on a central server to aggregate participants' models, leading to potential issues such as communication bottlenecks, single points of failure, and reliance on a central entity. Decentralized Federated Learning (DFL) addresses these challenges by enabling decentralized model aggregation and minimizing dependency on a central server. However, existing DFL platforms struggle with managing heterogeneous federation network topologies, adapting to virtualized or physical deployments, and providing comprehensive metrics for evaluating different federation scenarios.

## Architecture

Fedstellar comprises three main components:

- **Frontend**: A web application with an interactive graphical interface for creating, managing, and monitoring federations. It allows users to customize parameters such as the number and type of devices, network topology, machine and deep learning algorithms, and datasets for each participant.

- **Controller**: Responsible for deploying federations of nodes using physical or virtual devices. It orchestrates the operations of the federation, including the assignment of roles and the configuration of network topologies.

- **Core**: Deployed on each device in the federation, the core component provides the logic needed to train, aggregate, and communicate within the network. It manages model training, data preprocessing, secure communication among devices, and storage of the federated models. Additionally, the core supervises the calculation of KPIs and conveys this information back to the frontend for performance monitoring.

## Supported Features

- **Federation Architectures**: Supports Decentralized Federated Learning (DFL), Semi-Decentralized Federated Learning (SDFL), and Centralized Federated Learning (CFL).

- **Network Topologies**: Allows the generation and deployment of complex network topologies, including fully connected, star, ring, and random configurations.

- **Aggregation Algorithms**: Implements various aggregation algorithms such as FedAvg, Trimmed Mean, Median, FedProx, Krum, Zeno, and Fed+.

- **Monitoring and Metrics**: Provides real-time monitoring of model and network performance, including metrics like F1 score, CPU/RAM usage, communication latency, and throughput.

- **Security**: Incorporates secure communication mechanisms using symmetric and asymmetric encryption to ensure data privacy and integrity.

## Experimental Results

### Physical Deployment

- **Devices**: 5 Raspberry Pi 4 and 3 Rock64 boards.

- **Scenario**: Cyberattack detection using syscall data.

- **Model**: Autoencoder with FedAvg aggregation.

- **Topology**: Fully connected.

- **Results**:
  - F1 Score: 91%
  - Training Time: ~60 minutes
  - CPU Usage: up to 86%
  - RAM Usage: ~30%
  - Network Usage: ~1190 MB

### Virtualized Deployment

- **Devices**: 20 Docker containers on a high-resource host.

- **Datasets**: MNIST and CIFAR-10.

- **Models**: LeNet5 (MNIST), MobileNet (CIFAR-10).

- **Topologies**: Fully connected, star, ring.

- **Federation Types**: DFL, SDFL, CFL.

#### MNIST Results

- **DFL (Fully Connected)**:
  - F1 Score: 98.7%
  - CPU Usage: 78%
  - Network Usage: 1243 MB
  - Time to F1 ≥90%: 28 minutes

- **CFL (Star)**:
  - F1 Score: 99.2%
  - CPU Usage: 58%
  - Network Usage: 985 MB
  - Time to F1 ≥90%: 40 minutes

#### CIFAR-10 Results

- **DFL (Fully Connected)**:
  - F1 Score: 91.2%
  - CPU Usage: 80%
  - Network Usage: 1280 MB
  - Time to F1 ≥90%: 33 minutes

### Scalability

- **Participants**: Scaled from 10 to 100 participants on MNIST with a fully connected topology.

- **Results**:
  - F1 Score improved from 97.5% to 99.3%.
  - Time to F1 ≥90% reduced from 35 minutes to 19 minutes.
  - Network Usage increased from 950 MB to 3200 MB.

## Comparison with Other FL Platforms

| Platform       | Architecture | F1 Score | CPU Usage (%) | Network Usage (MB) | Time to F1 ≥90% |
|----------------|--------------|----------|---------------|--------------------|-----------------|
| TFF            | CFL          | 98.2%    | 62            | 1100               | 43 minutes      |
| FedML          | CFL          | 98.9%    | 59            | 1050               | 41 minutes      |
| Scatterbrained | DFL          | 91.8%    | 89            | 2631               | 45 minutes      |
| Fedstellar     | CFL          | 99.2%    | 58            | 985                | 40 minutes      |
| Fedstellar     | DFL          | 98.7%    | 78            | 1243               | 28 minutes      |

## Future Work

- **Dynamic Topologies**: Incorporate support for dynamic topologies to adapt to changing network conditions.

- **Multi-Aggregator Scenarios**: Explore scenarios with multiple aggregators in SDFL to enhance scalability and fault tolerance.

- **Adaptive Timeouts and Aggregator Selection**: Implement adaptive mechanisms for timeouts and aggregator selection to improve efficiency.

- **Real-Time Fault Isolation and Anomaly Detection**: Develop capabilities for real-time fault isolation and anomaly detection within federations.

- **Enhanced Mobile or UAV-Based Deployments**: Extend support for deployments on mobile devices or unmanned aerial vehicles (UAVs) to broaden application domains.

## References

Martínez Beltrán, Enrique Tomás, et al. *Fedstellar: A Platform for Decentralized Federated Learning*. [[Link to Paper](https://arxiv.org/pdf/2306.09750)]


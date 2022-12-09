# AuthorshipAttributionLine
Main DigiLab NLCR initiative

This project is main part of ongoing initiative to establish productive ML environement and test possible approaches upon data stored in National Digital Library.

## Organisational and technical workflow model (BPMN)

![AuthorLineWF rv](https://user-images.githubusercontent.com/25201690/206743941-be281d0e-3dcf-45c9-bf19-ebaf46884151.png)


## Running model(s)

| Version | Specification of literature | Authors | Passage Segmentation | Tokens Total  | Note |
| ---- | -------- | ------ | ----- | ------ | ------ |
| 1. version | Czech Romanticism  | 6  | 50,100,200,500,1000 tokens  | 2 214 878 | hyperparemeters set upon 300 experiments |
| 2. version |  Czech Romanticism | 23 | 50,100,200,500,1000 tokens  | 16 677 327 | stable |



## Environment

| Part | Solution | Detail |
| ---- | -------- | ------ |
| Automatised workflow | CRON + Authorguesser | https://github.com/DigilabNLCR/AuthorGuesser |
| Main ML Library | Scikit-learn | https://scikit-learn.org/ |
| Python version | 3.9 | https://www.python.org/downloads/release/python-390/ |
| Data Integration | National digital library of the Czech Republic | https://ndk.cz/ |
| Preprocessing Integration | UDPipe | https://lindat.mff.cuni.cz/services/udpipe/ |
| Monitoring | Htop, Netdata (+Zabbix)| https://github.com/netdata/netdata |
| Operation System | Centos 7.9 | EOL 1/2 2024, https://www.centos.org/download/ |
| GPU | NVidia Tesla V100S - 32GB | DP 8,2 TFLOPS / SP 16,4 TFLOPS |
| Server |1× SUPERMICRO 1U GPU server | LGA3647, INTEL Xeon Silver 4216 (16-core) 2.1GHZ, 64 GB RAM, 3840 GB SSD |

## **Roles**

### Conception and Research
* **Zdenko Vozár** -  *project lead, solution conception*
* **Jan Holomek** -  *project lead*
* **Martin Holub** -  *statistical and mathematical conception, theory of machine learning*
* **Jan Hajič** - *experiment design, theory of machine learning*
* **František Válek** - *delexicalisation research*
* **Michal Charypar** -  *dataset proposal, literary history background*

### Developement and Operative
* **František Válek** - *principal programmer. operative*
* **Jan Hajič** - *senior programmer, code review, experiment design*
* **Jiří Szromek** -  *supporting programmer, operative*
* **Petra Habetinova** -  *supporting programmer*
* **Inna Skoupá** -  *tester*
* **Miloš Bilwachs** -  *operative support*


## **More**
More information aboutDigiLab: https://digilab.nkp.cz/

# Dedication
National Library of the Czech Republic

_Realized with the support of institutional research of National Library of the Czech Republic funded by Ministry of Culture of the Czech Republic as part of the framework of Longterm conception developement of scientific organization._

Národní knihovna České Republiky

_Realizováno v rámci institucionálního výzkumu Národní knihovny České republiky financovaného Ministerstvem kultury ČR v rámci Dlouhodobého koncepčního rozvoje výzkumné organizace._

# Modelagem e simulação de drone

## Descrição do estudo

O objetivo desse estudo consiste em analisar a modelagem e simulação de um modelo de sistema dinâmico de um drone. 

Após uma pesquisa extensa sobre o tema e plataformas de desenvolvimento, optou-se por escolher o modelo proposto por Khanh Dang denominado [Simulate Quadrotor in Simulink with SimMechanics](http://www.mathworks.com/matlabcentral/fileexchange/48052-simulate-quadrotor-in-simulink-with-simmechanics). O projeto disponibilizado para estudo foi desenvolvido utilizando o software Matlab e tem como inspiração o drone [DJI™ F450](https://www.dji.com/flame-wheel-arf/feature).

## Análise do modelo

O modelo de sistema dinâmico estudado considera os seis graus de liberdade em *pitch*, *roll* e *yaw*, utiliza um controle do tipo PID (Proporcional-Integral-Derivativo) e considera como interface humano máquina o seguinte controle:

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/ControlMode.png">
</p>

### Diagrama de blocos

O modelo completo de simulação do drone pode ser descrito no diagrama de blocos abaixo. A implementação desse modelo foi realizada por meio da ferramenta Simulink do Matlab. Nessa modelagem considera-se 3 grandes blocos: *Signal Builder*, *Controller* e *Quadrotor 3D Model*.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/AssemblyQuadrotor.PNG">
</p>

#### Signal Builder

O módulo *Signal Builder* é responsável por configurar os sinais de entrada da malha de controle referentes à altura de voo do drone e os ângulos de *pitch*, *roll* e *yaw*.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Inputs.PNG">
</p>

A partir da definição dos sinais utilizados como referência para voo do drone é possível definir o grau de precisão do tipo de controle implementado comparando com o resultado da simulação.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/InputSignals.PNG">
</p>

#### Controller

O módulo *Controller* tem como entradas a velocidade máxima do drone e o erro representado pela diferença entre os sinais de referência definidos no módulo *Signal Builder* e os sinais obtidos pela simulação, enquanto que as saídas são as velocidades de cada um dos 4 motores do drone.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Controller.PNG">
</p>

Para ajustar esse controle foram utilizados 4 blocos PID, um para cada sinal de erro, conectados a blocos de ganho e a limitadores que saturam o valor de saída quando a entrada atinge valores acima ou abaixo do definido. As constantes utilizadas nesses blocos foram obtidas empiricamente.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/PIDControl.PNG">
</p>

#### Quadrotor 3D Model

Como dito anteriormente, as saídas do bloco *Controller* são as velocidades dos 4 motores do drone. A partir dessas velocidades são definidos 6 variáveis: *X*, *Y*, *Gaz*, *Roll*, *Pitch* e *Yaw*. Todas essas variáveis são responsáveis por determinar o voo do drone durante a simulação, além de servir como parâmetro para cálculo de erro em relação ao sinal de referência, caracterizando assim um sistema de controle de malha fechada.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Quadrotor3DModel.PNG">
</p>

## Resultados obtidos na simulação

Ao executar a simulação do modelo estudado é possível visualizar em 3D o drone voando tendo como referência de voo os sinais atribuídos no bloco *Signal Builder*. Para facilitar o entendimento do comportamento do drone foi gravado o resultado de uma das simulações e disponibilizado no arquivo [drone-simulation.mp4](https://github.com/matheusrmorgado/Drone-Simulation/blob/master/drone-simulation.mp4).

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/QuadrotorMechanicsView.PNG">
</p>

### Simulation Data Inspector

Após analisar visualmente o comportamento de voo do drone, foi realizada uma análise mais aprofundada do grau de determinismo do drone. Para isso, foi utilizada a ferramenta do Simulink denominada Simulation Data Inspector.

A partir dessa ferramenta, foi possível observar os diferentes sinais do motor do drone gerados pela simulação e então comparar com os sinais de referência. A partir dos logs analisados desses sinais foram gerados gráficos para realizar a comparação e o cálculo de erro entre sinais.

#### Posição X e Y

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/XY.png">
</p>

#### Gaz

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Gaz.png">
</p>

#### Pitch

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Pitch.png">
</p>

#### Roll

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Roll.png">
</p>

#### Yaw

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/Yaw.png">
</p>

### Influência do modelo de controle

Para estudar com mais detalhes a influência da escolha de um sistema de controle do tipo PID no voo de um drone, foi realizada uma comparação com uma simulação utilizando o controle do tipo P. O resultado obtido foi um erro maior e um sinal mais distorcido. Dessa forma, observa-se que o controle do tipo PID é uma boa solução na implementação da modelagem de um drone.

<p align="center">
  <img src="https://github.com/matheusrmorgado/Drone-Simulation/blob/master/images/RollControllers.png">
</p>

## Referências

* Simulate Quadrotor in Simulink with SimMechanics: http://www.mathworks.com/matlabcentral/fileexchange/48052-simulate-quadrotor-in-simulink-with-simmechanics

* Quadcopter Simulation and Control Made Easy: https://www.mathworks.com/videos/quadcopter-simulation-and-control-made-easy-93365.html

* DJI™ F450 User Manual: https://www.dji.com/flame-wheel-arf/download

## Direitos autorais

O estudo aqui publicado teve como único objetivo analisar a modelagem de drone desenvolvida por Khanh Dang disponibilizada na plataforma File Exchange da MathWorks. A licença original do projeto encontra-se reproduzida integralmente no arquivo [LICENSE.md](https://github.com/matheusrmorgado/Drone-Simulation/blob/master/LICENSE.md).

“This work has not been authorized, sponsored, or otherwise approved by DJI.”
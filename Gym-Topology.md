# Rooms
* Receção:
  * Sensores:
    * sensor_luminusidade1
    * sensor_tensao_selfS
    * sensor_corrente_selfS

  * Actuadores:
    * Lamp1
    * Vending_Current

  * Regras: 
    * sensor_luminusidade1 <= 1200  --> Lamp1:ON
    * sensor_tensao_selfS > 400 OR sensor_corrente_selfS > 3 --> Vending_Current:OFF (assumindo uma vending machine com potencia maxima de 1200W)


* WC_Masculino: 
  * Sensores:
    * sensor_corrente_WC_M
    * sensor_luminusidade_WC_M

  * Actuadores:
    * Lamp_WC_M

  * Regras: 
    * sensor_corrente_WC_M==0 AND sensor_luminusidade_WC_M <= 800 AND Flag_WC_M==0 --> Lamp_WC_M:ON, Flag_WC_M==1
    * sensor_corrente_WC_M==0 AND Flag_WC_M==1 OR sensor_luminusidade_WC_M >= 1200  --> Lamp_WC_M:OFF, Flag_WC_M==0

* WC_Feminino: 
  * Sensores:
    * sensor_corrente_WC_F
    * sensor_luminusidade_WC_F

  * Actuadores:
    * Lamp_WC_F

  * regras: 
    * sensor_corrente_WC_F==0 AND sensor_luminusidade_WC_F <= 800 AND Flag_WC_F==0 --> Lamp_WC_F:ON, Flag_WC_F==1
    * sensor_corrente_WC_F==0 AND Flag_WC_F==1 OR sensor_luminusidade_WC_F >= 1200   --> Lamp_WC_F:OFF, Flag_WC_F==0

* Musculação
  * Sensores:
    * mote_x: sensor_tensao_M1 , sensor_corrente_M1
    * mote_y: sensor_tensao_M2 , sensor_corrente_M2

  * Atuadores:
    * mote_X: Musc_Machine1
    * mote_Y: Musc_Machine2

  * Regras:
    * sensor_tensao_M1 > 150 OR sensor_corrente_M1 > 4 --> Musc_Machine1:OFF (assumindo uma maquina com potencia maxima de 520W)
    * sensor_tensao_M2 > 60 OR sensor_corrente_M2 > 2 --> Musc_Machine2:OFF (assumindo uma maquina com potencia maxima de 120W)
    * sensor_corrente2==0 And --> lotação pessoas++,Lamp_ instrucao:ON, Lamp2:ON
    * lotação > 20 --> Alarm1:ON (um quadradinho que passa de verde para vermelho)

* Balneario_Masc
  * Sensores:
    * sensor_corrente_B_M
    * sensor_luminusidade_B_M
    * sensor_humidade_B_M

  * Actuadores:
    * Lamp_B_M
    * desumidificador_B_M

  * Regras:
    * sensor_corrente_B_M==0 AND sensor_luminusidade_B_M <= 800  --> Lamp_B_M:ON
    * sensor_humidade_B_M > 8 --> desumidificador_B_M:ON
    * sensor_humidade_B_M <3 --> desumidificador_B_M:OFF

* Balneario_Fem
  * Sensores:
    * sensor_corrente_B_F
    * sensor_luminusidade_B_F
    * sensor_humidade_B_F

  * Actuadores:
    * Lamp_B_F
    * desumidificador_B_F

  * Regras:
    * sensor_corrente_B_F==0 AND sensor_luminusidade_B_F <= 800  --> Lamp_B_F:ON 
    * sensor_humidade_B_F > 8 --> desumidificador_B_F:ON
    * sensor_humidade_B_F <3 --> desumidificador_B_F:OFF

* Piscina
  * Sensores:
    * sensor_humidade_piscina
    * sensor_Luminusidade_piscina
    * sensor_corrente_Piscina
    * sensor_temperatura_piscina

  * Atuadores:
    * desumidificador_piscina
    * humidificador_piscina
    * Lamp_Piscina
    * heater_piscina
    * dispensador_toalhas1

  * Regras:
    * sensor_humidade_piscina > 30 --> desumidificador_piscina:ON
    * sensor_humidade_piscina =<10 --> desumidificador_piscina:OFF
    * sensor_humidade_piscina<3 --> humidificador_piscina:ON
    * sensor_humidade_piscina>10 --> humidificador_piscina:OFF
    * sensor_Luminusidade_piscina < 1000 --> Lamp_Piscina:ON
    * sensor_temperatura_piscina < 13 --> heater_piscina:ON
    * sensor_temperatura_piscina > 30 --> heater_piscina:OFF
    * sensor_corrente_Piscina==0 --> Rising edge dispensador_toalhas1:ON

* Sauna
  * Sensores:
    * sensor_temperatura_sauna
    * sensor_humidade_sauna
    * sensor_corrente_Sauna

  * Actuadores:
    * humidificador_sauna
    * heater_sauna
    * dispensador_toalhas2

  * Regras:
    * sensor_humidade_sauna<3 --> humidificador_sauna:ON
    * sensor_humidade_sauna>10 --> humidificador_sauna:OFF
    * sensor_temperatura_sauna < 25 --> heater_sauna:ON
    * sensor_temperatura_sauna > 35 --> heater_sauna:OFF
    * sensor_corrente_sauna==0 --> Rising edge dispensador_toalhas2:ON

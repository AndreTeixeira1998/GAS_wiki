# Rooms
* **Receção:**
  * **Sensores:**
    * sensor_luminosidade1
    * sensor_tensao_selfS
    * sensor_corrente_selfS

  * **Actuadores:**
    * Lamp1
    * Lamp2
    * Vending_Current

  * **Regras:**
    * sensor_luminosidade1 <= 1200  --> Lamp1:ON AND Lamp2:ON)
    * (sensor_tensao_selfS > 400 OR sensor_corrente_selfS > 30) --> Vending_Current:OFF
      (assumindo uma vending machine com potência máxima de 12000W)


* **WC_Masculino:** 
  * **Sensores:**
    * sensor_corrente_WC_M
    * sensor_luminosidade_WC_M

  * **Actuadores:**
    * Lamp_WC_M

  * **Regras:** 
    * sensor_corrente_WC_M==0 AND sensor_luminosidade_WC_M <= 800 AND Flag_WC_M==0 --> Lamp_WC_M:ON, Flag_WC_M==1
    * sensor_corrente_WC_M==0 AND Flag_WC_M==1 OR sensor_luminosidade_WC_M >= 1200  --> Lamp_WC_M:OFF, Flag_WC_M==0

* **WC_Feminino:** 
  * **Sensores:**
    * sensor_corrente_WC_F
    * sensor_luminosidade_WC_F

  * **Actuadores:**
    * Lamp_WC_F

  * **regras:** 
    * sensor_corrente_WC_F==0 AND sensor_luminosidade_WC_F <= 800 AND Flag_WC_F==0 --> Lamp_WC_F:ON, Flag_WC_F==1
    * sensor_corrente_WC_F==0 AND Flag_WC_F==1 OR sensor_luminosidade_WC_F >= 1200   --> Lamp_WC_F:OFF, Flag_WC_F==0

* **Musculação**
  * **Sensores:**
    * sensor_tensao_M1 
    * sensor_corrente_M1
   

  * **Atuadores:**
    * Musc_Machine1
    * Lamp2
    

  * **Regras:**
    * sensor_tensao_M2 > 60 OR sensor_corrente_M2 > 2 --> Musc_Machine1:OFF 
    * sensor_corrente_M1==0 And --> lotação pessoas++,Lamp_ instrucao:ON, Lamp2:ON
  

* **Balneario_Masc**
  * **Sensores:**
    * sensor_corrente_B_M
    * sensor_luminosidade_B_M
    * sensor_humidade_B_M

  * **Actuadores:**
    * Lamp_B_M
    * desumidificador_B_M

  * **Regras:**
    * sensor_corrente_B_M==0 AND sensor_luminosidade_B_M <= 800  --> Lamp_B_M:ON
    * sensor_humidade_B_M > 800 --> desumidificador_B_M:ON
    * sensor_humidade_B_M <300 --> desumidificador_B_M:OFF

* **Balneario_Fem**
  * **Sensores:**
    * sensor_corrente_B_F
    * sensor_luminosidade_B_F
    * sensor_humidade_B_F

  * **Actuadores:**
    * Lamp_B_F
    * desumidificador_B_F

  * **Regras:**
    * sensor_corrente_B_F==0 AND sensor_luminosidade_B_F <= 800  --> Lamp_B_F:ON 
    * sensor_humidade_B_F > 800 --> desumidificador_B_F:ON
    * sensor_humidade_B_F <300 --> desumidificador_B_F:OFF

* **Piscina**
  * **Sensores:**
    * sensor_humidade_piscina
    * sensor_Luminosidade_piscina
    * sensor_corrente_Piscina
    * sensor_temperatura_piscina

  * **Atuadores:**
    * desumidificador_piscina
    * Lamp_Piscina
    * heater_piscina
    * dispensador_toalhas1

  * **Regras:**
    * sensor_humidade_piscina > 300 --> desumidificador_piscina:ON
    * sensor_temperatura_piscina > 30 --> heater_piscina:OFF
    * sensor_corrente_Piscina==0 --> Rising edge dispensador_toalhas1:ON
    * sensor_Luminosidade_piscina>800 --> Lamp_Piscina:OFF

* **Sauna**
  * **Sensores:**
    * sensor_temperatura_sauna
    * sensor_humidade_sauna
    * sensor_corrente_Sauna

  * **Actuadores:**
    * humidificador_sauna
    * heater_sauna
    * dispensador_toalhas2

  * **Regras:**
    * sensor_humidade_sauna<300 --> humidificador_sauna:ON
    * sensor_temperatura_sauna > 30 --> heater_sauna:OFF
    * sensor_corrente_sauna==0 --> Rising edge dispensador_toalhas2:ON

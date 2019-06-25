# configuracoes-device-ttn

Guia de configuração de *End Device* LoRa na [TTN](https://www.thethingsnetwork.org/) utilizando o dispositivo [The TheThings UNO](https://www.thethingsnetwork.org/marketplace/product/the-things-uno)

A aplicação desenvolvida consiste nas medições de temperatura, luminosidade e distância. 

Pré requisitos:
- Gateway Lora :satellite:
- Acesso a TTN

### Materiais utilizados

* [The TheThings UNO](https://www.thethingsnetwork.org/marketplace/product/the-things-uno)
* Sensor ultrassônico - [HC-SR04](https://www.filipeflop.com/blog/sensor-ultrassonico-hc-sr04-ao-arduino/)
* Display LCD 16×2
* Potenciômetro 10kΩ
* Sensor NTC - temperatura
* Sensor LDR - luminosidade
* 2 resistores 10kΩ
* Protoboard

## Prototipagem

O esquema abaixo mostra como as conexões para a execução do código [aqui](https://github.com/mftutui/configuracoes-device-ttn/blob/master/aplication.ino) foram feitas.
* Lembrando que o esquemático mostra um **ARDUINO LEONARDO** mas está sendo utilizado um módulo **The Things UNO**

![esquemático](https://github.com/mftutui/configuracoes-device-ttn/blob/master/device_v1.png)

A montagem real do circuito ficou da seguinte maneira:

![montagem1](https://github.com/mftutui/configuracoes-device-ttn/blob/master/montagem1.JPG)
![montagem2](https://github.com/mftutui/configuracoes-device-ttn/blob/master/montagem2.JPG)

Afim de minimizar o espaço ocupado pelo *device* foram usadas uma *protoshield* para Arduino e uma mini *protoboard*.

## Código

Aqui estão algumas coisas que você precisa saber sobre o código compartilhado.

Ele foi feito e carregado para o TTUNO usando a IDE do [Arduino](https://www.arduino.cc/), a TTN (TheThingsNetwork) disponibiliza uma biblioteca para isso.
Mas se você está fazendo a sua própria aplicação usando o TTUNO posso supor que já tenha feito pelo menos um teste com ele.
Se não, aqui estão os links para os vídeos que a própria TTN disponibiliza no Youtube, eu recomento que assista esses:
- [Uso do console](https://www.youtube.com/watch?v=JrNjY-pGuno).
- Ativação e uso do TTUNO [1](https://www.youtube.com/watch?v=kqI78zkhaFQ), [2](https://www.youtube.com/watch?v=28Fh5OF8ev0), [3](https://www.youtube.com/watch?v=-VaW9bBVrYM) e [4](https://www.youtube.com/watch?v=VXNfNDcFU2c).

As bibliotecas a serem incluídas na IDE são:
```c
// Biblioteca The Things Network
#include <TheThingsNetwork.h>

//Biblioteca para o sensor ultrassonico
#include <Ultrasonic.h>

//Biblioteca para usar o display LCD
#include <LiquidCrystal.h>

```
* A biblioteca **Ultrasonic** utilizada foi [essa](https://github.com/filipeflop/Ultrasonic)

Tendo em vista que a sua aplicação já está criada na TTN e que você possui os números de *appEui* e *appKey* não esqueça adiciona-los ao código!

Caso ela não esteja dê uma olhada [nesse](https://github.com/mftutui/ttn-first-steps) link. 

```c
// Set your AppEUI and AppKey
const char *appEui = "****************";
const char *appKey = "********************************";

```

Pronto agora é só carregar o código e partir para o console da TTN.

## Visualizando o payload

Seu código foi carregado para o *device* e a gora é a hora de testar a aplicação na TTN. Você pode observar as informações na aplicação clicando na barra ***Data***.

![dados](https://github.com/mftutui/configuracoes-device-ttn/blob/master/dados.png)

O conjunto dos dados capturados pelo dispositivo é chamado de *payload*, ele aparece na figura acima, mas não está legível. Para compreender o que está no *payload* precisamos decodificar esses dados.

No console da TTN, vá até a aplicação criada e clique na aba ***Payload Formats***, em *decocer* o *payload* pode ser tratado com **JSON**. Nesse caso os bits estão sendo separados um a um e retornam com os valores "traduzidos".

```
function Decoder(bytes, port) {

  var dist1 = bytes[0];
  var dist2 = bytes[1];
  var temp = bytes[2];
  var lum = bytes[3];

  return {
    Distancia: dist1+","+dist2+"cm", Temperatura: temp+"°C", Luminosidade: lum+""
  };
}
```

![payload1](https://github.com/mftutui/configuracoes-device-ttn/blob/master/payload1.png)

Um teste conferindo a separação dos dados pode ser feio ao incluir informações no box **Payload**. Não esqueça de salvar as alterações em  *save payload functions*.

![payload2](https://github.com/mftutui/configuracoes-device-ttn/blob/master/payload2.png)

Pronto, agora as informações aparecerão decodificadas na aba **Data**

![decode_data](https://github.com/mftutui/configuracoes-device-ttn/blob/master/decode_data.png)

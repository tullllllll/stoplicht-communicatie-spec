# stoplicht-communicatie-spec
Dit document beschrijft de communicatie tussen de kruispuntsimulatie en de controller van het stoplichtprobleem.

## Berichten
De simulate kan verschillende berichten sturen naar de controller.
De controller reageert hierop en stuurt de nieuw staat in een bericht naar de simulatie.
Elk bericht heeft het volgende format:
```json
{
  "messageType": "string",
  "currentTimeStamp": 1,
  "payload": {}
}
```
Waar de structuur van het payload object afhankelijk is van het type bericht `messageType`. Elke message die gestuurd wordt, geeft een `messageType` aan die beschreven is in deze spec.
Wanneer de simulatie een bericht maakt om te sturen, zet deze de huidige simulatie tijd in het bericht, bij `currentTimeStamp`. Als de controller reageert op dit bericht, wordt te `currentTimeStamp` bewaart.

### messageTypes

| messageType    | verplicht implementeren | Reden van bestaan                                             | Specificatie | voorbeeld                           | Wanneer gestuurd                                                                                                                                                                  |
| -------------- | ----------------------- | ------------------------------------------------------------- | ------------ |-------------------------------------| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| “SensorUpdate“ | ✅                       | controller op de hoogte stellen van veranderingen in situatie | tbd          | [hier](./format-verkeerslicht.json) | Periodiek (elke 100 simulatie milliseconden) simulatie → controller, nadat controller klaar is met stoplicht-staat bepalen stuurt deze de nieuwe staat in hetzelfde format terug. |



## Volgorde van acties
- De simulatie stuurt de staat van alle stoplichten en sensoren in JSON naar de controller, via een websocket. `messageType: "SensorUpdate"` [format](./format-verkeerslicht.json)
- De controller bepaald wat de verkeerslichten moeten doen. (implementatie per groep)
- De controller stuurt de gewenste stoplicht staat naar de simulator. `"messageType": "SensorUpdate"` (of dit hetzelfde bericht type moet zijn is nog te debatteren) [format](./format-verkeerslicht.json)

*de regressiemeter is geen middle man en luistert mee.*


## Problemen met deze oplossing?
- Als je een verandering wilt zien in wat de afspraken zijn, of daar een vraag over hebt, maak een [issue](https://github.com/jorrit200/stoplicht-communicatie-spec/issues) aan.
- Als je denkt dat deze spec afwijkt van een gemaakte afspraak, of dat een gemaakte afspraak nog niet geïmplementeerd is, maak een [pull-request](https://github.com/jorrit200/stoplicht-communicatie-spec/pulls) aan.
- communiceer het naar de anderen in discord of de lessen.
- veranderingen worden democratisch behandeld. Finale besluitvorming gebeurt altijd in een vergadering

Over tijd heen zal deze spec specifieker ingaan op het JSON-format.

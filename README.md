# rimfrost-service-kund-prenumeration-asyncapi

AsyncAPI-spec för `rimfrost-service-kund-prenumeration` — en generell service för prenumeration på förändringar i kundcentrerat data.

## Syfte

När en användare kompletterar en uppgift på Mina sidor uppdateras kundcentrerat data av en extern källa. Regler (t.ex. kompletteringsregler) behöver kunna reagera på dessa förändringar för att kunna påverka regelutfallet.

Denna service tillhandahåller en Kafka-baserad prenumerationsmekanism: en regel registrerar intresse för en specifik kund (via idtyp + idvärde) och får en notifikation när kundens data har uppdaterats.

## Flöde

```
Regel                          kund-prenumeration              Extern källa
  |                                    |                             |
  |-- subscribe (prenumerationId,  --> |                             |
  |   kund, replyTo) ----------->      |                             |
  |                                    |                             |
  |                       [kundcentrerat data uppdateras] <---------|
  |                                    |
  |<-- notification (prenumerationId,  |
  |    kund) <------------------------ |
  |                                    |
  |-- unsubscribe (prenumerationId) -> |
```

## Kanaler

| Kafka-topic | Riktning | Beskrivning |
|---|---|---|
| `kund.subscribe` | → service | Registrera prenumeration |
| `kund.unsubscribe` | → service | Avregistrera prenumeration |
| `replyTo` (angiven av prenumerant) | ← service | Event om förändrat kundcentrerat data |

## Spec

Se [`asyncapi.yaml`](asyncapi.yaml) för fullständig meddelandedefinition.


# API for integrating services with TREGO.

## Flight search

#### Request:
HTTP GET to http://api.trego.ru/api/v1/avia/

Sample:
```
http://api.trego.ru/api/v1/avia/?from=LED&to=PAR&date1=2012-10-24&date2=2012-11-13&adults=1&children=0&infants=0&partner=test&password=xxx
```

Params:

 * `from` — IATA code of origin (mandatory)
 * `to` — IATA code of destination (mandatory)
 * `date1` — date of departure in ISO8601 format (mandatory)
 * `date2` — date of return flight in ISO8601 format (optional)
 * `adults` — number of adult passengers (optional, default is 1)
 * `children` — number of children (optional, default is 0)
 * `infants` — number of infants without seat (optional, default is 0)
 * `cabin` — preferred cabin class (choice of "Y" for econom, "C" for business, "F" for first; optional). If param is omitted or its value not in choices then  value defaults to Y
 * `partner` — partner ID (mandatory)
 * `password` — partner password (mandatory)

Response formatting parameters:

 * `xml` — "1" for XML output (default) and "0" for JSON
 * `pretty` — "1" for pretty JSON output

Apparently, if `date2` is omitted we look for one-way flight. If `date2` is set and correct we return results of search for round-trip flights.

#### Response 
format: XML

```
<variants requestTime="5.8584">
  <variant>
    <price>7825</price>
    <currency>rub</currency>
    <url>FLIGHT_DEEPLINK</url>
    <validatingCarrier>FV</validatingCarrier>
    <segment>
      <flight>
        <operatingCarrier>FV</operatingCarrier>
        <marketingCarrier>FV</marketingCarrier>
        <number>451</number>
        <departure>LED</departure>
        <departureDate>2012-10-24</departureDate>
        <departureTime>23:20</departureTime>
        <arrival>TJM</arrival>
        <arrivalDate>2012-10-25</arrivalDate>
        <arrivalTime>04:35</arrivalTime>
        <equipment>735</equipment>
        <cabin>Y</cabin>
        <class>U</class>
        <flightTime>195</flightTime>
      </flight>
    </segment>
  </variant>
  …
</variants>
```

Response content:

 *  /variants - root element containing ticket variants.
 *  //variant - full flight variant
 *  variant/price - total price for all the passengers.
 *  variant/currency - code of currency used in price. For example RUB.
 *  variant/url - deeplink to variant page for booking.
 *  variant/validatingCarrier - IATA code of ticket issuing airline
 *  variant/segment - 1 elements for one way, 2 for roundtrip
 *  variant/segment/flight - 1..n, more than one if there're connecting points in the requested leg
 *  //flight - one hop flight information
 *  flight/operatingCarrier - airline IATA code (2 latin letters)
 *  flight/marketingCarrier - selling airline IATA code (2 latin letters)
 *  flight/number - 2..4 digits, flight number
 *  flight/departure - departure airport IATA code (3 latin letters)
 *  flight/departureDate - departure date, 'YYYY-MM-DD', local
 *  flight/departureTime - departure time, 'HH:MM', local
 *  flight/arrival - arrival airport IATA code (3 latin letters)
 *  flight/arrivalDate - arrival date, 'YYYY-MM-DD', local
 *  flight/arrivalTime - arrival time, 'HH:MM', local
 *  flight/equipment - IATA (or russian domestic) aircraft/bus/train equipment type code.
 *  flight/cabin - cabin class, could be 'F' for 'first', 'C' for 'business', 'Y' for 'economy/couch'
 *  flight/class - booking class
 *  flight/flightTime - pure flight time

One can append arbitrary GET-parameter `marker` to DEEPLINK so it will look as `DEEPLINK&marker=12345.t`, it will be stored and can be seen in statistics response.


## Statistics

#### Request:

HTTP GET to `http://api.trego.ru/api/v1/stat`

Sample:
```
curl 'http://api.trego.ru/api/statistics?date1=2012-05-01&date2=2012-09-01&partner=test&password=xxx'
```

Statistics request parameters:

 *  date1 - YYYY-MM-DD date
 *  date2 - YYYY-MM-DD date
 *  partner - partner identifier
 *  password - password of current partner

Response contains all bookings created between date1 and date2 with requested partner ID.

#### Response example:

```
<bookings>
  <booking>
    <id>JHKHVR</id>
    <created_at>2012-08-12 00:02:01</created_at>
    <marker>1232:1222.t</marker>
    <price>3199</price>
    <profit>200</profit>
    <currency>RUB</currency>
    <state>PAID</state>
  </booking>
  …
</bookings>
```

Response content:

 *  /bookings - root element containgin all bookings
 *  //booking - information on one booking
 *  booking/id - unique identifier of booking
 *  booking/created_at - YYYY-MM-DD HH:MM date and time of booking in UTC time
 *  booking/marker - marker sent to booking deeplink in GET parameter marker
 *  booking/from - origin IATA
 *  booking/to - destination IATA
 *  booking/price - prices of tickets
 *  booking/profit - metasearch agency profit for this booking
 *  booking/currency - currency for fields price and profit
 *  booking/state - state of booking. Possible values PROCESSING/PAID/CANCELLED

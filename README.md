# API for integrating services with TREGO.

*NB: This document is draft of API description and will definitely be updated.*

## Flight search

#### Request:
HTTP GET to http://api.trego.ru/api/v1/avia/

Sample:
http://api.trego.ru/api/v1/avia/?from=LED&to=PAR&date1=2012-10-24&date2=2012-11-13&adults=1&children=0&infants=0&partner=test&password=xxx

Params:

 * `from` — IATA code of origin (mandatory)
 * `to` — IATA code of destination (mandatory)
 * `date1` — date of departure in ISO8601 format (mandatory)
 * `date2` — date of return flight in ISO8601 format (optional)
 * `adults` — number of adult passengers (optional, default is 1)
 * `children` — number of children (optional, default is 0)
 * `infants` — number of infants without seat (optional, default is 0)
 * `cabin` — preferred booking class (choice of "Y" for econom, "C" for business, "F" for first; optional). If param is omitted or its value not in choices then  value defaults to Y
 * `partner` — partner ID (mandatory)
 * `password` — partner password (mandatory)

Response formatting parameters:

 * `xml` — "1" for XML output (default) and "0" for JSON
 * `pretty` — "1" for pretty JSON output

Apparently, if `date2` is omitted we look for one-way flight. If `date2` is set and correct we return results of search for return flights.

#### Response 
format: JSON

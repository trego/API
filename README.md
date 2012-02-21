# API for integrating services with TREGO.

*NB: This document is draft of API description and will definitely be updated.*

## Flight search

#### Request:
HTTP GET to http://api.trego.ru/v1/

Params:

 * `origin` — IATA code of origin (mandatory)
 * `destination` — IATA code of destination (mandatory)
 * `departure` — date of departure in ISO8601 format (mandatory)
 * `return` — date of return flight in ISO8601 format (optional)
 * `adults` — number of adult passengers (optional, default is 1)
 * `children` — number of children (optional, default is 0)
 * `infants` — number of infants without seat (optional, default is 0)
 * `class` — preferred booking class (choice of "Y" for econom, "C" for business, "F" for first; optional). If param is omitted or its value not in choices then  value defaults to Y
 * `partner` — partner ID (mandatory)

Apparently, if `return` is omitted we look for one-way flight. If `return` is set and correct we return results of search for return flights.

#### Response 
format: JSON

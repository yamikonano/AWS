<link href = "style.css" rel = "stylesheet">

# Route 53 #

- HA, HS, Fully managed, authoritative DNS
- domain register
- check health of resources
- 100% avail. SLA 

## <bl>DNS request/reponse ##
- high TTL
  - no need to response everytime before http request
  - cached for long time
  - outdated recored -> clients delay to receive
- low TTL
  - expensive
  - easy change record

## <o>Alias</o> vs <cy>CNAME</cy> ##
- <o>non root and root domain, <cy>root domain only
- <o>free of charge
- <o>natice health check
- <r>cannot <o>set TTL
- <o>auto recognize change
- <r>cannot <o>set to EC2
- <cy>value: must be domain name

## <bl>Route policies </bl> ##
- DNS perspective route
> Simple
- route to a single resources
- specify multi-value in same record,random chosen by client
- *Alias enable -> one resources
- **no health check**
> Weighted
- control % of request -> each specific resources

$$ traffic(\%)={Weight\ for\ a\ specific\ record \over Sum \ of \ all \ the \ weight \ for \ all\ records} $$
- DNS record need same name and type
- health check <bl>√
- 0 weight = no traffic, all 0 = all equally
> Failover

> Lantency Based
> Geolocation
> Multi-Value Answer
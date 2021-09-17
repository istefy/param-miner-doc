# param-miner-doc

Unofficial documentation for the great tool [Param Miner](https://github.com/PortSwigger/param-miner) by [James 'albinowax' Kettle](https://github.com/albinowax).

## Motivation

I've used Param Miner for quite a long time but what many of it's checkboxes do remained a mystery for me. This repo aims to shine some light on purpose and use cases for some non obvious parameters of Param Miner. Information gathered here origins mostly from reading the [source code](https://github.com/PortSwigger/param-miner).

## Attack Config

| Parameter name | Description |
| - | - |
| Add 'fcbz' cachebuster | If checked: Param Miner [adds `fcbz=1` URL parameter](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGrabber.java#L88) to every request in order to avoid cache hits. |
| Add dynamic cachebuster | add URL parameter `fcbz=Utilities.generateCanary();` to every request in order to avoid cache hits. |
| Add header cachebuster | Not use. |
| learn observed words | If checked: Param Miner extracts words from responses and [saves](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/GrabScan.java#L21) them to current session's parameter wordlist. |
| enable auto-mine | If checked Param Miner will execute [launchScan](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGrabber.java#L98) on every response processed at Proxy tab. Think of it like making Param Miner press `Guess *` buttons on every in-scope request for you. Also without it all other `auto-*` checkboxes won't take an effect. |
| auto-mine headers | Add new scan task with Headers |
| auto-mine cookies | Add new scan task with Cookies |
| auto-mine params | If checked add new scan task with Parameters |
| auto-nest params | Not use. First it [finds the most frequently occuring prefix]
| skip boring words | If checked: [skip](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L249) headers from [`boring_headers`](https://github.com/PortSwigger/param-miner/blob/master/resources/boring_headers) wordlist. |
| only report unique params | Report only new parameters (https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L256) |
| response | If checked: get words from HTTP response, [normalize](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L391) them and add to current session's parameter wordlist. |
| request | If checked: get words from HTTP request, [normalize](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L391) them and add to current session's parameter wordlist. |
| use basic wordlist | If checked: use [headers](https://github.com/PortSwigger/param-miner/blob/master/resources/headers) and [params](https://github.com/PortSwigger/param-miner/blob/master/resources/params) wordlists from Param Miner's repo. |
| use bonus wordlist | If checked: use [wordlists from Param Miner's repo](https://github.com/PortSwigger/param-miner/tree/master/resources). Normally used to include [`functions`](https://github.com/PortSwigger/param-miner/blob/master/resources/functions) and [`words`](https://github.com/PortSwigger/param-miner/blob/master/resources/words) wordlists however if `use basic wordlist` isn't checked it will also add [`headers`](https://github.com/PortSwigger/param-miner/blob/master/resources/headers) or [`params`](https://github.com/PortSwigger/param-miner/blob/master/resources/params) according to parameter type. |
| use assetnote params | If checked: use [`params`](https://github.com/PortSwigger/param-miner/blob/master/resources/assetnote-params) from assetnote resource
| use custom wordlist | Self explanatory. |
| bruteforce | ??? but used only at [this line](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGuesser.java#L150) |
| dynamic keyload | ??? This is the hard one - in order to understand it first need to understand how Param Miner works internally. Mostly related to [`ParamGuesser.addNewKeys`](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGuesser.java#L517) function. |
| max one per host+status | ??? |

(https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/BurpExtender.java#L316) and then [uses it here](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/BurpExtender.java#L355). |
| try cache poison | ? |
| try -_ bypass | If checked: for every HTTP header with at least one dash Param Miner will [replace dashes `-` with underscores `_` and add resulting header to wordlist](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamHolder.java#L37). Here is [good explanation](https://telekomsecurity.github.io/2020/05/smuggling-http-headers-through-reverse-proxies.html) of why this works. |
| rotation interval | ??? |
| force bucketsize | ? |
| max param length | Determines maximum length for params parsed from response. [Params with greater length truncated to this limit but not ignored!](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L398) Note: it doesn't affect params supplied by any of wordlists.</br></br>Also when [determining a bucket size](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L147) max param length [is used](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L211) as length of dummy parameters in trial payloads. |
| Add dynamic cachebuster | ??? |



| custom wordlist path | Path to user supplied wordlist of parameters. Note: it'll take an effect only when `use custom wordlist` is checked. |
| skip uncacheable | If checked: skips [cookie and header params](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/TriggerParamGuesser.java#L66) if [no-cache](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/TriggerParamGuesser.java#L83) string found in response. Perhaps it's useful if you're looking for cache poisoning attacks and you want to skip responses that won't be cached anyways. |
| max one per host | Related to rate-limiting. Perhaps don't allows to [run more than 1 attack](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/TriggerParamGuesser.java#L108) against a given host at a time. |
| scan identified params | If checked: [run Burp Scanner](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGuesser.java#L536) on identified parameters. |

| fuzz detect | If checked: [appends ```<a`'"${{\``` to input values to try and detect better-hidden params](https://mobile.twitter.com/albinowax/status/1049295759915606016). |
| try method flip | If checked: for every [non-GET](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L163) request [will use Burp's `toggleRequestMethod`](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L166) which `can be used to toggle a request's method between GET and POST. Parameters are relocated between the URL query string and message body as required, and the Content-Length header is created or removed as applicable`. Finally results in [this branch](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamGuesser.java#L276) getting executed which tries to identify new parameters by making non-GET requests as GET requests. |
| thread pool size | ? |
| rotation increment | ??? |
| max bucketsize | [Maximum number of parameters probed in one request](https://github.com/PortSwigger/param-miner/blob/26db2f47b2e7852b977e776ebe13c1b887474b32/src/burp/ParamAttack.java#L226). Note that for JSON parameters maximum bucketsize is 256. |

## Contribution

If you've found a mistake or just want to add something please fill free to [create an Issue](https://github.com/nikitastupin/param-miner-doc/issues/new) or even a Pull Request!

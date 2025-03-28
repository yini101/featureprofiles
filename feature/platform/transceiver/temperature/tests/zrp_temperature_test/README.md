# TRANSCEIVER-8 (400ZR_PLUS): Telemetry: 400ZR_PLUS Optics module temperature streaming.

## Summary

Validate 400ZR_PLUS optics report module level internally measured temperature
in 1/256 degree Celsius increments as defined in the CMIS.

Link to CMIS:
https://www.oiforum.com/wp-content/uploads/CMIS5p0_Third_Party_Spec.pdf

## Procedure

*   Connect two ZR_PLUS optics using a duplex LC fiber jumper such that TX
    output power of one is the RX input power of the other module.
*   To establish a point to point ZR_PLUS link ensure the following:
      * Both transceivers state is enabled.
      * Both transceivers are set to a valid target TX output power
        example -3 dBm.
      * Both transceivers are tuned to a valid centre frequency
        example 193.1 THz.
*   With the ZR_PLUS link established as explained above, verify that the
    following ZR_PLUS transceiver telemetry paths exist and are streamed for both
    the ZR_PLUS optics.
    *   /platform/components/component/state/temperature/instant
    *   /platform/components/component/state/temperature/min
    *   /platform/components/component/state/temperature/max
    *   /platform/components/component/state/temperature/avg
*   For reported data check for validity min <= avg/instant <= max

*   If the modules or the devices are in a boot stage, they must not stream
    any invalid string values like "nil" or "-inf".
*   Reported temperature value must always be of type decimal64.


**Note:** For min, max, and avg values, 10 second sampling is preferred. If the
          min, max average values or the 10 seconds sampling is not supported,
          the sampling interval used must be specified and this must be
          captured by adding a deviation to the test.


*   Verify the module temperature is reported correctly with optics interface
    in disabled state.

    *   Use /interfaces/interface/config/enabled to disable the interfaces and
        wait 120 seconds(cooling off period) before taking the temperature
        reading again.
    *   Verify the module is able to stream the temperature data in this state.
    *   Verify the module reported temperature in this state is always less
        than the module temperature captured during steady state operation with
        interface state enabled.
    *   For reported data check for validity min <= avg/instant <= max
    
    *   If the modules or the devices are in a boot stage, they must not stream
        any invalid string values like "nil" or "-inf".
    *   Reported temperature value must always be of type decimal64. 

## OpenConfig Path and RPC Coverage

The below yaml defines the OC paths intended to be covered by this test.  OC paths used for test setup are not listed here.

```yaml
paths:
    ## Config Paths ##
    /interfaces/interface/config/enabled:
    ## State Paths ##
    /components/component/state/temperature/instant:
        platform_type: [ "TRANSCEIVER" ]
    /components/component/state/temperature/min:
        platform_type: [ "TRANSCEIVER" ]
    /components/component/state/temperature/max:
        platform_type: [ "TRANSCEIVER" ]
    /components/component/state/temperature/avg:
        platform_type: [ "TRANSCEIVER" ]
    
rpcs:
    gnmi:
        gNMI.Subscribe:
```


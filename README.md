# nymo-p1-emulator
DSMR emulator to simulate P1 events for Nymo Solyx Wateraccu

I've recently added a battery in my household and the Nymo Solyx cannot (from the P1 data it receives) know if its the battery providing surplus or if it is solar. 

This is a first proof of concept of an ESP32 that sends P1 events to the wireless transmitter of a Nymo Solyx.
This way, you can influence the events from ESPHOME/Home Assistant and include the battery in your calculation so it is not charging your Nymo with power from your battery.

In home assistant i'm doing the following:

    - name: "Gewenst overschot Nymo"
      unique_id: gewenst_overschot_nymo
      unit_of_measurement: W
      state_class: measurement
      state: >
          {% set afnemend_net = states('sensor.anker_solix_device_192_168_4_148_afnemend_vermogen_van_het_net') | float(0) %}
          {% set geleverd_batt = states('sensor.anker_solix_device_192_168_4_148_ontlaadvermogen_van_de_batterij') | float(0) %}
          {% set teruggeleverd_net = states('sensor.anker_solix_device_192_168_4_148_teruggeleverd_vermogen_aan_het_net') | float(0) %}
          {% set boost = states('input_number.nymo_boost') | float(0) %}
          {{ (teruggeleverd_net - afnemend_net - geleverd_batt + boost) | round(0) }}

And a number helper to put the boost number in.

The hardware used is described in the esphome script. 
I've just put this together and am testing it - so using something like this is fully at your own risk.

# Tesla PowerWall Battery Automation for Home Assistant

I created these files as a simple way to manage when my Tesla PowerWall battery is charging and discharging, when to allow importing from the grid, and when to operate in self-consumption mode. I wanted something that was fairly simple, while still allowing me to avoid importing electricity during peak pricing periods and to avoid charging the battery too fast (by importing electricity) and then exporting excess solar in the afternoon. My approach was to keep it simple and be "good enough" - there are much more sophisticated approaches available (e.g., [PredBat](https://springfall2008.github.io/batpred/)) that adjust setting on a more real-time basis based on actual and historical usage, predicted solar generation, battery charge rate, etc.

A few notes about my setup and approach:
* In the winter I consume more electricity than I generate and in the summer I generate much more than I consume (except on days when I charge my EV.)
* PG&amp;E is my utility. They have a 3-tier pricing structure (peak, semi-peak, off-peak), with peak/semi-peak from 3:00pm-Midnight.
* While my goal was to minimize cost (limit importing electricity from gird, exclusively use the battery during peak/semi-peak periods), I also optimized for simplicity and convenience. For example, I don't generate enough excess solar during the day to fully charge my EV and we often use our EV during the day during peak solar generation hours. I don't want to have to bother with charging my EV on successive days and I do not want to avoid using my EV mid-day because it is charging. I typically plug in my EV when the battery gets down to 30%. I plug it in late at night with charging autoamtilly starting in the very early morning. 
* The automation optimizes for electricity consumption and load shifting, not to protect against unexpected outages (i.e., I set a low backup reserve.) However, I created an automation to pause the charging automation when the Tesla "storm watch" setting is triggered for times when an outage might be more likley.
* My automation uses the Tesla Fleet integration ([setup video](https://www.youtube.com/watch?v=oEaEAiLOqO4&t=1232s)). There are also subscription services you can use to accomplish the same thing (e.g., [NetZero](https://www.netzero.energy/) or [Tessie](https://www.tessie.com/)), but HA provide ultimate flexibility and customization opportunites with no monthly subscription fee.
* My automation logic is different for winter and summer (which I defined based on past low solar generation/higher consumption and high solar generation/medium consumpiton periods, respectively.)
* Lastly, all of the values for the settings in these automations are fixed/hard coded. I did not set them up as variables, input select helpers, etc. This was done for simplicity and because I don't intend on changing them often.

# What Is Included

The follow files are include in this repo:
* **tesla_battery_automation.yaml** - primary automation file for configurating the *system operating mode*, *back-up reserve* and *charge from grid* settings during the winter and summer, as well as adjusting the system settings when the battery charge drops to a threshdold amount.
* **high_solar_gen_script.yaml** - script that defines the actions to run / settings to adjust during high solar production time period (summer.)
* **low_solar_gen_script.yaml** - script that defines the actions to run / settings to adjust durign low solar production time period (winter or cloudy/bad weather days.)
* **restart_automation_if_ha_restarts.yaml** - this automation resets the proper battery settings or restarts the charging automation if Home Assistant restarts, which may interrupt the 'telsa_battery_automation', which can run for a long time (>1 hour.)
* **storm_watch_toggle.yaml** - automation that disables the 'tesla_battery_automation' routine if the Telsa Storm Watch setting is turned on, allowing the Storm Watch settings to override.

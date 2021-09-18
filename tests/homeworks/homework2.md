## Homework 2

**Due date:** This homework is to be completed **before** the short course given on October 21. You need to make
sure the software has been properly installed and you have successfully completed the "homework 0" assignment.

**Purpose:** Learn how to measure snow depth levels with gnssrefl using GNSS data 


**Station:**
We will be using station **gls1**.
gls1 was installed at [Dye2](http://greenlandtoday.com/dye-2-a-relic-from-a-not-so-distant-past/?lang=en) on the Greenland Ice Sheet in 2011. 
The antenna is mounted on a long pole; approximately 3.5-meter of the pole was above the ice at the time of installation. 
A detailed discussion of the monument and 
data from the station can be found in [Larson, MacFerrin, and Nylen (2020)](https://tc.copernicus.org/articles/14/1985/2020/tc-14-1985-2020.pdf). 
The latest position time series for gls1 can be retrieved 
from the [Nevada Geodetic Laboratory](http://geodesy.unr.edu/gps_timeseries/tenv3/IGS14/GLS1.tenv3). 

As gls1 is on an ice sheet and the ice surface is relatively smooth in all directions, it 
is unlikely that a complicated azimuth mask will be required.

gls1 was originally installed with an elevation mask of 7 degrees, which is suboptimal for reflections research.
Even though the mask was later removed, we will use 7 degrees as the minimum elevation angle for all our analysis.
Similarly, even though the site managers later changed to enable L2C tracking, here we will only use L1 data.

Use the geoid tab at the [gnss-reflections webapp](https://gnss-reflections.org) to gather more information about the site. 
You can enter the station coordinates by hand if 
you know them, but since gls1 is part of a public archive provided by UNR, coordinates have been stored in the 
webapp. Just type in gls1 for the station name. Make a note of the station latitude, 
longitude, and ellipsoidal height that is returned by the webapp because you will need it later. 

**Using gnssrefl**

Our ultimate goal in this use case is to analyze one year of data. We have chosen the year 2012 
because there was a large melt event on the ice sheet. In order to set the proper quality control parameters, we will use 
<code>quickLook</code> for one day. First you need to translate one day of RINEX data.  Use the year 2012 and day of year 100.
Once you have successfully created a SNR file, run <code>quickLook</code>.
[For more details on translating RINEX files and quicklook output](https://github.com/kristinemlarson/gnssrefl).

Looking at the QC metrics plots created by <code>quickLook</code>, do you have some ideas on how to change the azimuth mask angles?

Now make SNR files for gls1 for the all of 2012. Use the <code>-weekly True</code> setting to save time.

We will next analyze a year of L1 GPS reflection data from gls1. We will use the default minimum and maximum 
reflector height values (0.5 and 6 meters). But for the reasons previously stated, you will want to 
set a minimum elevation angle of 7 degrees. We also specify that we only want to use the L1 data.
Use the utility <code>make_json_input</code> to set and store the analysis settings.

Hand-edit the azimuths in the json file to:

```
"azval": [ 40, 90, 90, 180, 180, 270, 270, 330 ],
```
    
Now that you have SNR files and json inputs, you can go ahead and estimate reflector heights for the year 2012 using <code>gnssir</code>.
Note that it is normal to see 'Could not read the first SNR file:' because we only created SNR files once a week.

Now you can use the <code>daily_avg</code> tool to compute a daily average reflector height for gls1. 
A median filter is set to 0.25 meters and 30 individual tracks are required in order to recover a daily average.
The data in this plot show you long-term accumulation as well as relatively small snow 
accumulation events. The overall plot is dominated by the large melt event in the summer of 2012.

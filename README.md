import xarray as xr
import pandas as pd
import matplotlib.pyplot as plt 
import numpy as np 
from warnings import filterwarnings
filterwarnings('ignore')
import cartopy.crs as ccrs
import cartopy.feature as cfeature 
1
import xarray as xr
2
import pandas as pd
3
import matplotlib.pyplot as plt 
4
import numpy as np 
5
from warnings import filterwarnings
6
filterwarnings('ignore')
7
import cartopy.crs as ccrs
8
import cartopy.feature as cfeature 
1
data = xr.open_mfdataset('C:/Users/hp/OneDrive/Desktop/PYTHON/THAILAND2/*.nc')
2
Thailand = data.where(data != -99.0)
1
Thailand_data = Thailand.precip
2
Thailand_data
xarray.DataArray'precip'datetime: 3652lat: 61lon: 35
Array	Chunk
Bytes	29.74 MiB	2.98 MiB
Shape	(3652, 61, 35)	(366, 61, 35)
Count	50 Tasks	10 Chunks
Type	float32	numpy.ndarray	
35
61
3652
Coordinates:
lon
(lon)
float32
97.25 97.5 97.75 ... 105.5 105.8
lat
(lat)
float32
20.75 20.5 20.25 ... 6.25 6.0 5.75
datetime
(datetime)
datetime64[ns]
2001-01-01 ... 2010-12-31
Attributes:
grid_mapping :
crs
1
monthly_data = Thailand_data.groupby('datetime.month')
2
yearly_data = Thailand_data.groupby('datetime.year')
1
# monthly rainfall totals
2
mon_rainfall_totals = monthly_data.sum('datetime')
3
​
4
# monthly climatologies rainfall totals
5
mon_mean_climo_totals = monthly_data.sum('datetime').mean('month')
1
# annual totals
2
annual_totals = yearly_data.sum()
3
​
4
# annual climatology totals
5
annual_climo_totals = yearly_data.sum('datetime').mean('year')
1
#plotting time series
2
fig,ax = plt.subplots(figsize = (12,7))
3
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
4
plt.plot(annual_totals.year,annual_totals.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
5
ax.set_title('Annual Precipitation Variability', fontweight = 'bold', fontsize=15 ,color = 'Orange')
6
ax.set_xlabel("YEAR", fontweight = 'bold',fontsize = 15, color= 'Orange')
7
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
8
plt.legend()
9
plt.show()

wetDays_per_year
1
#DRY days per year
2
dryDays_per_year = Thailand_data.where(Thailand_data < 1).groupby('datetime.year').count('datetime')
3
​
4
#WET days per year
5
wetDays_per_year = Thailand_data.where(Thailand_data >= 1).groupby('datetime.year').count('datetime')
6
​
7
#DRY days per MONTH
8
dryDays_per_month = Thailand_data.where(Thailand_data < 1).groupby('datetime.month').count('datetime')
9
​
10
#WET days per MONTH
11
wetDays_per_month = Thailand_data.where(Thailand_data >= 1).groupby('datetime.month').count('datetime')
12
​
13
# Extreme Rainfall >10mm
14
Xrain10 = Thailand_data.where(Thailand_data > 100).groupby('datetime.year').count('datetime')
15
​
16
# Extreme Rainfall >20mm
17
Xrain20 = Thailand_data.where(Thailand_data > 20).groupby('datetime.year').count('datetime')
1
#plotting time series for drydays per year
2
fig,ax = plt.subplots(figsize = (12,7))
3
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
4
plt.plot(annual_totals.year,dryDays_per_year.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
5
ax.set_title('COMPUTED DRY DAYS PER YEAR', fontweight = 'bold', fontsize=15 ,color = 'Orange')
6
ax.set_xlabel("YEAR", fontweight = 'bold',fontsize = 15, color= 'Orange')
7
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
8
plt.legend()
9
plt.show()

1
#plotting time series for wetdays per year
2
fig,ax = plt.subplots(figsize = (12,7))
3
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
4
plt.plot(annual_totals.year,wetDays_per_year.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
5
ax.set_title('COMPUTED WET DAYS PER YEAR', fontweight = 'bold', fontsize=15 ,color = 'Orange')
6
ax.set_xlabel("YEAR", fontweight = 'bold',fontsize = 15, color= 'Orange')
7
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
8
plt.legend()
9
plt.show()

1
fig,ax = plt.subplots(figsize = (12,7))
2
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
3
plt.plot(mon_rainfall_totals.month,dryDays_per_month.mean(['lon','lat'])*-1,color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
4
ax.set_title('COMPUTED DRY DAYS PER MONTH', fontweight = 'bold', fontsize=15 ,color = 'Orange')
5
ax.set_xlabel("MONTH", fontweight = 'bold',fontsize = 15, color= 'Orange')
6
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
7
plt.legend()
8
plt.show()

1
fig,ax = plt.subplots(figsize = (12,7))
2
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
3
plt.plot(mon_rainfall_totals.month,wetDays_per_month.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
4
ax.set_title('COMPUTED WET DAYS PER month', fontweight = 'bold', fontsize=15 ,color = 'Orange')
5
ax.set_xlabel("MONTH", fontweight = 'bold',fontsize = 15, color= 'Orange')
6
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
7
plt.legend()
8
plt.show()

97.25,105.8,20.75,5.75
1
# spatial plots for dry days per month
2
fig,ax=plt.subplots(3,4,figsize=(20,20), 
3
                    subplot_kw={'projection': ccrs.PlateCarree()},squeeze=0.1)
4
ax=ax.flatten()
5
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
6
for i in range(12):
7
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
8
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
9
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
10
#     ax[i].add_feature(cfeature.OCEAN)
11
#     ax[i].add_feature(cfeature.LAKES, color='blue')
12
#     ax[i].add_feature(cfeature.RIVERS)
13
    ax[i].set_extent([97.25,105.8,20.75,5.75])
14
    
15
    ax[i].set_title(month_names[i])
16
    cb= ax[i].contourf(dryDays_per_month.lon, dryDays_per_month.lat, dryDays_per_month[i],
17
                       cmap='RdBu', transform=ccrs.PlateCarree())
18
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
19
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
20
fig.subplots_adjust(wspace=-0.55, top=0.93)
21
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
22
# plt.savefig('done.png');

1
# spatial plots for dry and wet days per year and per month
2
fig,ax=plt.subplots(3,4,figsize=(20,20), 
3
                    subplot_kw={'projection': ccrs.PlateCarree()},squeeze=0.1)
4
ax=ax.flatten()
5
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
6
for i in range(12):
7
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
8
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
9
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
10
#     ax[i].add_feature(cfeature.OCEAN)
11
#     ax[i].add_feature(cfeature.LAKES, color='blue')
12
#     ax[i].add_feature(cfeature.RIVERS)
13
    ax[i].set_extent([97.25,105.8,20.75,5.75])
14
    
15
    ax[i].set_title(month_names[i])
16
    cb= ax[i].contourf(wetDays_per_month.lon, wetDays_per_month.lat, wetDays_per_month[i],
17
                       cmap='RdBu', transform=ccrs.PlateCarree())
18
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
19
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
20
fig.subplots_adjust(wspace=-0.55, top=0.93)
21
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
22
# plt.savefig('done.png');

1
​
1
# spatial plots for dry days per year
2
fig,ax=plt.subplots(5,2,figsize=(20,20), 
3
                    subplot_kw={'projection': ccrs.PlateCarree()})
4
ax=ax.flatten()
5
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct']
6
for i in range(10):
7
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
8
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
9
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
10
#     ax[i].add_feature(cfeature.OCEAN)
11
#     ax[i].add_feature(cfeature.LAKES, color='blue')
12
#     ax[i].add_feature(cfeature.RIVERS)
13
    ax[i].set_extent([97.25,105.8,20.75,5.75])
14
    ax[i].set_title(annual_totals.year.data[i])
15
    cb= ax[i].contourf(dryDays_per_year.lon, dryDays_per_year.lat, dryDays_per_year[i],
16
                       cmap='coolwarm', transform=ccrs.PlateCarree())
17
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
18
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
19
fig.subplots_adjust(wspace=-0.55, top=0.93)
20
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
21
# plt.savefig('done.png');

1
# spatial plots for dry days per year
2
fig,ax=plt.subplots(5,2,figsize=(20,20), 
3
                    subplot_kw={'projection': ccrs.PlateCarree()})
4
ax=ax.flatten()
5
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct']
6
for i in range(10):
7
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
8
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
9
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
10
#     ax[i].add_feature(cfeature.OCEAN)
11
#     ax[i].add_feature(cfeature.LAKES, color='blue')
12
#     ax[i].add_feature(cfeature.RIVERS)
13
    ax[i].set_extent([97.25,105.8,20.75,5.75])
14
    ax[i].set_title(annual_totals.year.data[i])
15
    cb= ax[i].contourf(wetDays_per_year.lon, wetDays_per_year.lat, wetDays_per_year[i],
16
                       cmap='coolwarm', transform=ccrs.PlateCarree())
17
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
18
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
19
fig.subplots_adjust(wspace=-0.55, top=0.93)
20
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
21
# plt.savefig('done.png');

1
#plotting time series for drydays per year
2
fig,ax = plt.subplots(figsize = (12,7))
3
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
4
plt.plot(Xrain10.year,Xrain10.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
5
ax.set_title('COMPUTED xtreme DAYS PER YEAR', fontweight = 'bold', fontsize=15 ,color = 'Orange')
6
ax.set_xlabel("YEAR", fontweight = 'bold',fontsize = 15, color= 'Orange')
7
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
8
plt.legend()
9
plt.show()

1
#plotting time series for drydays per year
2
fig,ax = plt.subplots(figsize = (12,7))
3
plt.subplots_adjust(hspace = 0.5, wspace = 0.2)
4
plt.plot(Xrain20.year,Xrain20.mean(['lon','lat']),color = 'blue', lw = 1.5 ,label= 'PRECIPITATION',marker='o')
5
ax.set_title('COMPUTED xtreme DAYS PER YEAR', fontweight = 'bold', fontsize=15 ,color = 'Orange')
6
ax.set_xlabel("YEAR", fontweight = 'bold',fontsize = 15, color= 'Orange')
7
ax.set_ylabel('PRECIPITATION', fontweight = 'bold', fontsize = 15, color= 'Orange')
8
plt.legend()
9
plt.show()

97.25,105.8,20.75,5.75
1
fig,ax=plt.subplots(2,5,figsize=(20,9), 
2
                    subplot_kw={'projection': ccrs.PlateCarree()})
3
ax=ax.flatten()
4
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct']
5
for i in range(10):
6
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
7
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
8
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
9
#     ax[i].add_feature(cfeature.OCEAN)
10
#     ax[i].add_feature(cfeature.LAKES, color='blue')
11
#     ax[i].add_feature(cfeature.RIVERS)
12
    ax[i].set_extent([97.25,105.8,20.75,5.75])
13
    ax[i].set_title(annual_totals.year.data[i])
14
    cb= ax[i].contourf(wetDays_per_year.lon, wetDays_per_year.lat, wetDays_per_year[i],
15
                       cmap='coolwarm', transform=ccrs.PlateCarree(),vmin= 100)
16
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
17
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
18
fig.subplots_adjust(wspace=-0.55, top=0.93)
19
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
20
# plt.savefig('done.png');

1
fig,ax=plt.subplots(3,4,figsize=(20,20), 
2
                    subplot_kw={'projection': ccrs.PlateCarree()},squeeze=0.1)
3
ax=ax.flatten()
4
month_names=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
5
for i in range(12):
6
    ax[i].add_feature(cfeature.COASTLINE.with_scale('110m'),linewidth=0.5)
7
    ax[i].add_feature(cfeature.BORDERS,linewidth=2)
8
    ax[i].add_feature(cfeature.STATES, linewidth=0.5)
9
#     ax[i].add_feature(cfeature.OCEAN)
10
#     ax[i].add_feature(cfeature.LAKES, color='blue')
11
#     ax[i].add_feature(cfeature.RIVERS)
12
    ax[i].set_extent([97.25,105.8,20.75,5.75])
13
    
14
    ax[i].set_title(month_names[i])
15
    cb= ax[i].contourf(dryDays_per_month.lon, dryDays_per_month.lat, dryDays_per_month[i],
16
                       cmap='RdBu', transform=ccrs.PlateCarree())
17
    color_bar=fig.add_axes([0.82,0.29,0.025,0.5])
18
fig.colorbar(cb,cax=color_bar,label='Soil Moisture')
19
fig.subplots_adjust(wspace=-0.55, top=0.93)
20
plt.suptitle('SOIL MOISTURE LEVEL 1', fontweight='bold');
21
# plt.savefig('done.png');

1
​### Hi there 👋

<!--
**byrbrew/byrbrew** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

## Processing Notes

### GNSS

+ Nevada Geodetic Lab has processed and released the preliminary GNSS offsets available at http://geodesy.unr.edu/news_items/20240104/us6000m0xl_forweb.txt. Accessed on 2024-01-04.

### SAR - ALOS-2

+ Free-and-open data available at JAXA: https://www.eorc.jaxa.jp/ALOS/jp/dataset/open_and_free/palsar2_l11_l22_j.htm
+ Preliminary analysis by JAXA: https://www.eorc.jaxa.jp/ALOS/jp/library/disaster/dis_pal2_noto_earthquake_20240110_j.htm
+ Preliminary analysis by GSI: https://www.gsi.go.jp/uchusokuchi/20240101noto_insar.html

#### data info

1. 20240101_20220926, 121, 0750_0770, U2-6L, Asc-L: `/home/yunjunz/data/2024NotoEQ/ALOS2_A121_20220926_20240101`
2. 20240102_20230606, 026, 2820_2840, U2-8L, Dsc-L: `/home/yunjunz/data/2024NotoEQ/ALOS2_D026_20230606_20240102`
3. 20240103_20231206, 127, 0720_0730, U2-9R, Asc-R: `/home/yunjunz/data/2024NotoEQ/ALOS2_A127_20231206_20240103`
4. 20240108_20230612, 128, 0720_0730, U3-13R, Asc-R: `/home/yunjunz/data/2024NotoEQ/ALOS2_A128_20230612_20240108`
5. 20240109_20211019, 019, 2860_2880, U3-10R, Dsc-R: `/home/yunjunz/data/2024NotoEQ/ALOS2_D019_20211019_20240109`

#### DEM

SNWE: 35 38 135 139

```bash
cd ~/data/2024NotoPenEQ
mkdir DEM; cd DEM
sardem --bbox 135 35 139 39 --make-isce-xml -d COP
multilook.py elevation.dem -r 3 -a 3 -o elevation90m.dem
wbd.py 35 38 135 139
```

#### InSAR

SNWE for geocode: 36.2, 37.7, 136.2, 138.0

```bash
cd ~/data/2024NotoPenEQ/ALOS2_A121_20220926_20240101
cd download; unzip '*.zip'; cd ..

# initial run
screen -S ALOS2_A121 -L
load_insar
alos2App.py alos2App.xml --steps

# re-run for dense offset
screen -S ALOS2_A121 -L
load_insar
export OMP_NUM_THREADS=48
alos2App.py alos2App_v2.xml --steps --start=dense_offset --end=filt_offset
```

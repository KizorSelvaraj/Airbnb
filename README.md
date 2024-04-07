# Airbnb
Airbnb Sample Dataset From MongoDB
Project Overview :
The Project consists of the dataset of Airbnb which was retrieved from MongoDB atlas. The data we have is the total number of accommodations listed on Airbnb worldwide.
We have 7 different countries, room types, property types, prices, hostname locations, and many more. With this, We are going to make a different analysis.
Price Analysis
Host Analysis
Room Type Distribution
Property Type Distribution
Geo-Visualization
Dashboard Creation Using Tableau
KINDLY CHECK OUT THE VIDEO OF THE PROJECT : - click here

I have listed the Feature of data below :
    Id                     0
    amenities              0
    Review_scores          0
    lati                   0
    longi                  0
    Country_code           0
    Country                0
    Street                 0
    host_name              0
    host_id                0
    guests_included        0
    extra_people           0
    cleaning_fee           0
    security_deposit       0
    price                  0
    bathrooms              0
    Listing_url            0
    number_of_reviews      0
    beds                   0
    bedrooms               0
    accommodates           0
    cancellation_policy    0
    maximum_nights         0
    Minimum_nights         0
    Bed_type               0
    Room_type              0
    Property_type          0
    House_rules            0
    Description            0
    Name                   0
    Is_exact_location      0

WorkFlow :
Step- 1 Retrieve Data From MongoDB Atlas :
Initialize Need to create an account with MongoDB Atlas. Secondly, Create a database and browse the sample_airbnb_dataset. We cannot download the dataset directly we need to make a connected MongoDB and Retrieve data as mentioned below.
        tst=[]
        for i in collection.find():
            data=dict(Id=i['_id'],
                      Listing_url=i['listing_url'],
                      Name=i.get("name"),
                      Description=i.get("description"),
                      Neighborhood_overview=i.get("neighborhood_overview"),
                      House_rules=i.get("house_rules"),
                      Property_type=i['property_type'],
                      Room_type=i['room_type'],
                      Bed_type=i['bed_type'],
                      Minimum_nights=int(i['minimum_nights']),
                      maximum_nights=int(i['maximum_nights']),
                      cancellation_policy=i['cancellation_policy'],
                      accommodates=int(i['accommodates'])
                      ....
        tst.append(data)  
        airbnb=pd.DataFrame(tst)

Step- 2 Data Cleaning :
Treating Null values in the dataset -
House_rules            2285
security_deposit       2084
cleaning_fee           1531
Review_scores          1474
Description              95
Finding the Duplicate values -
air_dup=Airbnb.duplicated().sum()
print("No of Duplicate values in Airbnb",air_dup)

No of Duplicate values in Airbnb 0
Once we Clean the data per the requirement of the project save it into the local directory -
Airbnb.to_csv("AirBnB01.csv",index=False)
Step- 3 EDA (exploratory data analysis) :
EDA Makes us understand the dataset and what analysis can be made from the dataset.
Countries and list of Airbnb Accommodation -
#Countries with high Airbnb
country_cnt=Counter(Airbnb1['Country'])
pd.DataFrame(country_cnt,index=np.arange(1)).sort_values(by=[0],axis=1,ascending=False)
United States	Turkey	Canada	Spain	Australia	Brazil	Hong Kong	Portugal	China
1222	661	649	633	610	606	600	555	19
Display as Bar Chart-
img

Room Type Distribution -
Roomdf=Airbnb1.groupby('Room_type').Id.count()
Roomdf=Roomdf.reset_index()
Roomdf=Roomdf.rename(columns={'Id':'Total_listed'})
label=Roomdf['Room_type']
values=Roomdf['Total_listed']
fig=go.Figure(data=[go.Pie(labels=label,values=values,hole=.5,title="Room Type Distribution")])
fig.update_layout(width=500,height=450)
fig.show()
img

Property Type Distribution -
  Roomdf=df3.groupby('Property_type').Id.count()
  Roomdf=Roomdf.reset_index()
  Roomdf=Roomdf.rename(columns={'Id':'Total_listed'})
  Roomdf=Roomdf.sort_values(by='Total_listed',ascending=True)
  fig=px.bar(Roomdf,x="Total_listed",y="Property_type",title="Property_type Distribution",color_discrete_sequence=px.colors.sequential.Blackbody_r)
  fig.update_layout(width=600,height=450)
img

Cancellation_policy -
        cal_df=df3.groupby('cancellation_policy').Id.count()
        cal_df=cal_df.reset_index()
        cal_df=cal_df.rename(columns={'Id':'Total_listed'})
        label=cal_df['cancellation_policy']
        values=cal_df['Total_listed']
        fig=go.Figure(data=[go.Pie(labels=label,values=values,hole=.5,title="Cancellation_policy Distribution")])
        fig.update_layout(width=600,height=450)
img

Price Analysis -
    price_Df=df3.groupby(['Country','Property_type','Room_type']).price.mean()
    price_Df=price_Df.reset_index()
    price_Df=price_Df.sort_values(by='price',ascending=True)
    fig=px.bar(price_Df,x='price',y='Country',title='Average Price distribution in Room type and Corresponding Countries',
        color='Room_type')
img img

Review and Pricing analysis
price_review=df3[['Review_scores','price',"host_name"]].sort_values(by='price')
fig=px.scatter(price_review,x='price',y='Review_scores',color='host_name',title='Price Distribution by Review Score')
img img

Host Analysis
re_100=df3[['host_name','number_of_reviews','Country']]
review_100=re_100.sort_values('number_of_reviews',ascending=False)
review_100=review_100[review_100['number_of_reviews']>=250].sort_values('number_of_reviews',ascending=False)
fig=px.bar(review_100,x='number_of_reviews',y='host_name',color='Country',title="High Number(>250) of Reviews by Host and Country")
fig.update_layout(width=550,height=450)
img img

Geospatial Visualization
fig = px.scatter_mapbox(df3, lat='lati', lon='longi', color='price', size='accommodates',
color_continuous_scale=px.colors.cyclical.Edge_r,hover_name='Name', mapbox_style="carto-positron", zoom=0)
fig.update_layout(width=1150,height=800,title='Geospatial Distribution of Listings')
img

Step- 4 Streamlit Application :
Develop an Interactive and Dynamic web page- img
Step- 5 Powerbi Dashboard Creation:
![powerbi](https://github.com/KizorSelvaraj/Airbnb/assets/149144969/8befea13-baee-41d5-b7ed-2df077a53f42)


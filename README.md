# Getting Started

- Please read the INSTRUCTIONS.md first
- For any questions around Create React App (CRA), reference
  CRA_DOCUMENTATION.md

# Code and Design Decisions
<!-- Please document your code & design decisions here. -->
//see test-documentation (word file) for all screenshots and proofs
in App.js
Firstly i created a route from listingproperty page 
 <!-- <BrowserRouter>
      <Routes>
      <Route path="/" element={<ListingProperty />} />
      <Route path="/listingproperty" element={<ListingProperty />} />
      
    </Routes>
  </BrowserRouter> -->

In page ListingProperty (path:  src > pages > LisitngProperty )
<!-- import React, { useState, useEffect } from 'react'; -->
used heartSwitch feature for "Favorite state"
<!-- import { HeartSwitch } from '@anatoliygatt/heart-switch';  -->
to use bootstrap
<!-- import 'bootstrap/dist/css/bootstrap.min.css'; -->
used bootsrap card ( to display property)
<!-- import Card from 'react-bootstrap/Card';  -->
used third party tool for getting data from RestApi
<!-- import axios from 'axios';  -->
imported style page for page ListingProperty
<!-- import './style.css'; -->

const ListingProperty = () => {
  const [propertyData, setPropertyData] = useState([]); // propertyData : will store all the data of api
  const [checkedItems, setChekcedState] = useState([]); // checkItem state : it contains all the hearted item (i.e property)
  const [items, setItems] = useState([]); // to store data  on local storage

  useEffect(async () => {
    let url = 'https://api.simplyrets.com/properties'; // restapi

    const result = await axios(url, {
      headers: {
        // Key = ‘Authorization’
        // Value = ‘Basic ‘+ base 64 encoding of a user ID and password
        Authorization: 'Basic ' + btoa(`${'simplyrets'}:${'simplyrets'}`), // passing credential
      },
    });
    // When sending data to a web server, the data has to be a string thats why we convert a JavaScript object into a string with JSON.stringify().
    //storing api data in local storage with key="items"
    localStorage.setItem('items', JSON.stringify(result.data));
    setPropertyData(result.data);
  }, []);

  //handleChange function will work whenever we will heart any property
  const handleChange = (e) => {
    let flag = checkedItems.includes(e.target.name); // flag to check if property is already favored or not (on double click it will unheart it )
    const { name } = e.target; // destructuring name (i.e mlsId)

    if (name && !flag) { 
      setChekcedState((oldState) => [...oldState, name]);
    } else {
      setChekcedState((oldState) => oldState.filter((item) => item !== name));
    }
  };
  //  //storing hearted data in local storage with key="hearted"
  localStorage.setItem('hearted', JSON.stringify(checkedItems));
  return (
    <React.Fragment>
      <div className="header-container">Property Listings</div>

      <div className="card-container">
        <div className="grid">
         
          {propertyData.map((val, key) => {//propertyData contain all api data so we are mapping it
            const { photos, property, address, listDate, listPrice, mlsId } =val; //destructuring to get required fields

            let temp = listDate.substring(0, 10);//getting substring which contains day-month-year
            let day = temp.substring(8, 10);
            let month = temp.substring(5, 7);
            let year = temp.substring(0, 4);
            let dollarUSLocale = Intl.NumberFormat('en-US'); // formating our price (listPrice) accouring to dollar
            dollarUSLocale = dollarUSLocale.format(listPrice);
            //this condition is for instaruction : - `bathsFull` + `bathsHalf` (ex: 1 full + 3 half = 2.5)
            if (property.bathsHalf % 2 == 0) { 
              var a = property.bathsHalf / 2;
              a = property.bathsFull + a;
            } else {
              a = property.bathsHalf / 2;
              a = property.bathsFull + a;
            }
            return (
              <Card
                style={{ width: '20rem', margin: '1rem', position: 'relative' }}
                key={key}
              >
                <Card.Img
                  variant="top"
                  src={photos[0]}
                  style={{ padding: '0.2rem' }}
                />
                <Card.Body style={{ position: 'absolute', right: '16px' }}>
                  <HeartSwitch
                    inactiveTrackFillColor="white"
                    inactiveTrackStrokeColor="white"
                    activeTrackFillColor="red"
                    activeTrackStrokeColor="red"
                    inactiveThumbColor="white"
                    activeThumbColor="red"
                    type="checkbox"
                    size="sm"
                    name={mlsId}
                    id={mlsId}
                    onChange={(event) => {
                      handleChange(event);
                    }}
                  />
                </Card.Body>
                <Card.Body>
                  <p style={{ fontWeight: 'bold' }}>
                    {property.bedrooms} BR | {a} BF | {property.area} Sq ft
                  </p>
                  <span style={{ fontWeight: 'bold', fontSize: '1.25rem' }}>
                    ${dollarUSLocale}
                  </span>
                  <p>{address.full}</p>
                  <Card.Text style={{ color: '#787878' }}>
                    Listed: {month}/{day}/{year}
                  </Card.Text>
                </Card.Body>
              </Card>
            );
          })}
        </div>
      </div>
    </React.Fragment>
  );
};

export default ListingProperty; -->




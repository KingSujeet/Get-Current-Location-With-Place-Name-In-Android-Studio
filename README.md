# Get Current Location With Place Name In Android Studio
<h4>Below is main code to get current location with place name.</h4>

<h3>✔ <b>Dependencies</b> --</h3>
<p>
    dependencies {<br><br>
  
    implementation 'com.google.android.gms:play-services-location:17.1.0'
    
      ----//------
}
</p>


    LocationManager locationManager;
    String provider;
    TextView txt_location;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        txt_location = findViewById(R.id.txt_location);

        // Getting LocationManager object
        locationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);

        // Creating an empty criteria object
        Criteria criteria = new Criteria();

        // Getting the name of the provider that meets the criteria
        provider = locationManager.getBestProvider(criteria, false);

        if (provider != null && !provider.equals("")) {

            // Get the location from the given provider
            if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
                // TODO: Consider calling
                //    ActivityCompat#requestPermissions
                // here to request the missing permissions, and then overriding
                //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
                //                                          int[] grantResults)
                // to handle the case where the user grants the permission. See the documentation
                // for ActivityCompat#requestPermissions for more details.
                return;
            }
            Location location = locationManager.getLastKnownLocation(provider);

            locationManager.requestLocationUpdates(provider, 20000, 1, this);

            if(location!=null)
                onLocationChanged(location);
            else
                Toast.makeText(getBaseContext(), "Location can't be retrieved", Toast.LENGTH_SHORT).show();

        }else{
            Toast.makeText(getBaseContext(), "No Provider Found", Toast.LENGTH_SHORT).show();
        }


    }



    @SuppressLint("SetTextI18n")
    @Override
    public void onLocationChanged(@NonNull Location location) {

        try {

            Geocoder geo = new Geocoder(this.getApplicationContext(), Locale.getDefault());
            List<Address> addresses = geo.getFromLocation(location.getLatitude(), location.getLongitude(), 1);
            if (addresses.isEmpty()) {
                txt_location.setText("Waiting for Location");
            }
            else {
                txt_location.setText(
                        addresses.get(0).getFeatureName()
                        + ", "
                        + addresses.get(0).getLocality()
                        +", "
                        + addresses.get(0).getAdminArea()
                        + ", "
                        + addresses.get(0).getCountryName());
            }
        }catch (Exception e){}

    }

    @Override
    public void onStatusChanged(String provider, int status, Bundle extras) {

    }

    @Override
    public void onProviderEnabled(@NonNull String provider) {

    }

    @Override
    public void onProviderDisabled(@NonNull String provider) {

    }

//Import a load of libraries
import android.content.Context;
import android.hardware.Sensor;
import android.hardware.SensorManager;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;

Context context;
SensorManager manager;
Sensor sensor;
AccelerometerListener listener;

//Accelerometer Sensor vars
float acc_x, acc_y, acc_z;

float x_pos_origin = width/2; //Middle of screen X
float y_pos_origin = height/2; //Middle of screen Y

//Positional vars
float new_x_pos;
float new_y_pos;
float old_x_pos;
float old_y_pos;

//vars
float radius = 0;
float angle = 0;

float r_colourSweepVal = 0;
float g_colourSweepVal = 0;
float b_colourSweepVal = 0;

//configuration vars (you can tweak these for differnt speed/colour effects)
float r_colourSpeed = 0.1;
float g_colourSpeed = 0.23;
float b_colourSpeed = 0.3; 

float angleSpeed = 0.2;
float radiusSpeed = 0.011;


void setup() 
{
  fullScreen();

  //Prep sensor management
  context = getActivity();
  manager = (SensorManager)context.getSystemService(Context.SENSOR_SERVICE);
  sensor = manager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);
  listener = new AccelerometerListener();
  manager.registerListener(listener, sensor, SensorManager.SENSOR_DELAY_GAME);

  //Set up screen
  //textFont(createFont("SansSerif", 10 * displayDensity));
  background(102);
  stroke(255);
  fill(102);
  
  //Set up vars
  x_pos_origin = width/2;
  y_pos_origin = height/2;

  old_x_pos = x_pos_origin;
  old_y_pos = y_pos_origin;
}


void draw() {
  //fill(102);
  //text("X: " + acc_x + "\nY: " + acc_y + "\nZ: " + acc_z+ "\nacc_x: " + acc_x+ "\nacc_y: " + acc_y + "\nold_x_pos: " + old_x_pos+ "\nold_y_pos: " + old_y_pos, 0, 0, width, height);
  
  spiral();
  
  setBrush();

  paint();
}

void setBrush(){
    //increment colour vars
  r_colourSweepVal+=r_colourSpeed; //change colours
  g_colourSweepVal+=g_colourSpeed;
  b_colourSweepVal+=b_colourSpeed;
  
  //constrain to limits (0-255)
  if (r_colourSweepVal >= 255) {
    r_colourSpeed = -r_colourSpeed;
  }
  if (r_colourSweepVal <= 0 ) {
    r_colourSpeed = -r_colourSpeed;
  }
  if (g_colourSweepVal >= 255) {
    g_colourSpeed = -g_colourSpeed;
  }
  if (g_colourSweepVal <= 0 ) {
    g_colourSpeed = -g_colourSpeed;
  }
  if (b_colourSweepVal >= 255) {
    b_colourSpeed = -b_colourSpeed;
  }
  if (g_colourSweepVal <= 0 ) {
    b_colourSpeed = -b_colourSpeed;
  }
  
  //Set stroke colour
  float red = 255 - r_colourSweepVal;
  float green =  255 - g_colourSweepVal;
  float blue = 255 - b_colourSpeed;
  stroke(color(red, green, blue));
  
  //Set the brush size based on z axis (vertical jolts etc)
  float brush = acc_z;  
  strokeWeight(brush);  
}

void spiral(){
  angle+=angleSpeed; //increase angle
  //constrain to 360 degs
  if (angle>360) {
    angle = 0;
  }
  
  radius+=radiusSpeed; //increase radius
  //if radius meets the screen edge go back to center
  if (radius>width || radius>height) {
    radius = 0;
  }  
}

void paint()
{
 //calculate new xy (based on spiral)
  new_x_pos = x_pos_origin + cos(radians(angle))*radius+acc_x;
  new_y_pos = y_pos_origin + sin(radians(angle))*radius+acc_y;
  
  //constrain to screen 
  if ( new_x_pos > width) {
    new_x_pos = width; 
    radius=0;
  }
  if ( new_x_pos < 0) {
    new_x_pos = 0; 
    radius=0;
  }
  if ( new_y_pos > height) {
    new_y_pos = height; 
    radius=0;
  }
  if ( new_y_pos <0) {
    new_y_pos = 0; 
    radius=0;
  }
  
  //Draw a line from old pos to new
  line(old_x_pos, old_y_pos, new_x_pos, new_y_pos);

  //Lastly, save new xy as old
  old_x_pos = new_x_pos;
  old_y_pos = new_y_pos; 
}

/*void mouseReleased() {  
  //Didn't quite get this working yet...
  //The idea was to have some way of saving what you see as images as it progresses over time
  //better method needed
  PImage img = createImage(width, height, RGB);
  img.loadPixels();
  for (int i = 0; i < img.pixels.length; i++) {
    img.pixels[i] = color(0, 90, 102);
  }
  img.updatePixels();
  img.save("img.jpg");
}*/

class AccelerometerListener implements SensorEventListener {
  public void onSensorChanged(SensorEvent event) {
    acc_x = event.values[0] * 10;
    acc_y = event.values[1] * 10;
    acc_z = event.values[2];
  }
  public void onAccuracyChanged(Sensor sensor, int accuracy) {
  }
}

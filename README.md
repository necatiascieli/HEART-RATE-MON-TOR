# **HEARTBEAT CONTROL IN SPORTS**
Firstly we are starting with what is a heartbeat? The pulse, popularly known as the heartbeat, is the force that the constantly working heart exerts on the blood vessels by contracting and relaxing. With each contraction, the blood is first sent to the aorta and then to the other vessels with pressure. Thanks to their elastic structure, the vessels constantly expand and then narrow. This enlargement can be felt by groping in places close to the surface, such as the wrist, temple and groin. Heart rate may differ from person to person due to different factors such as age, gender and physical structure, as well as due to different reasons such as body temperature, physiological changes, physical activity and emotional changes, drug use, illness and stress.  It is normal to observe changes. The important thing at this point is that the pulse is regular.

## WHAT SHOULD BE YOUR RESTING PULSE ?
The pulse rate in adults should be between 60-100 per minute during a period when the person is physically rested and mentally calm. Athletes may have a lower heart rate, and it is considered normal to have a heart rate of 40–60 per minute.

## WHY IS HEART RATE CONTROL IMPORTANT IN SPORTS ?
Many coaches take extreme measures to monitor the physical condition of their athletes. The need to ensure that the athlete was in the best possible condition to both train and compete meant that a great deal of time and money was spent over the years trying to pinpoint this information. Almost every method imaginable has been tried to assess an athlete's physical readiness for training or performance, whether it's the use of expensive equipment or time-consuming testing.

However, there is one particular test that is extremely effective, although it can be performed in bed and takes less than a minute to be administered by the athlete himself - checking your Resting Heart Rate (RHR). Monitoring your Resting Heart Rate is a very effective way to assess an athlete's fitness level and aid recovery, and is a very reliable indicator of cardiovascular health in general.

![heart beat](https://blog.decathlon.com.tr/wp-content/uploads/2021/01/kardiyo.jpg)

# **HOW THE HEARTBEAT SENSOR WORKS**
Heart rate, which is important to control in every part of our lives, is one of the most important health data that is controlled in the training of athletes. This heart rate measurement data, which allows players to guide their training tempo, saves athletes from experiencing serious heart problems on the field or in training. For example, Christian Eriksen, one of the Premier League teams, Tottenham Hotspur player, experienced such a health problem in the Denmark-Finland match in the EURO 2020 tournament held in 2021. In order to avoid such situations, heart rate control is of great importance.

In this project, I designed a heart rate monitor for the players of Manchester City, one of the Premier League teams. In this way, the heart rhythm data of the players will be easily examined by the medical team working at the club and they will be able to make an early diagnosis in case of a contrary situation.

## 1)USED MATERIALS
**.** **1x Arduino Uno**				
**.** **1x KY-039 HEARTBEAT SENSOR**	
**.** **3x Jumper Cables**

![arduino uno](https://www.direnc.net/arduino-uno-r3-smd-arduino-ana-board-china-36886-17-B.jpg)
![ky-03](https://st1.myideasoft.com/idea/dz/31/myassets/products/629/robitshop-parmaknabiz-1.jpg?revision=1612995787)
![jumper cables](https://www.okulstore.com/ProductImages/102214/big/jumper_kablo_erkek_erkek.jpg)

## 2)WHAT IS THE KY-039 / HEARTBEAT SENSOR
The KY-039 heartbeat sensor is designed to detect a pulse while a human finger is place between the infrared diode and the photo transistor.  The pulse will be represented on the signal output pin.

This sensor works by using a photo transistor to detect the presence of light, in this case how much light is passing through a finger.  When blood moves, the amount of light changes and that change can be detected as a pulse.
### Datasheet
KY-039 sensor has 3 inputs. First input is called signal, second input is Vcc and the third one is ground port.

### Specifications
 | Plugin			| Values
 |	:---:			| :---:
 |Frequency (Hz)        | 50 / 60 Hz
 |Ip Rating			| IP65
 |Temparature (deg. Celcius)		| 0 to 55 Degree Celcius
 |Voltage (V)			| 6 to 12 V DC	
 |Product Code		| KY-039
 |Size				| 1.9 x 1.5 cm
 ### ARDUINO CONNECTION
 
 ![connection](https://electropeak.com/learn/wp-content/uploads/2021/02/Heartbeat-Sensor-KY-039-circuit.jpg)
 ## Arduino Code of The System
 ```sh
 #define samp_siz 4
#define rise_threshold 5

// Pulse Monitor Test Script
int sensorPin = 0;

void setup() {
    Serial.begin(9600);
}

void loop ()
{
    float reads[samp_siz], sum;
    long int now, ptr;
    float last, reader, start;
    float first, second, third, before, print_value;
    bool rising;
    int rise_count;
    int n;
    long int last_beat;

    for (int i = 0; i < samp_siz; i++)
      reads[i] = 0;
    sum = 0;
    ptr = 0;

    while(1)
    {
      // calculate an average of the sensor
      // during a 20 ms period (this will eliminate
      // the 50 Hz noise caused by electric light
      n = 0;
      start = millis();
      reader = 0.;
      do
      {
        reader += analogRead (sensorPin);
        n++;
        now = millis();
      }
      while (now < start + 20);  
      reader /= n;  // we got an average
      
      // Add the newest measurement to an array
      // and subtract the oldest measurement from the array
      // to maintain a sum of last measurements
      sum -= reads[ptr];
      sum += reader;
      reads[ptr] = reader;
      last = sum / samp_siz;
      // now last holds the average of the values in the array

      // check for a rising curve (= a heart beat)
      if (last > before)
      {
        rise_count++;
        if (!rising && rise_count > rise_threshold)
        {
          // Ok, we have detected a rising curve, which implies a heartbeat.
          // Record the time since last beat, keep track of the two previous
          // times (first, second, third) to get a weighed average.
          // The rising flag prevents us from detecting the same rise more than once.
          rising = true;
          first = millis() - last_beat;
          last_beat = millis();

          // Calculate the weighed average of heartbeat rate
          // according to the three last beats
          print_value = 60000. / (0.4 * first + 0.3 * second + 0.3 * third);
          
          Serial.print(print_value);
          Serial.print('\n');
          
          third = second;
          second = first;
          
        }
      }
      else
      {
        // The curve is falling
        rising = false;
        rise_count = 0;
      }
      before = last;
      
      
      ptr++;
      ptr %= samp_siz;

    }
}
 
 
 ```





## BLOCK DIAGRAM DESIGN BY LABVIEW
![labview block diagram](https://user-images.githubusercontent.com/98168728/171508981-712777cf-e9b0-4e01-b7cc-28f18f452ee0.png)
## FRONT PANEL DESIGN BY LABVIEW
![labview front panel](https://user-images.githubusercontent.com/98168728/171509216-5fea480d-93c6-43c4-804c-620237ee6fd1.png)



  



---
title: 'Broncode / Github'
taxonomy:
    category:
        - docs
---

Er zijn diverse varianten van software in omloop en je kan natuurlijk ook je eigen software schrijven. Om mensen op weg te helpen tref je hier wat links aan, **zonder enige garantie** natuurlijk.

De initiële versie staat op [PsychoMnts](https://github.com/PsychoMnts/Xiaomi-Scooter-Motion-Control) zijn GitHub account. Bij de lijst van [forks](https://github.com/PsychoMnts/Xiaomi-Scooter-Motion-Control/network/members) kan je zien wie hier verder mee aan de haal is gegaan. 
**Tip:** Diverse forks zijn echte verbeteringen op het origineel, maar sommige werken niet. Aan jou om uit te proberen!

Door diverse leden uit de groep is onderstaande code als goed werkend bestempeld, je hoeft hier enkel de waarde van het scooter type aan te passen. 
We testen nu met een software die geschikt is voor meerdere modellen zonder aanpassingen per model. Zodra die gereed is zal hij op [GitHub](https://github.com/mysidejob/) geplaatst worden. Hierin zal ook automatisch de Ninebot variant werken, de onderstaande code is hiervoor niet geschikt in verband met een kleine aanpassing in het protocol.

    #include <Arduino.h>
    #include <arduino-timer.h>
    #include <SoftwareSerial.h>

    // ==========================================================================
    // ================================ SETUP ===================================
    // ==========================================================================
    //
    // Supported models:
    // 0 = Xiaomi Mi Scooter Essential
    // 1 = Xiaomi Mi Scooter 1S (Not tested)
    // 2 = Xiaomi Mi Scooter Pro 2
    //
    const int SCOOTERMODEL    = 2;    // Select scooter type 
    const int DURATION_LOW    = 1000; // Throttle duration  (in millisec)
    const int DURATION_HIGH   = 4000; // Throttle duration (in millisec)
    const int THROTTLE_LOW    = 25;   // Throttle setting  (in full percentage)
    const int THROTTLE_HIGH   = 100;  // Throttle setting  (in full percentage)
    const int SPEED_LOW       = 5;    // Speed when to use low throttle/duration (in kmph)
    const int SPEED_HIGH      = 15;   // Speed when to use high throttle/duration (in kmph)
    const int THROTTLE_DELAY  = 1500;  // Time before accepting a new boost (in millisec)
    const int READINGS_COUNT  = 30;   // Amount of speed readings
    const int THROTTLE_PIN    = 10;   // Pin of programming board (9=D9 or 10=D10)
    const int SERIAL_PIN      = 2;    // Pin of serial input (2=D2)

    //
    // ==========================================================================
    // =============================  PROTOCOL ==================================
    // ==========================================================================
    //
    // Message structure:
    // + --- + --- + --- + --- + --- + --- + --- + --- + --- +
    // | x55 | xAA |  L  |  D  |  T  |  C  | ... | ck0 | ck1 |
    // + --- + --- + --- + --- + --- + --- + --- + --- + --- +
    // 
    // x55 = FIXED
    // xAA = FIXED
    // L   = MESSAGE LENGTH
    // D   = DESTINATION (20 = DASH TO MOTOR, 21 = MOTOR TO DASH, 22 = DASH TO BATT, 23 = BATT TO DASH, 24 = MOTOR TO BATT, 25 = BATT TO MOTOR)
    // T   = TYPE (1 = READ, 3 = WRITE, 64 = ?, 65 = ?)
    // C   = COMMAND (?)
    // ... = DATA
    // ck0 = CHECKSUM
    // ck1 = CHECKSUM
    //
    // ==========================================================================

    // CODE BELOW THIS POINT

    SoftwareSerial SoftSerial(SERIAL_PIN, 3); // RX, TX
    auto timer_m = timer_create_default();

    // States
    #define READY 0
    #define BOOST 1

    // Protocol decoding
    #define DASH2MOTOR 0x20
    #define MOTOR2DASH 0x21
    #define DASH2BATT  0x22
    #define BATT2DASH  0x23
    #define MOTOR2BATT 0x24
    #define BATT2MOTOR 0x25
    #define BRAKEVALUE 0x65
    #define SPEEDVALUE 0x64
    #define SERIALVALUE 0x31

    // Variables
    bool isBraking = true;                    // brake activated
    int speedCurrent = 0;                     // current speed
    int speedCurrentAverage = 0;              // the average speed over last X readings
    int speedLastAverage = 0;                 // the average speed over last X readings in the last loop
    int speedReadings[READINGS_COUNT] = {0};  // the readings from the speedometer
    int indexcounter = 0;                            // the indexcounter of the current reading
    int speedReadingsSum = 0;                 // the sum of all speed readings
    uint8_t state = READY;                    // current state
    int brakePrevious = 0;                    // count brake off

    uint8_t readBlocking() {
        while (!SoftSerial.available())
        delay(1);
        return SoftSerial.read();
    }

    void setup() {
        Serial.begin(115200); SoftSerial.begin(115200); // Start reading SERIAL_PIN at 115200 baud
        TCCR1B = TCCR1B & 0b11111001; // TCCR1B = TIMER 1 (D9 and D10 on Nano) to 32 khz
        setThrottle(0); // Set throtte to 0%
    }

    uint8_t buff[256];

    void loop() {
        while (readBlocking() != 0x55);
        if (readBlocking() != 0xAA)return;
        uint8_t len = readBlocking();
        buff[0] = len;
        if (len > 254)return;
        uint8_t addr = readBlocking();
        buff[1] = addr;
        uint16_t sum = len + addr;
        for (int i = 0; i < len; i++) {
            uint8_t curr = readBlocking();
            buff[i + 2] = curr;
            sum += curr;
        }
        uint16_t checksum = (uint16_t) readBlocking() | ((uint16_t) readBlocking() << 8);
        if (checksum != (sum ^ 0xFFFF))return;    
        switch (buff[1]) {
            case DASH2MOTOR: // Destintion: Dash to motor
                switch (buff[2]) {
                    case BRAKEVALUE:
                        isBraking = (buff[6]>47);

                        if(isBraking){
                            setThrottle(0);
                        }

                        brakePrevious = isBraking;

                        break;
                }
            case MOTOR2DASH: // Destination: motor to dash
                switch (buff[2]) {
                    case SPEEDVALUE:
                        if (buff[8] != 0)speedCurrent = buff[8];
                        //if(buff[8] != 0)d(buff,len);
                        break;
                }
        }

        speedReadingsSum = speedReadingsSum - speedReadings[indexcounter];
        speedReadings[indexcounter] = speedCurrent;
        speedReadingsSum = speedReadingsSum + speedReadings[indexcounter];
        indexcounter++;
        if(indexcounter > 29)indexcounter = 0;
        speedCurrentAverage = speedReadingsSum / READINGS_COUNT;

          motion_control();

        timer_m.tick();

    }

    bool release_throttle(void * ) { // After boost, wait for new boost
        setThrottle(SCOOTERMODEL==0 ? 10 : 0); // 10% throttle for Essential, 0% for Pro 2 and 1S
        timer_m.in(THROTTLE_DELAY, motion_wait);
        return false;
    }

    bool motion_wait(void * ) { // After boost delay
        state = READY;
        return false;
    }

    bool d(uint8_t buff[256],int len){
        Serial.print("DATA (");
        Serial.print(len);
        Serial.print("): HEX");
        for (int i = 0; i < len; i++) {
            Serial.print(buff[i], HEX);
            Serial.print(" ");
        }
        Serial.print(" / ");
        Serial.print(buff[len]);
        Serial.println(" ");
        return false;
    }

    void motion_control() {
        if (speedCurrent < 5 || isBraking) { // If braking or under 5kmh
            setThrottle(0); 
            state = READY;
            timer_m.cancel();
        } else if (state == READY && speedCurrentAverage - speedLastAverage > 0 && speedCurrentAverage > 5) { // If ready, above 5 kmh and increasing speed
            if (speedCurrentAverage<=SPEED_LOW) { // If speed is low or lower
                  setThrottle(THROTTLE_LOW); 
                  timer_m.in(DURATION_LOW, release_throttle);
            } else if (speedCurrentAverage>=SPEED_HIGH) { // If speed is high or higher
                  setThrottle(THROTTLE_HIGH);
                  timer_m.in(DURATION_HIGH, release_throttle);
            } else { // If speed between low and high,
                  int speedDiff = SPEED_HIGH-SPEED_LOW;
                  int throttleDiff = THROTTLE_HIGH-THROTTLE_LOW;
                  int durationDiff = DURATION_HIGH-DURATION_LOW;
                  setThrottle(THROTTLE_LOW+throttleDiff/speedDiff*(speedCurrentAverage-SPEED_LOW)); // Open throttle gradually
                  timer_m.in(DURATION_LOW+durationDiff/speedDiff*(speedCurrentAverage-SPEED_LOW), release_throttle); // Start timer to stop throttle
            }
            state = BOOST;
        }
        speedLastAverage = speedCurrentAverage;
    }

    int setThrottle(int percentage) {
      analogWrite(THROTTLE_PIN, percentage*1.88+45); // Percentage in whole numbers: 0-100, results in a value of 45-233
    }
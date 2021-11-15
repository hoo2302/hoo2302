const int stepPin = 9; 
const int dirPin = 2; 
const int enPin = 5;
int trig = 10;
int echo = 8;


void setup() {
 
  pinMode(stepPin,OUTPUT);
  pinMode(dirPin,OUTPUT);
  pinMode(enPin,OUTPUT);
  digitalWrite(enPin,LOW);
  Serial.begin(9600);
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);
  
}

void loop() {

  
  digitalWrite(trig, LOW);
  digitalWrite(echo, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(500);
  digitalWrite(trig, LOW);

  Serial.println("\n");

  unsigned long duration = pulseIn(echo, HIGH);
  float distance = duration / 29.0 / 2.0;
  Serial.print(distance);
  Serial.println("cm distance");


  float target;
  if(Serial.available()>0)
  target = Serial.parseFloat();
  Serial.print(target);
  Serial.println("cm target");


  float setdis = distance - target;


  if(setdis > 1.2){
  digitalWrite(dirPin,HIGH); 
  for(int x = 0; x < 12000; x++) {
  digitalWrite(stepPin,HIGH);
  delayMicroseconds(60);
  digitalWrite(stepPin,LOW);
  delayMicroseconds(60);
 
  }
 }else if(setdis < -1.2){
  digitalWrite(dirPin,LOW);
  for(int x = 0; x < 12000; x++) {
  digitalWrite(stepPin,HIGH);
  delayMicroseconds(60);
  digitalWrite(stepPin,LOW);
  delayMicroseconds(60);
  
  }
 }else if( -1.2 < setdis < 1.2){  
 digitalWrite(dirPin,LOW);

  for(int x = 0; x < 12000; x++) {
  digitalWrite(stepPin,LOW);
  delayMicroseconds(60);
   while(1){
     if(Serial.available()>0)
     break;
  }
 }
 }
}

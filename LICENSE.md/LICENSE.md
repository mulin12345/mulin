#define EN1 4
#define apwm1 5//反传
#define apwm2 6//正转
#define EN2 8
#define bpwm1 9
#define bpwm2 10
int RED1 = 38;
int RED2 = 39;
int value1 = 0;
int value2 = 0;
const int TrigPin1= 2; 
const int EchoPin1= 3;
const int TrigPin2= 40; 
const int EchoPin2= 41;
const int TrigPin3= 42; 
const int EchoPin3= 43;
const int TrigPin4= 44; 
const int EchoPin4= 45;
float cm1;  
float cm2;
float cm3;
float cm4;      
int temp=0;
long startime;
void setup()
{
  Serial.begin(9600);
  pinMode(EN1, OUTPUT);
  pinMode(EN2, OUTPUT);
  pinMode(RED1, INPUT);
  pinMode(RED2, INPUT);
  pinMode(TrigPin1, OUTPUT); 
  pinMode(EchoPin1, INPUT);
  pinMode(TrigPin2, OUTPUT); 
  pinMode(EchoPin2, INPUT);
  pinMode(TrigPin3, OUTPUT); 
  pinMode(EchoPin3, INPUT);
  pinMode(TrigPin4, OUTPUT); 
  pinMode(EchoPin4, INPUT);
  pinMode(apwm1,OUTPUT);
  pinMode(apwm2,OUTPUT);
  pinMode(bpwm1,OUTPUT);
  pinMode(bpwm2,OUTPUT);
  Serial.begin(9600);
}
void wheel1(int t1)
{
  if ( t1 > 0)
  {
    digitalWrite(EN1, HIGH);
    analogWrite(apwm1,0) ;
    analogWrite(apwm2,t1);
  } 
   else if (t1 < 0) 
    {
             t1 = -t1;
             digitalWrite(EN1, HIGH);
             analogWrite(apwm1, t1);
             analogWrite(apwm2, 0);
     } 
    else  if (t1 = 0)
    {
             digitalWrite(EN1, HIGH);
             analogWrite(apwm1, 0);
             analogWrite(apwm2, 0);
    }
}

void wheel2(int t2) 
{
           if ( t2 > 0) 
           {
                  digitalWrite(EN2, HIGH);
                  analogWrite(bpwm1, t2);
                  analogWrite(bpwm2, 0);
            } 
            else if (t2 < 0)
           {
                  t2 = -t2;
                  digitalWrite(EN2, HIGH);
                  analogWrite(bpwm1, 0);
                  analogWrite(bpwm2, t2);
            } 
            else  if (t2 = 0) 
           {
                  digitalWrite(EN2, HIGH);
                  analogWrite(bpwm1, 0);
                  analogWrite(bpwm2, 0);

            }
}

void loop()
{ 
            value1 = digitalRead(RED1);
            value2 = digitalRead(RED2);  
            
            digitalWrite(TrigPin1, LOW); //低高低电平发一个短时间脉冲去TrigPin 
            delayMicroseconds(1); 
            digitalWrite(TrigPin1, HIGH); 
            delayMicroseconds(5); 
            digitalWrite(TrigPin1, LOW); 
            cm1 = pulseIn(EchoPin1, HIGH) / 58.0; //将回波时间换算成cm 
           
          
            digitalWrite(TrigPin2, LOW); //低高低电平发一个短时间脉冲去TrigPin 
            delayMicroseconds(1); 
            digitalWrite(TrigPin2, HIGH); 
            delayMicroseconds(5); 
            digitalWrite(TrigPin2, LOW); 
            cm2 = pulseIn(EchoPin2, HIGH) / 58.0; //将回波时间换算成cm 
            
            
            digitalWrite(TrigPin3, LOW); //低高低电平发一个短时间脉冲去TrigPin 
            delayMicroseconds(1); 
            digitalWrite(TrigPin3, HIGH); 
            delayMicroseconds(5); 
            digitalWrite(TrigPin3, LOW); 
            cm3= pulseIn(EchoPin3, HIGH) / 58.0; //将回波时间换算成cm 
            
          
            digitalWrite(TrigPin4, LOW); //低高低电平发一个短时间脉冲去TrigPin 
            delayMicroseconds(1); 
            digitalWrite(TrigPin4, HIGH); 
            delayMicroseconds(5); 
            digitalWrite(TrigPin4, LOW); 
            cm4 = pulseIn(EchoPin4, HIGH) / 58.0; //将回波时间换算成cm 
         
            // Serial.print(cm11); 
            //Serial.print("cm"); 
            // Serial.println(); 
//**************************************************当1超声波感应到距离**************************************************************************
            if((cm1 < 70) && (cm2 > 70)&& (cm3 > 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  //1超声波  正前方攻击
            {
                   
                          wheel1(200);
                          wheel2(200);
                    
              }
          

        
              else if ( (cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//1超声波  左转
              { 
                         startime=millis();
                        if(millis()-startime<500)
                        {
                      
                               wheel1(150);
                               wheel2(-150);
                     
                        }
                        else
                       {
                               temp=1;
                        }
     
               }
               else if((cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//1超声波 右转
               { 
                       startime=millis();
                       if(millis()-startime<500)
                       {
                      
                               wheel1(-150);
                               wheel2(150);
                     
                        }
                        else
                        {
                         temp=1;
                        }
               }
                else if((cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//1超声波 后转往左
                {   
                           startime=millis();
                           if(millis()-startime<500)
                           {
                               
                                wheel1(200);
                                wheel2(-200);
                             
                             }
                             else
                            {
                             temp=1;
                             } 
           
                  }  
//////////////////////////////////////////////////////////////     
                  else if((cm1 < 70) && (cm2 < 70)&& (cm3 > 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  // 1、2超声波 正前方攻击
                 {
                              
       							    wheel1(200);
                                    wheel2(200);
                               
                           
                  }
          
     
        
                  else if ((cm1 < 70)&& (cm2 < 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//1、2超声波  左转

                  { 
                             startime=millis();
                            if(millis()-startime<500)
                            {
                                
                                    wheel1(150);
                                    wheel2(-150);
                                   
                             }
                             else
                            {
                                    temp=1;
                             }
               
                  }
                   else if((cm1 < 70)&& (cm2 < 70)&& (cm3 > 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//1、2超声波 右转
                  { 
                               startime=millis();
                                if(millis()-startime<500)
                                {
                                    
                                         wheel1(-150);
                                         wheel2(150);
                                   
                                 }
                                 else
                                {
                                         temp=1;
                                 }
                   }
                    else if((cm1 < 70)&& (cm2 < 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//1、2超声波 倒、左转
                    {   
                                startime=millis();
                                if(millis()-startime<500)
                                {
                                   
                                        wheel1(200);
                                        wheel2(-200);
                                 
                                 }
                                 else
                                {
                                        temp=1;
                                 }                
                      }   
////////////////////////////////////////////////////////////////////////
                      else if((cm1 < 70) && (cm2 > 70)&& (cm3 < 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  // 1、3超声波 正前方攻击
                       {
                                
                                          wheel1(200);
                                          wheel2(200);
                                   
                                  
                         }
    
                          else if ((cm1 < 70)&& (cm2 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//1、3超声波 左转
                      
                          { 
                                        startime=millis();
                                        if(millis()-startime<500)
                                        {
                                            
                                               wheel1(150);
                                               wheel2(-150);
                                           
                                         }
                                         else
                                        {
                                               temp=1;
                                         }
     
                          }
                          else if((cm1 < 70)&& (cm2 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//1、3超声波 右转
                         { 
                                        startime=millis();
                                        if(millis()-startime<500)
                                        {
                                            
                                               wheel1(-150);
                                               wheel2(150);
                                           
                                         }
                                         else
                                        {
                                                temp=1;
                                         }
                         }
                         else if((cm1 < 70)&& (cm2 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//1、3超声波 倒、左转
                         {   
                                      startime=millis();
                                      if(millis()-startime<500)
                                      {
                                         
                                            wheel1(200);
                                            wheel2(-200);
                                       
                                       }
                                       else
                                      {
                                            temp=1;
                                       } 
                            
                         }   
////////////////////////////////////////////////////////////////////////
                        else if((cm1 < 70) && (cm2 > 70)&& (cm3 > 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  // 1、4超声波 正前方攻击
                         {
                                
                                        wheel1(200);
                                        wheel2(200);
                                   
                                   
                          }
                     
        
                          else if ((cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//1、4超声波 左转
                      
                          { 
                                        startime=millis();
                                        if(millis()-startime<500)
                                        {
                                            
                                              wheel1(150);
                                              wheel2(-150);
                                           
                                         }
                                         else
                                        {
                                              temp=1;
                                         }
     
                           }    
                           else if((cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//1、4超声波 右转
                           { 
                                        startime=millis();
                                        if(millis()-startime<500)
                                        {
                                            
                                               wheel1(-150);
                                               wheel2(150);
                                           
                                         }
                                         else
                                        {
                                              temp=1;
                                         }
                            }
                            else if((cm1 < 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//1、4超声波 倒左转
                            {   
                                        startime=millis();
                                        if(millis()-startime<500)
                                        {
                                           
                                              wheel1(200);
                                              wheel2(-200);
                                         
                                         }
                                         else
                                        {
                                              temp=1;
                                         } 
                              
                              } 
/////////////////////////////////////////////////////							  
                             else if((cm1 < 70) && (cm2 <70)&& (cm3 < 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)//1、2、3超声波 正前方攻击
                             {
                                        
                                              wheel1(200);
                                              wheel2(200);
                                         
                                        
                             }
     
        
                             else if ((cm1 < 70)&& (cm2 < 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//1、2、3超声波  左转
                          
                            { 
                                            startime=millis();
                                            if(millis()-startime<500)
                                            {
                                                
                                                  wheel1(150);
                                                  wheel2(-150);
                                               
                                             }
                                             else
                                            {
                                                  temp=1;
                                             }
                               
                              }
                              else if((cm1 < 70)&& (cm2 < 70)&& (cm3 < 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//1、2、3超声波   右转
                              { 
                                 startime=millis();
                                            if(millis()-startime<500)
                                            {
                                                
                                                  wheel1(-150);
                                                  wheel2(150);
                                               
                                             }
                                             else
                                            {
                                                  temp=1;
                                             }
                                }
                                else if((cm1 < 70)&& (cm2 < 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//1、2、3超声波  倒、左转
                                {   
                                            startime=millis();
                                            if(millis()-startime<500)
                                            {
                                               
                                                  wheel1(200);
                                                  wheel2(-200);
                                             
                                             }
                                             else
                                            {
                                                  temp=1;
                                             } 
                                  
                                }   
////////////////////////////////////
                               else if((cm1 < 70) && (cm2 > 70)&& (cm3 < 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  // 1、3、4超声波 正前方攻击
                               {
                                          
                                                wheel1(200);
                                                wheel2(200);
                                           
                                          
                                        
                               }
        
                               else if ((cm1 < 70)&& (cm2 > 70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//1、3、4超声波  左转
                                  
                              { 
                                           startime=millis();
                                           if(millis()-startime<500)
                                           {
                                                        
                                                  wheel1(150);
                                                  wheel2(-150);
                                                       
                                            }
                                            else
                                            {
                                                     temp=1;
                                             }
                                       
                                }
                                else if((cm1 < 70)&& (cm2 > 70)&& (cm3 <70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//1、3、4超声波 右转
                                { 
                                         startime=millis();
                                         if(millis()-startime<500)
                                         {
                                                        
                                                  wheel1(-150);
                                                  wheel2(150);
                                                       
                                          }
                                          else
                                          {
                                                  temp=1;
                                           }
                                 }
                                 else if((cm1 < 70)&& (cm2 > 70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//1、3、4超声波 倒、左转
                                 {   
                                          startime=millis();
                                          if(millis()-startime<500)
                                          {
                                                       
                                                    wheel1(200);
                                                    wheel2(-200);
                                                     
                                           }
                                           else
                                           {
                                           temp=1;
                                            } 
                                          
                                   }
/////////////////////////////////////////////////////////////
                                  else if((cm1 < 70) && (cm2 <70)&& (cm3 < 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  // 1、2、3、4超声波 正前方攻击
                                   {
                                              
                                                       
                                                   wheel1(200);
                                                   wheel2(200);
                                                     
                                              
                                                  
                                            
                                     }
                                          
                                     else if ((cm1 < 70)&& (cm2 <70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//1、2、3、4超声波 右转
                                  
                                     { 
                                                startime=millis();
                                                if(millis()-startime<500)
                                                {
                                                        
                                                      wheel1(150);
                                                      wheel2(-150);
                                                       
                                                 }
                                                 else
                                                 {
                                                       temp=1;
                                                  }
                                       
                                       }
                                       else if((cm1 < 70)&& (cm2 < 70)&& (cm3 <70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//1、2、3、4超声波 右转 
                                       { 
                                                    startime=millis();
                                                    if(millis()-startime<500)
                                                    {
                                                        
                                                          wheel1(-150);
                                                          wheel2(150);
                                                       
                                                     }
                                                     else
                                                    {
                                                          temp=1;
                                                     }
                                        }
                                        else if((cm1 < 70)&& (cm2 < 70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//1、2、3、4超声波 倒、左转
                                        {   
                                                   startime=millis();
                                                    if(millis()-startime<500)
                                                    {
                                                       
                                                          wheel1(200);
                                                          wheel2(-200);
                                                     
                                                     }
                                                     else
                                                    {
                                                          temp=1;
                                                     } 
                                          
                                          }
//**********************************************************当1超声波没感觉到距离，2超声波感应到距离时***********************************************************										  
                                         else if( (cm2 < 70) && (cm1 > 70)&& (cm3 > 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  // 2超声波 右转 
                                          {
                                                     wheel1(-150);
                                                     wheel2(150);
													 delay(700);//向右拐直角
                                                    
                                            
                                           }
                                          
                                           else if ((cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0) //2超声波 左转
                                  
                                           { 
                                                     startime=millis();
                                                     if(millis()-startime<500)
                                                     {
                                                        
                                                           wheel1(150);
                                                           wheel2(-150);
                                                       
                                                      }
                                                      else
                                                     {
                                                           temp=1;
                                                      }
                                       
                                            }
                                            else if((cm2 > 1) && (cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//2超声波 右转
                                           { 
                                                     startime=millis();
                                                     if(millis()-startime<500)
                                                     {
                                                        
                                                            wheel1(-150);
                                                            wheel2(150);
                                                       
                                                      }
                                                      else
                                                     {
                                                           temp=1;
                                                     }
                                            }
                                            else if((cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//2超声波 倒、左转
                                           {   
                                                     startime=millis();
                                                     if(millis()-startime<500)
                                                     {
                                                       
                                                              wheel1(200);
                                                              wheel2(-200);
                                                     
                                                      }
                                                      else
                                                     {
                                                              temp=1;
                                                     } 
        
                                           }
///////////////////////////////////////////////
                                          else if((cm2 < 70) && (cm1 > 70)&& (cm3 < 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  // 2、3、4超声波 右转
                                          {
                                                    wheel1(-150);
                                                    wheel2(150);
													delay(700);//向右拐直角
                  
          
                                           }
        
                                            else if ((cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//2、3、4超声波 左转 
                                            
                                            { 
                                                        startime=millis();
                                                        if(millis()-startime<500)
                                                        {
                                                                  
                                                                  wheel1(150);
                                                                  wheel2(-150);
                                                                 
                                                         }
                                                               else
                                                              {
                                                               temp=1;
                                                               }
                                                 
                                                }
                                                 else if((cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//2、3、4超声波 右转 
                                                { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                  wheel1(-150);
                                                                  wheel2(150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                               temp=1;
                                                               }
                                                 }
                                                 else if( (cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//2、3、4超声波 倒、左转
                                                 {   
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                    wheel1(200);
                                                                    wheel2(-200);
                                                               
                                                               }
                                                               else
                                                              {
                                                                    temp=1;
                                                               } 
                                                    
                                                   }
//////////////////////////////////////////////////////
                                                  else if((cm2 < 70) && (cm1 > 70)&& (cm3 < 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  // 2、3超声波 右转
                                                   {
                                                               wheel1(-150);
                                                               wheel2(150);
                                                               delay(700);//向右拐直角      
                                                      
                                                    }
                                                    
                                                    else if ((cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//2、3超声波 左转 
                                            
                                                    { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                    wheel1(150);
                                                                    wheel2(-150);
                                                                   
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               }
                                                 
                                                      }
                                                      else if((cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//2、3超声波 右转
                                                     { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                    wheel1(-150);
                                                                    wheel2(150);
                                                                  
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               }
                                                    }
                                                    else if((cm2 < 70)&& (cm1 > 70)&& (cm3 < 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//2、3超声波 倒、后转
                                                   {   
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                     wheel1(200);
                                                                     wheel2(-200);
                                                               
                                                               }
                                                               else
                                                              {
                                                                      temp=1;
                                                               } 
                                                    
                                                    }     
//////////////////////////////////////////////////////////////////////////////
                                                   else if( (cm2 < 70) && (cm1 > 70)&& (cm3 > 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  //2、4超声波 右转
                                                   {
                                                               wheel1(-150);
                                                               wheel2(150);
                                                               delay(700);//向右拐直角
                                                      
                                                    }
                                                    
                                                    else if ((cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//2、4超声波 左转 
                                            
                                                   { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                     wheel1(150);
                                                                     wheel2(-150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               }
                                                 
                                                     }
                                                      else if((cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//2、4超声波 右转
                                                     { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                   
                                                                     wheel1(-150);
                                                                     wheel2(150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               }
                                                      }
                                                      else if((cm2 > 1) && (cm2 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//2、4超声波 倒、后转
                                                     {   
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                     wheel1(200);
                                                                     wheel2(-200);
                                                               
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               } 
                                                    
                                                      }  
//**************************************************************当1、2超声波未感应到，3超声波感应到*****************************************************
                                                     else if((cm3 < 70) && (cm1 > 70)&& (cm2 > 70) && (cm4 < 70) && value1==0 && value2==0 && temp == 0)  // 3、4超声波 左转
                                                      {
                                                               wheel1(150);
                                                               wheel2(-150);
                                                              delay(700);//向右拐直角   
                                                      
                                                      }
                                                    
                                                      else if ( (cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 < 70)  && value1==1 && value2==0 &&temp ==0 )//3、4超声波 左转
                                            
                                                     { 
                                                               startime=millis();
                                                               if(millis()-startime<500)
                                                               {
                                                                  
                                                                      wheel1(150);
                                                                      wheel2(-150);
                                                                 
                                                                }
                                                                else
                                                               {
                                                                       temp=1;
                                                                }
                                                 
                                                      }
                                                      else if((cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 < 70)  && value1==0 && value2==1 &&temp==0)//3、4超声波 右转
                                                     { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                      wheel1(-150);
                                                                      wheel2(150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                                      temp=1;
                                                               }
                                                      }
                                                      else if((cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 < 70)  && value1==1 && value2==1 &&temp==0)//3、4超声波 倒、左转
                                                      {   
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                      wheel1(200);
                                                                      wheel2(-200);
                                                               
                                                               }
                                                               else
                                                              {
                                                                      temp=1;
                                                               } 
                                                    
                                                       }   
/////////////////////////////////////////////////////////////////////////
                                                     else  if((cm3 < 70) && (cm1 > 70)&& (cm2 > 70) && (cm4 > 70) && value1==0 && value2==0 && temp == 0)  //3超声波 左转
                                                       {
                                                               wheel1(150);
                                                               wheel2(-150);
                                                               delay(700);//向左拐直角
                                                      
                                                       }
															
														else if ((cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp ==0 )//3超声波 左转
													
														{ 
																	  startime=millis();
																	  if(millis()-startime<500)
																	  {
																		  
																			wheel1(150);
																			wheel2(-150);
																		 
																	   }
																	   else
																	  {
																			  temp=1;
																	   }
														 
														}
														 else if((cm3 > 1) && (cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//3超声波 右转 
														{ 
																	  startime=millis();
																	  if(millis()-startime<500)
																	  {
																		  
																			  wheel1(-150);
																			  wheel2(150);
																		 
																	   }
																	   else
																	  {
																			  temp=1;
																	   }
														  }
														  else if((cm3 < 70)&& (cm1 > 70)&& (cm2 > 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//3超声波 倒、左转
														  {   
																	  startime=millis();
																	  if(millis()-startime<500)
																	  {
																		 
																			  wheel1(200);
																			  wheel2(-200);
																	   
																	   }
																	   else
																	  {
																				temp=1;
																	   } 
															
															}
//*******************************************************当1、2、3超声波都未感应到，4超声波感应到*********************************************************															
                                              else if((cm4 < 70) && (cm1 > 70)&& (cm3 > 70) && (cm2 > 70) && value1==0 && value2==0 && temp == 0)  // 4超声波 掉头
                                                {
                                                               wheel1(150);
                                                               wheel2(-150);
															   delay(1400);//原地倒转180度
                                                                
                                                 }
                                                    
                                                else if ((cm4 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm2 > 70)  && value1==1 && value2==0 &&temp ==0 )//4超声波 左转
                                            
                                                { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                  
                                                                      wheel1(150);
                                                                      wheel2(-150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                                        temp=1;
                                                               }
                                                 
                                                }
                                                 else if((cm4 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm2 > 70)  && value1==0 && value2==1 &&temp==0)//4超声波  左转
                                                { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {       
                                                                  
                                                                     wheel1(-150);
                                                                      wheel2(150);
                                                                 
                                                               }
                                                               else
                                                              {
                                                                      temp=1;
                                                               }
                                                  }
                                                  else if((cm4 > 1) && (cm4 < 70)&& (cm1 > 70)&& (cm3 > 70) && (cm2 > 70)  && value1==1 && value2==1 &&temp==0)//4超声波 倒、左转
                                                  {   
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                     wheel1(200);
                                                                     wheel2(-200);
                                                               
                                                               }
                                                               else
                                                              {
                                                                     temp=1;
                                                               } 
                                                    
                                                    }
//**********************************************************当所有超声波都未感应到距离则正常行走********************************													
                                                 else  if ((cm1 > 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==0 && value2==0 &&temp==0)//前进
                                                           {
                                                                 
                                                              
                                                                     wheel1(100);
                                                                     wheel2(100);
                                                               
                                                               }
                                                               else
                                                              {
                                                                       temp=1;
                                                               } 
                                                            }
                                                              															  
                                                            else if ((cm1 > 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==0 &&temp==0)//左转 
                                                            { 
                                                                startime=millis();
                                                                if(millis()-startime<500)
                                                                {
                                                                 
                                                                      wheel1(150);
                                                                      wheel2(-150);
                                                               
                                                                }
                                                                else
                                                               {
                                                                       temp=1;
                                                                } 
                                                            }
                                                           else if ((cm1 > 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==0 && value2==1 &&temp==0)//右转 
                                                           { 
                                                              startime=millis();
                                                              if(millis()-startime<500)
                                                              {
                                                                 
                                                                     wheel1(-150);
                                                                     wheel2(150);
                                                               
                                                               }
                                                               else
                                                              {
                                                                      temp=1;
                                                               } 
                                                           }
                                                          else if((cm1 > 70)&& (cm2 > 70)&& (cm3 > 70) && (cm4 > 70)  && value1==1 && value2==1 &&temp==0)//倒、左转
                                                           {
                                                                startime=millis();
                                                                if(millis()-startime<500)
                                                               {
                                                                 
                                                                      wheel1(200);
                                                                      wheel2(-200);
                                                               
                                                                }
                                                               else
                                                              {
                                                                        temp=1;
                                                               } 
                                                            }
  }
                                            
                                            

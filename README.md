# Typhlo-safety-application
PROBLEM STATEMENT:
Technology now a days are skyrocketing the capable persons who can handle it wisely. We  forget the people who are naturally disabled and are struggling to make an impact in their social life. Without vision it can be challenging for the visually impaired to move through a daily routine.When we use the word blind we could imagine a pity person staying in the roadside to cross the road with someone’s assistance. These people are now always accompanied and we can see them struggling on day today events. Some scenarios are emergency and it is really hard to get help. So we are coming forward with an innovative idea to be used and utilised by visually challenged persons.This idea incorporates important functions that helps them in emergency and also helps to take preventive measures for the ailed. We hope to bring a product with full fledged application and also that could help us gain knowledge in the same domain so that it would bring us a bright carrier.
ABSTRACT
The project aims at providing a  very easy  and a feasible hand-held helping application for the visually challenged people in their day-to-day life. We encounter many people who are so much talented In spite of their illness. These people are mostly dependent for some reason on their friends or on their family. Not everyone accompanies them in all their situations. We hereby would like to help these people during their challenging situation. We hereby would like to create a three different solution modules for their three different situations. 
	The first module provides an emergency alert to the emergency contact number saved in a particular mobile phone used by the person. 
	The second module is a pill remainder , which would remind the person for taking a medicine at the particular time.  
 
INTRODUCTION
 The native android application is created in Java language.  The key point of  native apps is that they provide more security compared to hybrid apps. The native android applications are more efficient and compatible with all the android phones. This app is created to help the physically challenged people and elderly people. The expanding accessibility options included in the Google Android operating system, plus a wide array of affordable mobile devices that run the Android OS, have made the platform an increasingly popular choice for those looking for a Smartphone or tablet. Since Android is an open operating system, deployed by a number of manufacturers on their phones and tablets, buyers can choose from an array of hardware, without having to wonder whether the gadget they like best is accessible. In addition to the Talkback screen reader, Android's recent versions allow users with low vision to build their own accessible experiences using a combination of settings for changing the way the screen looks. A few vendors, including Samsung, have even added accessibility tools of their own to the stock Android environment
                                                      MODULES
 MESSAGE READER
•	MESSAGE LISTENER
•	RECEIVE MESSAGE
•	ALERT SYSTEM
•	TEXT TO SPEECH
MESSAGE LISTENER			
The message listener has started while the app starts. The listener is started to receive the new message. The listener runs in background to receive the new SMS. Message Reader detects the new message using a broadcast receiver (until this moment the app has not been wasting battery)
RECEIVE MESSAGE
This module receives new SMS when the receiver mobile receives SMS. This event has risen by the broadcast receiver module. This event finds a new SMS which is received in the inbox. 
ALERT SYSTEM
This alert system, alerts the user while the new SMS received. This alert system plays a sound whenever a new SMS received. 
TEXT TO SPEECH
Starts a background service with the TTS (Text To Speech) system, when all the work is done, Message Reader will start to speak the message. In this module the received text words that should be heard clearly with the correct pronunciation. For this voice generation the TTS library file called and the parameter is passed to that library. The parameter should be in the text format.
 EMERGENCY REQUESTER
The emergency requester module, works with the physical keys present in the mobile phone. This module generates an emergency help request with the location information. While the blind people needs help, the person have to press the power button for few times in their mobile phone, the user location is gathered using the GPS module, then the emergency help request sent to the contact as SMS with the location information of the blind person. 
 MEDICINE REMAINDER
PILL BOX
In this module the user can add medications for their illness based on the prescriptions given by the doctors. The pill box should be maintained with the schedule and time details. The patient can schedule the remainder for once or can every day in a week. 

TRIGGERING ALARM 
The alarm can be set for multiple medicines and timings including date, time and medicine description. While the current time reaches the scheduled time to take medicine, this system plays an alarm and intimate the patient to take medicine
PROJECT STATUS
EMERGENCY HELPER(WORKING SUCCESSFULLY)
MEDICINE REMAINDER(WORKING SUCCESSFULLY)
TEXT TO SPEECH 
PENDING WORK 
TEXT TO SPEECH
##MODULES
1.EMERGENCY HELPER(MADHUMITHA)
2.MEDICINE REMAINDER(MADHUMITHA)
3.TEXT TO SPEECH (SATHISH)



 







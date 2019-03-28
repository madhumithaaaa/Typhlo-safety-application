# Typhlo-safety-application
PROBLEM STATEMENT

Technology now a days are skyrocketing the capable persons who can handle it wisely. We  forget the people who are naturally disabled and are struggling to make an impact in their social life. Without vision it can be challenging for the visually impaired to move through a daily routine.When we use the word blind we could imagine a pity person staying in the roadside to cross the road with someone’s assistance. These people are now always accompanied and we can see them struggling on day today events. Some scenarios are emergency and it is really hard to get help. So we are coming forward with an innovative idea to be used and utilised by visually challenged persons.This idea incorporates important functions that helps them in emergency and also helps to take preventive measures for the ailed. We hope to bring a product with full fledged application and also that could help us gain knowledge in the same domain so that it would bring us a bright carrier.

ABSTRACT

The project aims at providing a  very easy  and a feasible hand-held helping application for the visually challenged people in their day-to-day life. We encounter many people who are so much talented In spite of their illness. These people are mostly dependent for some reason on their friends or on their family. Not everyone accompanies them in all their situations. We hereby would like to help these people during their challenging situation. We hereby would like to create a three different solution modules for their three different situations. 
The first module provides an emergency alert to the emergency contact number saved in a particular mobile phone used by the person. 	The second module is a pill remainder , which would remind the person for taking a medicine at the particular time.
The third module is text to speech
 
INTRODUCTION

 The native android application is created in Java language.  The key point of  native apps is that they provide more security compared to hybrid apps. The native android applications are more efficient and compatible with all the android phones. This app is created to help the physically challenged people and elderly people. The expanding accessibility options included in the Google Android operating system, plus a wide array of affordable mobile devices that run the Android OS, have made the platform an increasingly popular choice for those looking for a Smartphone or tablet. Since Android is an open operating system, deployed by a number of manufacturers on their phones and tablets, buyers can choose from an array of hardware, without having to wonder whether the gadget they like best is accessible. In addition to the Talkback screen reader, Android's recent versions allow users with low vision to build their own accessible experiences using a combination of settings for changing the way the screen looks. A few vendors, including Samsung, have even added accessibility tools of their own to the stock Android environment
 
                                                      MODULES
  MESSAGE READER
  
	MESSAGE LISTENER
  
	RECEIVE MESSAGE
  
	ALERT SYSTEM
  
	TEXT TO SPEECH
  
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

The alarm can be set for multiple medicines and timings including date, time and medicine description. While the current time reaches the scheduled time to take medicine, this system plays an alarm and intimate the patient to take medicine.

PROJECT STATUS

1.Emergency helper (working sucessfully)

2.Medicine remainder (working successfully)

3.text to speech

##MODULES

1.emergency helper (madhumitha)

2.Medicine remainder (madhumitha)

3.Text to speech (sathish)

##coding

package com.appsrox.remindme.model;

import java.text.SimpleDateFormat;
import java.util.Calendar; 
import java.util.Date;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import com.appsrox.remindme.RemindMe;
import com.appsrox.remindme.Util;

public class DbHelper extends SQLiteOpenHelper {
	
//	private static final String TAG = "DbHelper";

	public static final String DB_NAME = "remindme.db";
	public static final int DB_VERSION = 1;
	
	public static final SimpleDateFormat sdf = new SimpleDateFormat(RemindMe.DEFAULT_DATE_FORMAT);

	public DbHelper(Context context) {
		super(context, DB_NAME, null, DB_VERSION);
	}

	@Override
	public void onCreate(SQLiteDatabase db) {
		db.execSQL(Alarm.getSql());
		db.execSQL(AlarmTime.getSql());
		db.execSQL(AlarmMsg.getSql());
	}

	@Override
	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
		db.execSQL("DROP TABLE IF EXISTS " + Alarm.TABLE_NAME);
		db.execSQL("DROP TABLE IF EXISTS " + AlarmTime.TABLE_NAME);
		db.execSQL("DROP TABLE IF EXISTS " + AlarmMsg.TABLE_NAME);
		
		onCreate(db);		
	}
	
	public void shred(SQLiteDatabase db) {
		db.delete(AlarmMsg.TABLE_NAME, AlarmMsg.COL_STATUS+" = ?", new String[]{AlarmMsg.CANCELLED});
	}
	
	private final String populateSQL = Util.concat("SELECT ",
														"a."+Alarm.COL_FROMDATE+", ",
														"a."+Alarm.COL_TODATE+", ",
														"a."+Alarm.COL_RULE+", ",
														"a."+Alarm.COL_INTERVAL+", ",
														"at."+AlarmTime.COL_AT,
												" FROM "+Alarm.TABLE_NAME+" AS a",
												" JOIN "+AlarmTime.TABLE_NAME+" AS at",
												" ON a."+Alarm.COL_ID+" = at."+AlarmTime.COL_ALARMID,
												" WHERE a."+Alarm.COL_ID+" = ?"); 
	
	public void populate(long alarmId, SQLiteDatabase db) {

		String[] selectionArgs = {String.valueOf(alarmId)};
		Cursor c = db.rawQuery(populateSQL, selectionArgs);
		
		if (c.moveToFirst()) {			
			Calendar cal = Calendar.getInstance(); 			
			AlarmMsg alarmMsg = new AlarmMsg();
			long now = System.currentTimeMillis();
			db.beginTransaction();
	        try {
				do {
					Date fromDate = sdf.parse(c.getString(0)); //yyyy-M-d
					cal.setTime(fromDate);
					
					//at
					String[] tokens = c.getString(4).split(":"); //hh:mm
					cal.set(Calendar.HOUR_OF_DAY, Integer.parseInt(tokens[0]));
					cal.set(Calendar.MINUTE, Integer.parseInt(tokens[1]));
					cal.set(Calendar.SECOND, 0);
					cal.set(Calendar.MILLISECOND, 0);
					
					String rule = c.getString(2); //every day month
					String interval = c.getString(3); //mm hh dd MM yy
					
					if (rule == null && interval == null) {//one time
						alarmMsg.reset();
						alarmMsg.setAlarmId(alarmId);
						alarmMsg.setDateTime(cal.getTimeInMillis());
						if (alarmMsg.getDateTime() < now-Util.MIN)
							alarmMsg.setStatus(AlarmMsg.EXPIRED);
						alarmMsg.save(db);						
						
					} else {//repeating
						if (rule != null) {						
							tokens = rule.split(" ");
							interval = "0 0 1 0 0"; //date++;
							
							//Day
							if (!"0".equals(tokens[1])) {
								cal.set(Calendar.DAY_OF_WEEK, Integer.parseInt(tokens[1]));
								interval = "0 0 7 0 0"; //week++;
							}
							//Every
							if (!"0".equals(tokens[0]) && "0".equals(tokens[1])) {
								cal.set(Calendar.DATE, Integer.parseInt(tokens[0]));
								interval = "0 0 0 1 0"; //month++;
							}
							//Month
							if (!"0".equals(tokens[2])) {
								cal.set(Calendar.MONTH, Integer.parseInt(tokens[2])-1);
								interval = "0 0 0 0 1"; //year++;
							}

							while(cal.getTime().before(fromDate)) {
								nextDate(cal, interval);
							}
						}
						
						Date toDate = sdf.parse(c.getString(1));
						toDate.setHours(0);
						toDate.setMinutes(0);
						toDate.setSeconds(0);
						toDate.setDate(toDate.getDate()+1);
						while(cal.getTime().before(toDate)) {
							alarmMsg.reset();
							alarmMsg.setAlarmId(alarmId);
							alarmMsg.setDateTime(cal.getTimeInMillis());
							if (alarmMsg.getDateTime() < now-Util.MIN)
								alarmMsg.setStatus(AlarmMsg.EXPIRED);
							alarmMsg.save(db);
							nextDate(cal, interval);
						}
					}
					
				} while(c.moveToNext());
				
		        db.setTransactionSuccessful();
		    } catch (Exception e) {
//		    	Log.e(TAG, e.getMessage(), e);
		    } finally {
		    	db.endTransaction();
		    }
		}
		c.close();
	}
	
	private void nextDate(Calendar cal, String interval) {
		String[] tokens = interval.split(" ");
		cal.add(Calendar.MINUTE, Integer.parseInt(tokens[0]));
		cal.add(Calendar.HOUR_OF_DAY, Integer.parseInt(tokens[1]));
		cal.add(Calendar.DATE, Integer.parseInt(tokens[2]));
		cal.add(Calendar.MONTH, Integer.parseInt(tokens[3]));
		cal.add(Calendar.YEAR, Integer.parseInt(tokens[4]));
	}
	
	private final String listSQL = Util.concat("SELECT ",
													"a."+Alarm.COL_NAME+", ",
													"am."+AlarmMsg.COL_ID+", ",
													"am."+AlarmMsg.COL_DATETIME+", ",
													"am."+AlarmMsg.COL_STATUS,
											" FROM "+Alarm.TABLE_NAME+" AS a",
											" JOIN "+AlarmMsg.TABLE_NAME+" AS am",
											" ON a."+Alarm.COL_ID+" = am."+AlarmMsg.COL_ALARMID);	

	/**
	 * @param db
	 * @param args {startTime, endTime}
	 * @return cursor
	 */	
	public Cursor listNotifications(SQLiteDatabase db, String... args) {
		String selection = "am."+AlarmMsg.COL_STATUS+" != '"+AlarmMsg.CANCELLED+"'";
		selection += (args!=null && args.length>0 && args[0]!=null) ? " AND am."+AlarmMsg.COL_DATETIME+" >= "+args[0] : "";
		selection += (args!=null && args.length>1 && args[1]!=null) ? " AND am."+AlarmMsg.COL_DATETIME+" <= "+args[1] : "";		
		
		String sql = Util.concat(listSQL,
								" WHERE "+selection,
								" ORDER BY am."+AlarmMsg.COL_DATETIME+" ASC");

		return db.rawQuery(sql, null);		
	}
	
	public int cancelNotification(SQLiteDatabase db, long id, boolean isAlarmId) {
		ContentValues cv = new ContentValues();
		cv.put(AlarmMsg.COL_STATUS, AlarmMsg.CANCELLED);
		return db.update(AlarmMsg.TABLE_NAME, 
							cv, 
							(isAlarmId ? AlarmMsg.COL_ALARMID : AlarmMsg.COL_ID)+" = ?", 
							new String[]{String.valueOf(id)});
	}
	
	public int cancelNotification(SQLiteDatabase db, String startTime, String endTime) {
		ContentValues cv = new ContentValues();
		cv.put(AlarmMsg.COL_STATUS, AlarmMsg.CANCELLED);
		return db.update(AlarmMsg.TABLE_NAME, 
							cv, 
							AlarmMsg.COL_DATETIME+" >= ? AND "+AlarmMsg.COL_DATETIME+" <= ?", 
							new String[]{startTime, endTime});
	}	
	
	 public static final String getDateStr(int year, int month, int date) {
		 return Util.concat(year, "-", month, "-", date);
	 }
	 
	 public static final String getTimeStr(int hour, int minute) {
		 return Util.concat(hour, ":", minute>9 ? "":"0", minute);
	 }	 

}
CONCLUSION AND FUTURE ENHANCEMENT


The “Application for blind people” is a complex system involving many sub process. The system overcomes the limitation of existing manual system. This project has been designed, developed and implemented thus providing a full- fledged approach for proficient and best of results. The project satisfies each efficient user for saving his time and also helps him in clearing the providing help to the physically challenged peoples.
This project deals with the elements of the native technologies.
It enables the physically challenged and the elderly people to overcome their difficulties and live without much support from others.
It enables the well wishers of the user’s to know their location to ensure their security and safety.
The project monitoring services can be updated with necessary enhancements in the database. The system overheads the problem in the existing ones by capable of processing voluminous data in a user-friendly manner.
The persons, who are involved in working the task manually, have seen this project running and expressing satisfaction about the working procedures and the “conversion handling” incorporated in the project.
Future enhancements can be made such as issuing user id to the user, where by the user can use that as a reference which specifies all his previous performance, the project work us stopped at this satisfactory level, due to time constraints.
 






 







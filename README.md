# Java_Timezones


//adjusting offsets
TimeZone currentTimeZone = sc_.getTimeZone();
Calendar currentDt = new GregorianCalendar(currentTimeZone, EN_US_LOCALE);
// Get the Offset from GMT taking DST into account
int gmtOffset = currentTimeZone.getOffset(
    currentDt.get(Calendar.ERA), 
    currentDt.get(Calendar.YEAR), 
    currentDt.get(Calendar.MONTH), 
    currentDt.get(Calendar.DAY_OF_MONTH), 
    currentDt.get(Calendar.DAY_OF_WEEK), 
    currentDt.get(Calendar.MILLISECOND));
// convert to hours
gmtOffset = gmtOffset / (60*60*1000);
System.out.println("Current User's TimeZone: " + currentTimeZone.getID());
System.out.println("Current Offset from GMT (in hrs):" + gmtOffset);
// Get TS from User Input
Timestamp issuedDate = (Timestamp) getACPValue(inputs_, "issuedDate");
System.out.println("TS from ACP: " + issuedDate);
// Set TS into Calendar
Calendar issueDate = convertTimestampToJavaCalendar(issuedDate);
// Adjust for GMT (note the offset negation)
issueDate.add(Calendar.HOUR_OF_DAY, -gmtOffset);
System.out.println("Calendar Date converted from TS using GMT and US_EN Locale: "
    + DateFormat.getDateTimeInstance(DateFormat.SHORT, DateFormat.SHORT)
    .format(issueDate.getTime()));

//adjusting offsets
public static Calendar convertToGmt(Calendar cal) {

    Date date = cal.getTime();
    TimeZone tz = cal.getTimeZone();

    log.debug("input calendar has date [" + date + "]");

    //Returns the number of milliseconds since January 1, 1970, 00:00:00 GMT 
    long msFromEpochGmt = date.getTime();

    //gives you the current offset in ms from GMT at the current date
    int offsetFromUTC = tz.getOffset(msFromEpochGmt);
    log.debug("offset is " + offsetFromUTC);

    //create a new calendar in GMT timezone, set to this date and add the offset
    Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
    gmtCal.setTime(date);
    gmtCal.add(Calendar.MILLISECOND, offsetFromUTC);

    log.debug("Created GMT cal with date [" + gmtCal.getTime() + "]");

    return gmtCal;
}

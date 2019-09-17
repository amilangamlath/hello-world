# hello-world
Just Another Repository
package com.mbsl.velocity.risk.auth;

import com.google.gson.JsonObject;
import com.mbsl.velocity.risk.ml.Person;
import com.mbsl.velocity.risk.utill.DateFormatter;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

/*
author- Amila Jayarathna
 */
@Controller
public class MLProcessController {

    @RequestMapping(value = "/register", method = RequestMethod.POST)
    @ResponseBody
    public String postSubmit(@RequestBody Person dto, HttpServletRequest req, HttpServletResponse res) throws ParseException, Exception {

        HttpSession session = req.getSession();

        URL url = new URL("http://10.1.1.111:10010/web/CreateNewCustomer");
        HttpURLConnection con = (HttpURLConnection) url.openConnection();

        //add reuqest header
        con.setRequestMethod("POST");
        con.setRequestProperty("Content-Type", "application/json; utf-8");
        con.setRequestProperty("Accept", "application/json");

        con.setDoOutput(true);
        JsonObject person = new JsonObject();

        String innevName = (String) session.getAttribute("as400env");
        String userName = (String) session.getAttribute("user");
        String clientFullName = dto.getSname() + dto.getFname() + dto.getLname();
        String dob = dto.getDob();
        String moDate = dto.getDob();

        String snameArray[] = dto.getSname().split(" ");
        String snameInitial = "";
        for (String stemp : snameArray) {
            snameInitial = snameInitial + "." + stemp.charAt(0);
        }

        String fnameArray[] = dto.getFname().split(" ");
        String fnameInitial = "";
        for (String ftemp : fnameArray) {
            snameInitial = fnameInitial + "." + ftemp.charAt(0);
        }
        String shortName = (dto.getLname() + " " + snameInitial + "." + fnameInitial).toUpperCase();

        Date nDOBdate1 = new SimpleDateFormat("yyyy-MM-dd").parse(DateFormatter.getHtml5Date(dob, "yyyy-MM-dd", "yyyy-MM-dd"));
        String nDOBdate2 = new SimpleDateFormat("yyyyDDD").format(nDOBdate1);
        Date moDate1 = new SimpleDateFormat("yyyy-MM-dd").parse(DateFormatter.getHtml5Date(moDate, "yyyy-MM-dd", "yyyy-MM-dd"));
        String moDate2 = new SimpleDateFormat("yyyyDDD").format(moDate1);

        String cusType = dto.getCusTypeItemSelected().substring(0, 2);
        String mktSeg[] = dto.getMktItemSelected().split("-");
        String dmiConf = dto.getDmiItemSelected().substring(0, 2);
        String cusCls = dto.getCusClassSelected().substring(0, 2);
        String cusPref = dto.getPrefCusSelected().substring(0, 4);
        String source = dto.getSourceSelected().substring(0, 2);
        String branch[] = dto.getBrItemSelected().split("-");
        String sheluteState[] = dto.getSheluteItemSelected().split("-");
        String addOption = dto.getAddItemSelected().substring(0, 2);

        String status = dto.getStatItemSelected().substring(0, 2);

        person.addProperty("INENVNAME", innevName);
        person.addProperty("USERNAME", userName);
        person.addProperty("CUSTOMERSTATUS", sheluteState[0]);
        person.addProperty("ALTERNATEADDRESSOPTION", addOption);
        person.addProperty("CLIENTFULLNAME1", clientFullName);
        person.addProperty("NAMEADDRESSLINE1", dto.getAdd1());
        person.addProperty("NAMEADDRESSLINE2", dto.getAdd2());
        person.addProperty("NAMEADDRESSLINE3", dto.getAdd3());
        person.addProperty("NAMEADDRESSLINE4", dto.getAdd4());
        person.addProperty("NAMEADDRESSLINE5", dto.getAdd5());
        person.addProperty("NAMEADDRESSLINE6", dto.getAdd6());
        person.addProperty("ZIPCODE", dto.getZipCode());
        person.addProperty("ZIPCODESUFFIX1", 0);
        person.addProperty("ZIPCODESUFFIX2", " ");
        person.addProperty("ZIPCODESUFFIX3", " ");
        person.addProperty("SHORTNAME", shortName);
        person.addProperty("SOCIALSECURITYNUMBER", 0);
        person.addProperty("TAXIDNUMBERFLAG", " ");
        person.addProperty("CELLULARPHONENUMBER", dto.getPhn());
        person.addProperty("HOMEPHONENUMBER", 0);
        person.addProperty("BUSINESSPHONE", 0);
        person.addProperty("PRIMARYOFFICERNUMBER", dto.getOfficer());
        person.addProperty("SECONDARYOFFICERNUMBER", " ");
        person.addProperty("EXTRAOFFICERNUMBER1", " ");
        person.addProperty("EXTRAOFFICERNUMBER2", " ");
        person.addProperty("CUSTOMEROPENINGDATE", 70218);
        person.addProperty("ACCOUNTTYPE", " ");
        person.addProperty("CUSTOMERTYPE", 101);
        person.addProperty("SICCODE", 0);
        person.addProperty("SEX", dto.getGender());
        person.addProperty("RACE", " ");
        person.addProperty("OWNHOME", dto.getOwnhome());
        person.addProperty("YEAREMPLOYEDATCURRENTJOB", 0);
        person.addProperty("INCOMEINTHOUSANDS", 0);
        person.addProperty("PRIMARYSOURCEOFINCOME", " ");
        person.addProperty("DATEOFBIRTH", nDOBdate2);
        person.addProperty("NUMBEROFDEPENDENTS", dto.getDependent());
        person.addProperty("CONTACTPERSONBUSINESS", " ");
        person.addProperty("CONTACTTITLE", dto.getSheluteItemSelected());
        person.addProperty("USERDEFINEDIRAWHFLAG", " ");
        person.addProperty("HIGHESTMEMONUMBER", 0);
        person.addProperty("NATIONALIDNUMBER", dto.getNic());
        person.addProperty("USERKEYFIELD", " ");
        person.addProperty("CUSTOMERLINKAGEFIELD", " ");
        person.addProperty("USERFIELD3", " ");
        person.addProperty("DINERSCLUBHOLDERFLAG", " ");
        person.addProperty("DINERSCLUBCARDNUMBER", " ");
        person.addProperty("DCEXPIRATIONDATE", 0);
        person.addProperty("MASTERCARDHOLDERFLAG", " ");
        person.addProperty("MASTERCARDNUMBER", " ");
        person.addProperty("MCEXPIRATIONDATE", 0);
        person.addProperty("VISACARDHOLDERFLAG", " ");
        person.addProperty("VISACARDNUMBER", " ");
        person.addProperty("VISAEXPIRATIONDATE", 0);
        person.addProperty("ATMCARDHOLDERFLAG", " ");
        person.addProperty("ATMCARDNUMBER", " ");
        person.addProperty("ATMCARDEXPIRATIONDATE", 0);
        person.addProperty("LANGUAGECODE", " ");
        person.addProperty("CITIZENSHIPCODE", 0);
        person.addProperty("LEGALRESIDENCECODE", 0);
        person.addProperty("WITHHOLDINGPERCENTAGE", 0);
        person.addProperty("PASSPORTNUMBER", " ");
        person.addProperty("TAXIDNUMBER", dto.getTaxRegNo());
        person.addProperty("PROFESSIONCODE", 0);
        person.addProperty("INTERNALSHORTKEY", " ");
        person.addProperty("INTLDIALCODE", 0);
        person.addProperty("POSTALCODE", dto.getPostCode());
        person.addProperty("POSTCODEFLAG", " ");
        person.addProperty("ACCOMMODATIONCODE", " ");
        person.addProperty("BRANCHNUMBER", branch[0]);
        person.addProperty("MOVEDINDATE", moDate2);
        person.addProperty("MARITALSTATUS", status);
        person.addProperty("MAILINDICATOR", " ");
        person.addProperty("SOLICITABLECODE", "1");
        person.addProperty("SOCIOECONOMICGROUP", " ");
        person.addProperty("HOMEPHONEAVAIL", " ");
        person.addProperty("BUSPHONEAVAIL", " ");
        person.addProperty("PERSONALNONPERSONAL", cusType);
        person.addProperty("SALUTATION", dto.getSheluteItemSelected());
        person.addProperty("FACSIMILENUMBER", 0);
        person.addProperty("TELEXNUMBER", 0);
        person.addProperty("TELEXANSWERBACK", " ");
        person.addProperty("CUSTDOCFLAG", " ");
        person.addProperty("CUSTDOCACTIVITYDATE", 0);
        person.addProperty("TINCERTFLAG", " ");
        person.addProperty("TINACTIVITYDATE", 0);
        person.addProperty("NBROFCERTSSENT", 0);
        person.addProperty("CASHEXCLUSIONCODE", " ");
        person.addProperty("CASHEXCLUSIONLIMIT", 0);
        person.addProperty("CUSTOMEREXTRACTFLAG", " ");
        person.addProperty("DATELASTMAINTAINED", 2018038);
        person.addProperty("CURRCOUNTRYCODE", 0);
        person.addProperty("MARKETSEQMENT", mktSeg[0]);
        person.addProperty("EMPLOYEECODE", " ");
        person.addProperty("INQUIRYLEVEL", " ");
        person.addProperty("MAINTENANCELEVEL", " ");
        person.addProperty("LOCATIONCODE", 0);
        person.addProperty("LASTCONTACTDATE", 0);
        person.addProperty("DATEOFDEATH", 0);
        person.addProperty("SOURCEOFDATA", source);
        person.addProperty("CUSTOMERCLASSIFICATION", cusCls);
        person.addProperty("LEVELOFACTIVITY", " ");
        person.addProperty("MOVEDINDATECONFIRMATION", dmiConf);
        person.addProperty("PRINCIPALTELEPHONEEXTENTIONNUMBER", 0);
        person.addProperty("EMPLOYERTELEPHONEEXTENTIONNUMBER", 0);
        person.addProperty("BEHAVIOURALTYPE", " ");
        person.addProperty("LIFESTAGE", " ");
        person.addProperty("RESPONSIVENESS", " ");
        person.addProperty("CONTACTPREFERENCENOTES", " ");
        person.addProperty("CUSTOMERALIASNAME", " ");
        person.addProperty("CUSTOMEREMPLOYMENTSTATUS", " ");
        person.addProperty("HOMEEMAILADDRESS", " ");
        person.addProperty("BUSINESSEMAILADDRESS", " ");
        person.addProperty("ACCDESC", 0);
        person.addProperty("PARSEDADDRESSPRESENT", " ");
        person.addProperty("PREFERREDCUSTOMERCODE", cusPref);
        person.addProperty("MOVINGINDATEFORMAT", 0);
        person.addProperty("TITLE", " ");
        person.addProperty("FIRSTNAME", " ");
        person.addProperty("SECONDNAME", " ");
        person.addProperty("SURNAME", dto.getSname());
        person.addProperty("CURRBUSNAME", " ");
        person.addProperty("CURRAPTNUMBER", " ");
        person.addProperty("CURRHOUSENAME", " ");
        person.addProperty("CURRHOUSENBR", " ");
        person.addProperty("CURRSTREET", " ");
        person.addProperty("CURRDISTRICT", " ");
        person.addProperty("CURRPOSTTOWN", " ");
        person.addProperty("CURRCOUNTY", " ");
        person.addProperty("CURRPOSTCODE", " ");
        person.addProperty("CURRPOSTCODEFLAG", " ");
        person.addProperty("CURRCOMPLIMENT", " ");
        person.addProperty("CURRZIPCODE", 0);
        person.addProperty("CURRZIPCODESUFFIX", 0);
        person.addProperty("ZIPCODEROUTENUMBER", " ");
        person.addProperty("ZIPCODECHECKDIGIT", " ");
        person.addProperty("CURRSTATEABBREV", " ");
        person.addProperty("CURRCOUNTRY1", " ");
        person.addProperty("ALIASTITLE", " ");
        person.addProperty("ALIASFIRSTNAME", " ");
        person.addProperty("ALIASSECONDNAME", " ");
        person.addProperty("ALIASLASTNAME", " ");
        person.addProperty("ALIASCOMPLEMENT", " ");
        person.addProperty("EXTENSIONCODE", " ");
        person.addProperty("COUNTRYOFBIRTH", 0);

        String jsonInputString = person.toString();

        try (OutputStream os = con.getOutputStream()) {
            byte[] input = jsonInputString.getBytes("utf-8");
            os.write(input, 0, input.length);
        }

        String resp = "";
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(con.getInputStream(), "utf-8"))) {
            StringBuilder response = new StringBuilder();
            String responseLine = null;
            while ((responseLine = br.readLine()) != null) {
                response.append(responseLine.trim());
            }
            System.out.println(response.toString());
            resp = response.toString();
        }

        return "Amila Nandun******";
    }

}

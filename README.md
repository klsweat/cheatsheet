# cheatsheet
#SEQUELIZE:
Create Models: 
- sequelize model:create --name Todo --attributes title:string,description:string





#PHP

$var = $link->real_escape_string($_POST['DESCRIPTION']); 

$varConverted = htmlspecialchars($var, ENT_QUOTES, 'UTF-8');
#


#LDAP PHP | user attribute

```PHP
// $ds is a valid connexion id (samAccountName)
$filterall='(&(objectCategory=person)(employeeid=*)(sAMAccountName='.$_SESSION["username"].'))';
$justthese = array("displayname","department" , "title",  "whenCreated","employeeid"); 
$result = ldap_search($ds,$LDAPContainer , $filterall ,$justthese) or die ("Search failed");

$info = ldap_get_entries($ds, $result);
for ($i=0; $i < $info["count"]; $i++) { 
echo "Name: ".$info[$i]["displayname"][0]."<br>\n"; 
echo "Department: ".$info[$i]["department"][0]."<br>\n";
echo "Title: ".$info[$i]["title"][0]."<br>\n"; 
echo "EmployeeID: ".$info[$i]["employeeid"][0]."<br>\n";

$date = $info[$i]["whencreated"][0];
// Get the date segments by splitting up the LDAP date
$year = substr($date,0,4);
$month = substr($date,4,2);
$day = substr($date,6,2);
$hour = substr($date,8,2);
$minute = substr($date,10,2);
$second = substr($date,12,2);

// Make the Unix timestamp from the individual parts
$timestamp = mktime($hour, $minute, $second, $month, $day, $year);

// Output the finished timestamp
print "Date Created : ".$month."/".$day."/".$year. "\n";


echo "<div class='archievebox'><a href=''>View Details</a></div>";
echo "<hr>"; 

} ```





#[list all users in a table with info]

```PHP
$search_filter = "(&(objectCategory=person))";
$result = ldap_search($ds, $LDAPContainer, $search_filter);
if (FALSE !== $result){
  $entries = ldap_get_entries($ds, $result);
  if ($entries['count'] > 0){
      $odd = 0;
      foreach ($entries[0] AS $key => $value){
          if (0 === $odd%2){
              $ldap_columns[] = $key;
          }
          $odd++;
      }
      echo '<table class="data">';
      echo '<tr>';
      $header_count = 0;
      foreach ($ldap_columns AS $col_name){
          if (0 === $header_count++){
              echo '<th class="ul">';
          }else if (count($ldap_columns) === $header_count){
              echo '<th class="ur">';
          }else{
              echo '<th class="u">';
          }
          echo $col_name .'</th>';
      }
      echo '</tr>';
      for ($i = 0; $i < $entries['count']; $i++){
          echo '<tr>';
          $td_count = 0;
          foreach ($ldap_columns AS $col_name){
              if (0 === $td_count++){
                  echo '<td class="l">';
              }else{
                  echo '<td>';
              }
              if (isset($entries[$i][$col_name])){
                  $output = NULL;
                  if ('lastlogon' === $col_name || 'lastlogontimestamp' === $col_name){
                      $output = date('D M d, Y @ H:i:s', ($entries[$i][$col_name][0] / 10000000) - 11676009600);
                  }else{
                      $output = $entries[$i][$col_name][0];
                  }
                  echo $output .'</td>';
              }
          }
          echo '</tr>';
      }
      echo '</table>';
  }
}
```




#### MAKE A AJAX REQUEST AND RUN MULTIPLE MSSQL QUERIES, AND RETURN RESULTS IN ARRAY
```
$sql="";
$result = mssql_query($sql);
while($row=mssql_fetch_array($result))
  {
    $var = $row['sql_var'];
    $array[] = '<option value="'.$var.'">'.$var.'</option>';
  }
  
  $arr = array();
  
  $arr[1] = $array; 
  
  echo json_encode($arr);
  exit;
  ```
  ---

AJAX REQUEST
------
__AJAX CALL ON INDEX.PHP FILE__.
__RETURN IN JSON FORMAT__.
__RENDER TO SELECT MENU__.

```
$.ajax({
  url: '../',
  data: dataString,
  type: 'post',
  dataType: "json"
}).done(function(responseData) {
  console.log('Done: ', responseData);
  $("#select_id").html(responseData).selectmenu('refresh', true);
}).fail(function() {
  console.log('Failed');
});

```











GIT CHANGE LOG
-----------------

git log --format="%ad|%s|%an" | ForEach-Object {
  New-Object PSObject -Property @{
    Time = $_.Split('|')[0]
    Message = $_.Split('|')[1]
    Author = $_.Split('|')[2]
  }
} | Format-Table -Property @{Expression={$_.Time};width=25;Label="Author date"}, 
                           @{Expression={$_.Message};Width=25;Label="Commit message"}, 
                           @{Expression={$_.Author};Width=11;Label="Author name"}



## HTML nest dynamic dropdowns Javascript 
 
 ```angular2html
<label><h3 style="height:10px;">TURNBACK CATEGORY:</h3></label> 
<select size="1" id="turnback_category" title="" name="turnback_category" >
    <option selected value=''>-- SELECT --</option>
    <option class="documentation"  value='Documentation'>Documentation</option>
    <option class="Quality" value='Quality'>Quality</option>
</select>

<div class="container" style="margin-top:0px !important;">
    <!--DOCUMENTATION-->
    <div class="Documentation">
        <label><h3 style="height: 10px;">TURNBACK TYPE:</h3></label> 
        <select name='turnback_type_doc' id="turnback_type_doc" data-theme="f" data-native-menu="true" >    
            <option selected value=''>-- SELECT --</option>
            <option class="documentation" id="print" value='Print'>Print</option>
            <option class="documentation" id="Inspection" value='Inspection'>Inspection</option>
        </select> 

        <label><h3 style="height: 10px;">TURNBACK ISSUE:</h3></label> 
        <select name='turnback_issue_doc' id="turnback_issue_doc" data-theme="f" data-native-menu="true" >  
            <option selected value=''>-- SELECT --</option>
            <option id="print" value='Incorrect'>Incorrect</option>
            <option id="print" value='Missing'>Missing</option>  
        </select>     

    </div><!--/documentation-->

    <!--QUALITY-->
    <div class="Quality">
        <label><h3 style="height: 10px;">TURNBACK TYPE:</h3></label> 
        <select name='turnback_type_qualtiy' id="turnback_type_qualtiy" data-theme="f" data-native-menu="true" >
            <option selected value=''>-- SELECT --</option>   
            <option class="Quality" id="Gaging" value='Gaging'>Gaging</option>
            <option class="Quality" id="cmm" value='CMM/Comparator'>CMM/Comparator</option>
            <option class="Quality"  id="no_staff" value='No Staff'>No Staff</option>  
        </select> 

        <div class="quality_hide">
            <label><h3 style="height: 10px;">TURNBACK ISSUE:</h3></label> 
            <select name='turnback_issue' id="turnback_issue" data-theme="f" data-native-menu="true" >
                <option selected value=''> -- SELECT --</option>
                <option  value=''>You must select a Turnback Type before selecting a Turnback Issue</option>            
            </select>
            
            <div class="container_quality">
                <div class="Gaging">
                    <label><h3 style="height: 10px;">TURNBACK ISSUE:</h3></label> 
                    <select name='turnback_issue_Gaging' id="turnback_issue_Gaging" data-theme="f" data-native-menu="true" >
                        <option selected value=''>-- SELECT --       </option>
                        <option  id="Gaging" value='Missing'>Missing</option>
                        <option id="Gaging" value='Incorrect'>Incorrect</option>
                    </select> 
                </div><!--/Gaging-->
            
                <div class="cmm">
                    <label><h3 style="height: 10px;">TURNBACK ISSUE:</h3></label> 
                    <select name='turnback_issue_CMM' id="turnback_issue_CMM" data-theme="f" data-native-menu="true" >
                        <option value='In Use'>In Use</option>
                    </select> 
                </div><!--/CMM/Comparator-->
            </div><!--/container_quality-->
    </div>
</div> <!-- /container -->
```
 
  ```angular2html
$(document).ready(function() {
    $('#turnback_type_qualtiy').bind('change', function() {
        var elements = $('div.container_quality').children().hide(); // hide all the elements
            var value = $(this).val();
            var selectVal = $("option:selected", this).attr("id");
            
            if (value.length) { // if somethings' selected
                elements.filter('.' + selectVal).show(); // show the ones we want
                $('.quality_hide').hide();

            }
    }).trigger('change');
    
    $('#turnback_type_qualtiy').bind('change', function() {
        var elements = $('div.container_quality').children().hide(); // hide all the elements
        var value = $(this).val();
        var selectVal = $("option:selected", this).attr("id");
    
        if (value.length) { // if somethings' selected
            elements.filter('.' + selectVal).show(); // show the ones we want
            $('.quality_hide').hide();
        }
    }).trigger('change');
})
```  


javascript
 uniqueArray = jtArray.filter((obj, index) => {
          return jtArray.map(obj => obj['title']).indexOf(obj['title']) === index;
        }); 



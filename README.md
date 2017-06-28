# cheatsheet
#PHP

$var = $link->real_escape_string($_POST['DESCRIPTION']); 

$varConverted = htmlspecialchars($var, ENT_QUOTES, 'UTF-8');
#


#LDAP PHP | user attribute
#
```// $ds is a valid connexion id (samAccountName)
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
#
#
#list all users in a table with info
#
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




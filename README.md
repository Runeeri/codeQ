สูตรรันเลขคิวอัตโนมัติ
={"ลำดับคิว";ARRAYFORMULA ("Q" & text (ROW (B1:INDEX (B:B, COUNTA (B:B)-1)), "000"))}
------------
โค้ด.gs
-------------
  var data
  var ss = SpreadsheetApp.openById('.....')
  var sheet = ss.getSheetByName('.....')
  var range = sheet.getDataRange().getDisplayValues()
function doGet(e) {
  var qid = e.parameter.qid
  var html = HtmlService.createTemplateFromFile('index')
  data = { result: 'result', range: range }
  html.data = JSON.stringify(data)
  return html.evaluate()
}
function sendNotify(queue) {
  var result = ''
  var msg = ''
  var token = '..ใสtokenLine..'
  for (var i = 1; i < range.length; i++) {
    if (range[i][1] == queue) {
      msg = '📣 ขอเชิญคิว..ลำดับที่ : '+range[i][3]+' คุณ: '+range[i][1] + ' 👆 ️เข้ารับบริการค่ะ'
      var options = {
        "method": "post",
        "payload": "message=" + msg,
        "headers": {
          "Authorization": "Bearer " + token
        }
      };
      UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
      result = 'success'
    } else {
      result = 'fail'
    }
  }
  return result
}
-------------
html
-------------
<!DOCTYPE html>
<html>
  <head>
    <base target="_top" />
    <meta charset="utf-8" />
    <meta  name="viewport"content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    <!-- Bootstrap CSS -->
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
      integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2"
      crossorigin="anonymous"
    />
    <script
      src="https://code.jquery.com/jquery-3.5.1.min.js"
      integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0="
      crossorigin="anonymous"
    ></script>
  </head>
  
  <body>
    <nav class="navbar navbar-dark bg-primary">
      <span class="navbar-brand mb-0 h1">............ใส่ชื่อร้าน............."</span>
      <img src="........ใส่ลิ้งค์รูป.........." width="60" height="60" alt="" loading="lazy">
    </nav>
    <div class="container-fluid">
      <table class="table" table-hover>
        <thead>
          <tr>
            <th>ประทับเวลา</th>
            <th>ชื่อ</th>
            <th>เบอร์โทร</th>
            <th>ลำดับคิว</th>
          </tr>
        </thead>
        <tbody>
        <? ?>
        </tbody>
      </table>
    </div>
    <script>
    var DATA
     window.onload = () => {
      DATA = '<?!= data ?>'
      DATA = JSON.parse(DATA)
      DATA.range.shift()
      //console.log(DATA)
          DATA.range.forEach((row,i) =>{
            $("table tbody").append(`<tr id="TR${i}"></tr>`);
          $("#TR" + i).append(`<td>${DATA.range[i][0]}</td>`);
          $("#TR" + i).append(`<td>${DATA.range[i][1]}</td>`);
          $("#TR" + i).append(`<td>${DATA.range[i][2]}</td>`);
          $("#TR" + i).append(`<td>${DATA.range[i][3]}</td>`);
          $("#TR" + i).append(`<td><button id="buttonQ" onclick='this.style.backgroundColor = "#03fc2c";google.script.run.withSuccessHandler(onSuccess).sendNotify("${DATA.range[i][1]}")'>เรียกคิว</button></td>`);
         })
      
      };
      function onSuccess(result){
        //console.log(result)

      }
    </script>
  </body>
</html>
-------------
โค้ด.gs
ลบข้อความ
-------------
function Clear() {
var Data = SpreadsheetApp.getActive().getSheetByName('Sheet1');
 Data.getRange('A2:D17').clearContent();

 SpreadsheetApp.getActiveSpreadsheet().toast("","ลบข้อมูลแล้ว");
}

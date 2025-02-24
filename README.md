<html lang="ko">
<head>
<title>무작위 번호 추출기 (Random Number Generator)</title>
<style>
@charset "utf-8";
@import url('//fonts.googleapis.com/css2?family=Noto+Sans+KR&family=Nanum+Gothic+Coding&family=Nanum+Pen+Script');
@import url('//cdn.jsdelivr.net/gh/joungkyun/font-d2coding/d2coding.css');
@font-face {
    font-family: 'TheJamsil5Bold';
    src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/noonfonts_2302_01@1.0/TheJamsil5Bold.woff2') format('woff2');
    font-weight: 700;
    font-style: normal;
}
body {
  font: normal normal normal large/1.3em 'Noto Sans KR',sans-serif;
  text-shadow:3px 3px 4px lightgray;
}
.mzheader {
  padding:0px 1em 0px 1em;
  background-color:#f0f0ff;
  font: normal normal normal large/1.8em 'Noto Sans KR',sans-serif;
  text-shadow:3px 3px 4px cyan;
  border:3px solid #e0e0ff; 
  border-radius: 3px;
  word-wrap:break-word;
}
a { text-decoration:none; }
.mztrailer {
 font: normal normal normal large/1.5em 'Nanum Gothic Coding','Noto Sans KR',sans-serif;
 text-shadow:3px 3px 4px cyan;
 word-wrap:break-word;
}
#console {
  margin:1em auto 1em auto; 
  padding:30px 30px 30px 30px;
  border:20px solid #e7e7e7;
  border-left:0.5em solid #e7e7e7;
  border-right:0.5em solid #e7e7e7;
  border-radius: 30px;
  color:black;
  /*background:black url("/background.png") center/cover no-repeat;*/
  background:#f0f0f0 none center/cover no-repeat;
  /*opacity:0.85;*/
  font-family: 'TheJamsil5Bold';
  font-weight: 700;
  font-style: normal;
  font-size: xx-large;
  /*text-shadow: 0px 0px 4px #ffff0f;*/
  word-wrap:break-word;
  word-break:keep-all;
  text-align: justify;
}
#console span {
  display: inline-block;
  margin: 0 10px; /* 숫자 간격 조정 */
  text-align: justify;
}
#rtime {
  font-size: small;
  text-shadow:1px 1px 1px lightgray;
}
h1 {
  font-weight: bold;
  font-size: xx-large;
  font-family: 'TheJamsil5Bold';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff') format('woff');

}
input[type="number"] {
  width: 5em;
  font-weight: bold;
  font-size: large;
  border:3px solid #0366d6;
  border-radius: 10px;
  text-align: right;
  
}
input[type="submit"] {
  width: 5em;        
  font-weight: bold;
  font-size: large;
  background-color: #0366d6;
  color: #fff;
  border: 1px solid #0366d6;
  border-radius: 10px;
}
* {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

.buttons {
    margin: 10%;
    text-align: center;
}

.btn-hover {
    width: 100%;
    font-size: x-large;
    font-weight: 700;
    color: #fff;
    cursor: pointer;
    margin: 0px;
    height: 50px;
    text-align:center;
    border: none;
    background-size: 400% 100%;

    border-radius: 20px;
    moz-transition: all .4s ease-in-out;
    -o-transition: all .4s ease-in-out;
    -webkit-transition: all .4s ease-in-out;
    transition: all .4s ease-in-out;
}

.btn-hover:hover {
    background-position: 100% 0;
    moz-transition: all .4s ease-in-out;
    -o-transition: all .4s ease-in-out;
    -webkit-transition: all .4s ease-in-out;
    transition: all .4s ease-in-out;
}

.btn-hover:focus {
    outline: none;
}
.btn-hover.color-9 {
    background-image: linear-gradient(to right, #25aae1, #4481eb, #04befe, #3f86ed);
    box-shadow: 0 4px 15px 0 rgba(65, 132, 234, 0.75);
}  
</style>
<script>
window.addEventListener('DOMContentLoaded', function () {
  document.getElementById("calcButton").addEventListener("click", drawing);
  function leftPad2(value) {
    if (value >= 10) { return value; }
    return "0" + value;
  }
  function drawing(event) {
    event.preventDefault();
    let startNUM = parseInt(document.getElementById("startNUM").value);
    let endNUM = parseInt(document.getElementById("endNUM").value);
    let selectNum = parseInt(document.getElementById("selectNum").value);
    let numArray = new Array;
    let randomNum;
    let overlappingFlag;
    if(isNaN(selectNum)) { selectNum = 1; document.getElementById("selectNum").value = selectNum; alert("추출 숫자의 갯수를 이해할 수 없어서 1개로 가정하고 진행하겠습니다."); }
    if(isNaN(startNUM)) { startNUM = 1; document.getElementById("startNUM").value = startNUM; alert("시작 값을 이해할 수 없어서 1로 가정하고 진행하겠습니다."); }
    if(isNaN(endNUM)) { endNUM = startNUM + selectNum + 45; document.getElementById("endNUM").value = endNUM; alert("끝 값을 이해할 수 없어서 대충 " + endNUM + "(으)로 가정하고 진행하겠습니다."); }
    if(startNUM > endNUM) {
      alert("숫자 범위가 뒤바뀐 듯 합니다. (It seems that the range of numbers has been reversed.)");
      let tempNum = startNUM;
      startNUM = endNUM;
      endNUM = tempNum;
      document.getElementById("startNUM").value = startNUM; 
      document.getElementById("endNUM").value = endNUM;
    }
    let rangeNum = endNUM + 1 - startNUM;
    if(rangeNum < selectNum) {
      alert("입력된 숫자범위의 경우의 수보다 추출 숫자의 갯수가 커서 추출 숫자의 갯수를 " + rangeNum + "(으)로 제한합니다. (The number of extracted numbers is larger than the number in the case of the input number range, so the number of extracted numbers is limited to " + rangeNum + ".)");
      selectNum = rangeNum;
      document.getElementById("selectNum").value = selectNum;
    }
    if(selectNum <= 0) { alert("추출 숫자의 갯수가 충분하지 않습니다. 1이상의 값을 입력해주세요. (Insufficient number of extraction numbers. Please enter a value of 1 or more.)"); }
    else if(selectNum > 4096) { alert("추출 숫자의 갯수가 너무 큰 경우 시간이 좀 걸릴 수 있습니다. 기다리시면 언젠가는 결과가 나올겁니다. (It may take some time if the number of extraction numbers is too large. Just wait, the results will come out someday.)"); }
    while(selectNum > 0) {
      randomNum = Math.floor(Math.random(1) * (endNUM + 1 - startNUM)) + startNUM;                        
      overlappingFlag = false;
      for(let a in numArray) { if(numArray[a] == randomNum) { overlappingFlag = true; break; } }
      if(!overlappingFlag) { numArray.push(randomNum); selectNum--; }
    }
    let d = new Date();
    let nowLocaleString = d.toLocaleString();
    let nowString = d.toString();
    let nowISOString = d.toISOString();
    let nowSimple = '' + d.getFullYear() + leftPad2(d.getMonth()+1) + leftPad2(d.getDate()) + leftPad2(d.getHours()) + leftPad2(d.getMinutes()) + leftPad2(d.getSeconds());
    numArray.sort(function (left, right) { return left - right; });
    document.getElementById("console").innerHTML = "★★ 행운의 숫자 ★★<br/><br/>" + numArray.join("&nbsp;&nbsp; &nbsp;&nbsp;") + "<br/><br/><br/><br/><div id=\"rtime\">&gt; 추첨시각 : " + nowLocaleString  + "<br/>" + "&nbsp;&nbsp;[ " + nowString + " ]</div><br/>" + "<form name=\"download\"><input type=\"submit\" id=\"download\" value=\"다운로드\" title=\"누르면 추첨 결과를 텍스트 형식의 파일로 다운로드합니다. (Download)\" /></form>";
    document.getElementById("download").addEventListener("click", function (event) {
      event.preventDefault();
      let contents = "# Generated by MINZKN.COM\r\n# " + nowLocaleString + " [" + nowString + "]"+ "\r\n" + numArray.join(",") + "\r\n";
      let encodedUri = encodeURI("data:attachment/text;charset=utf-8,") + encodeURIComponent(contents);
      let hiddenElement = document.createElement("a");
      hiddenElement.setAttribute("href", encodedUri);
      hiddenElement.target="_blank";
      hiddenElement.setAttribute("download", "numbers-" + nowSimple + ".txt");
      document.body.appendChild(hiddenElement);
      hiddenElement.click();
      document.body.removeChild(hiddenElement);
    });
  }
});
</script>
</head>
<body>
  <h1><a href="/random.html" target="_top" title="무작위 번호 추출기 (Random Number Generator) ">[2025년 메타버스산업 통합 사업설명회] </a></h1>
  <p title="Randomizes non-overlapping numbers (integers) within the range of numbers entered below."><b>아래 입력된 범위에서 무작위 번호를 추출합니다.</b></p>
  <form name="randomNumber">
    <p><span title="숫자 범위 (Range)">- &nbsp;추첨 범위</span>: &nbsp;<input type="number" size="6" id="startNUM" value="1" title="시작 값 (Begin)"/> ~ <input type="number" size="6" id="endNUM" value="100" title="끝 값 (End)" /></p>
    <p><span title="추출 숫자의 갯수 (Count)">- &nbsp;당첨 인원</span>: &nbsp;<input type="number" size="6" id="selectNum" value="10" title="추출 숫자가 너무 큰 경우 오래 기다려야 할 수 있습니다." />&nbsp;&nbsp;&nbsp;&nbsp;
    </p>
    <div class="calcButton"><button class="btn-hover color-9" id="calcButton">추첨시작</button>
</div>
  </form>
  <div id="console" title="추첨하기를 누르면 여기에 결과가 나와요. (Press the button to see the result.)">★★ 추첨결과 ★★</div>

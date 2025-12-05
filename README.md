<html lang="ko">
<head>
  
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무작위 번호 추출기</title>
    <link rel="stylesheet" href="styles.css">
  
<title>번호추첨기</title>
<style>
  
  h1 {
  display: none; /* 제목을 숨깁니다 */
}
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
  font: normal normal normal large/1.3em 'TheJamsil5Bold'; color: white;
  text-shadow:1px 1px 15px lightgray;
  background-image :url('https://images.unsplash.com/photo-1614850523459-c2f4c699c52e?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D');
  background-size: 100% 100%; 
  background-repeat: no-repeat; 
  background-position: left top; 
  background-attachment: fixed;
  
    
}
.bg-video {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  z-index: -1;
  opacity: 0.5;
  overflow: hidden;  
  
}

.bg-video__content {
  height: 100vh;
  width: 100vw;
  position: fixed;
  //background-size: cover;
  object-fit: cover; // background-size: cover 와 비슷함. (HTML 요소 or 비디오와 작동)
  
  
}
.mzheader {
  padding:0px 1em 0px 1em;
  background-color:#f0f0ff;
  font: normal normal normal large/1.8em 'TheJamsil5Bold'; color: white;
  text-shadow:3px 3px 4px white;
  border:3px solid #e0e0ff; 
  border-radius: 3px;
  word-wrap:break-word;
}
a { text-decoration:none; }
.mztrailer {
 font: normal normal normal large/1.5em 'TheJamsil5Bold'; color: white;
 text-shadow:3px 3px 4px white;
 word-wrap:break-word;
}
#console {
  margin:1em auto 1em auto; 
  padding:30px 30px 30px 30px;
  border:20px solid #e7e7e7;
  border-top:20px solid #e7e7e7;
  border-left:20px solid #e7e7e7;
  border-right:20px solid #e7e7e7;
  border-bottom:20px solid #e7e7e7;
  border-radius: 30px;
  color:black;
  /*background:black url("/background.png") center/cover no-repeat;*/
  background:#ffffff none center/cover no-repeat;
  opacity:0.75;
  font-family: 'TheJamsil5Bold';
  font-weight: 700;
  font-style: normal;
  font-size: xx-large;
  /*text-shadow: 0px 0px 4px #ffff0f;*/
  word-wrap:break-word;
  word-break:keep-all;
  text-align: left;
}
#console span {
  display: inline-block;
  margin: 0 10px; /* 숫자 간격 조정 */
  text-align: justify;
  font-size: 0; /* 초기 크기 */
  opacity: 0; /* 초기 투명도 */
}
#rtime {
  font-size: small;
  text-shadow:1px 1px 1px lightgray;
}
h2 {
  font-weight: bold; 
  font-size: 2em;
  font-family: 'TheJamsil5Bold';
  color: white !important; /* 흰색으로 설정하며 우선 적용 */
}
input[type="number"] {
  width: 6em;
  font-weight: bold;
  font-size: large;
  font-family: 'TheJamsil5Bold';
  border:3px solid #3c89ee;
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
 
  function animateNumber(element, targetNumber) {
    let currentNumber = 0;
    element.style.opacity = 1; // 숫자 보이기
    const duration = 800; // 애니메이션 지속 시간 (1초)
    
    // 목표 숫자에 따라 최대 카운트업 값 설정
    const maxCount = Math.max(targetNumber, 99); // 100의 자리 이상으로 설정
    const stepTime = duration / maxCount; // 각 숫자 증가에 필요한 시간

    const interval = setInterval(() => {
        if (currentNumber < maxCount) {
            currentNumber++;
            element.textContent = currentNumber; // 숫자 증가
        } else {
            clearInterval(interval); // 증가 완료 후 정지
            element.textContent = targetNumber; // 최종 목표 값으로 설정
        }
    }, stepTime); // 각 숫자가 stepTime 간격으로 증가
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
  
    const resultHtml = "★★ 행운의 숫자 (A Lucky Number) ★★<br/><br/>" + 
      numArray.map((num) => `<span style="font-size: xxx-large; opacity: 0;">${num}</span>`).join("   ") + 
      `<br/><br/><br/><br/><div id="rtime">&gt; 추첨시각 : ${nowLocaleString}<br/>${"&nbsp;&nbsp;[ " + nowString + " ]"}</div><br/>` + 
      `<form name="download"><input type="submit" id="download" value="다운로드" title="누르면 추첨 결과를 텍스트 형식의 파일로 다운로드합니다." /></form>`;
    
    document.getElementById("console").innerHTML = resultHtml;

    // 숫자 애니메이션 실행
    numArray.forEach((num, index) => {
      const spanElement = document.querySelectorAll("#console span")[index];
      setTimeout(() => {
        animateNumber(spanElement, num);
      }, index * 50); // 각 숫자가 500ms 간격으로 나타남
    });

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
  <h2><p style="text-align: center">[2025년 독자 AI 파운데이션 모델 대국민 보고회]</p></h2>
  <p title="Randomizes non-overlapping numbers (integers) within the range of numbers entered below."><p style="text-align: center"><b>📢잠시 후 경품 추첨 이벤트가 시작됩니다. 당첨자는 등록데스크에서 경품을 수령해주세요.<b/></p>
  <form name="randomNumber">
    <p style="text-align: center"><span title="숫자 범위 (Range)">🔢랜덤 번호 추첨 범위</b></span>: &nbsp;<input type="number" size="6" id="startNUM" value="1" title="시작 값 (Begin)"/> ~ <input type="number" size="6" id="endNUM" value="100" title="끝 값 (End)" />&nbsp;&nbsp;&nbsp;&nbsp; <span title="추출 숫자의 갯수 (Count)">🎉행운의 당첨자 수</b></span>: &nbsp;<input type="number" size="6" id="selectNum" value="10" title="추출 숫자가 너무 큰 경우 오래 기다려야 할 수 있습니다." />&nbsp;&nbsp;&nbsp;&nbsp;
    </p>
    <div class="calcButton"><button class="btn-hover color-9" id="calcButton">지금 바로 추첨을 시작합니다!!</button>
</div>
  </form>
  <div id="console" title="추첨하기를 누르면 여기에 결과가 나와요. (Press the button to see the result.)"><p style="text-align: center"> 🎁여기에 무작위 당첨번호가 나타납니다!<br><br>1등: OOO(00명)&nbsp;&nbsp;&nbsp;&nbsp;2등: OOO(00명)&nbsp;&nbsp;&nbsp;&nbsp;3등: OOO(00명)</p></div>
<div class="bg-video">
  <video class="bg-video__content" autoplay muted loop>
    <source src="https://videos.pexels.com/video-files/30696371/13134125_1920_1080_30fps.mp4" type="video/mp4" />
    Your browser is not supported!
  </video>
</div>

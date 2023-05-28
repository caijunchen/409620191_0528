# 409620191_0528

# 第二次作業

### sketch
```javascript=
// let points = [[7,10],[12,6],[12,4],[9,1],[10,-2],[10,-7],[5,-10],[1,-11],[1,-13],[-3,-13],[-14,-4],[-13,4],[-11,9],[-12,13],[-10,16],[-8,17],[-5,13],[3,13],[7,16],[10,15],[10,13],[7,10]]


// let points =[[6, -3], [5, 0], [7, 2],[7,4],[6,5],[9,5],[9,6],[8,7],[7,8],[6,8],[5,10],[4,10],[4,9],[5,8],[4,5],[0,5],[-2,4],[-4,1],[-4,-6],[-5,-7],[-10,-6],[-9,-7],[-4,-8],[-3,-7],[-1,-5],[4,4],[3,2],[3,1],[5,-3],[4,-4],[5,-4],[6,-3],[4,1],[5,2],[1,-4],[2,-5],[2,-8],[8,-8],[7,-7],[3,-7],[3,-1],[4,-1],[3,-1],[2,-3],[0,-5],[-4,-2],[-3,-4],[-1,-5],[-1,-9],[5,-10],[6,-9],[0,-8],[0,-5],[1,0],[-1,3],[5,-4],[6,-4],[7,-3],[6,1]];

let points = [[-2, 0], [-1,-1], [0, -1],[1,0],[1,2],[0,3],[-1,3],[-2,2],[-3,2],[-4,1],[-4,-2],[-5,-4],[-4,-4],[-3,-2],[-2,-1],[-2,-3], [-2,-4], [-1, -4],[0,-4],[0,-2],[2,-2],[2,-4], [4, -4],[4,1],[3,2],[1,2],[1,2]]; //list資料，



var stroke_colors = "9a8f97-c3baba-e9e3e6-b2b2b2-736f72".split("-").map(a=>"#"+a)
var fill_colors = "cad2c5-84a98c-52796f-354f52-2f3e46".split("-").map(a=>"#"+a)

function preload(){  //最早執行的程式碼
  elephant_sound = loadSound("sound/elephant.wav")
  bullet_sound = loadSound("sound/Launching wire.wav")
  //  bg_sound = loadSound("sound/574197.wav")

}

var ball //大象物件，代表單一個物件，利用這個變數來做正在處裡的物件
var balls =[] //陣列，放所有的物件資料，物件倉庫，裡面儲存所有的物件資料
var bullet  //飛彈物件
var bullets =[]
var monster //怪物物件
var monsters =[] 
var score = 0
var shipP //設定砲台的位置
function setup() { //設定大象物件倉庫內的資料
  createCanvas(windowWidth, windowHeight);
  shipP = createVector(width/2,height/2)  //預設砲台的位置為視窗中間
  //產生幾個物件
  for(var j=0;j<30;j=j+1)
  {
    ball = new Obj({}) //產生一個新物件，"暫時"放入到ball變數中
    balls.push(ball) //把ball物件放入到balls物件群(陣列)中
  }
  for(var j=0;j<30;j=j+1)
  {
    monster = new Monster({}) //產生一個新物件，"暫時"放入到ball變數中
    monsters.push(monster) //把monster物件放入到monsters物件群(陣列)中
  }

  //bg_sound.play()
}

function draw() { //每秒會執行60次
  background(220);
  // for(var k=0;k<balls.length;k=k+1){
  //   ball = balls[k]
  //   ball.draw()
  //   ball.update()
  // }
  if(keyIsPressed){ //鍵盤是否被按下，如果有鍵盤被按下，keyIsPressed的值為true
    if(key=="ArrowLeft" || key=="a"){ //按下鍵盤的往左鍵
      shipP.x = shipP.x-5
    }
    if(key=="ArrowRight" || key=="d"){ //按下鍵盤的往右鍵
      shipP.x = shipP.x+5
    }
    if(key=="ArrowUp" || key=="w"){ //按下鍵盤的往上鍵
      shipP.y = shipP.y-5
    }
    if(key=="ArrowDown" || key=="s"){ //按下鍵盤的往下鍵
      shipP.y = shipP.y+5
    }
  }
  for(let ball of balls){ //針對陣列變數，取出陣列內一個一個的物件
    ball.draw()
    ball.update()
    //++++++++++由此判斷，每隻大象有沒有接觸每一個飛彈++++++++++
    for(let bullet of bullets){
      if(ball.isBallInRanger(bullet.p.x,bullet.p.y)) //判斷ball與bullet有沒有碰觸
      {
        score = score - 1 //分數加一分
        elephant_sound.play()
        balls.splice(balls.indexOf(ball),1) //讓大象從大象倉庫內移除
        bullets.splice(bullets.indexOf(bullet),1) //讓飛彈從飛彈倉庫內移除
      }
    }
    //+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

  }


  for(let bullet of bullets){ //針對飛彈倉庫內的資料，一筆一筆的顯示出來
    bullet.draw()
    bullet.update()
  }

  for(let monster of monsters){ //針對飛彈倉庫內的資料，一筆一筆的顯示出來
    if(monster.IsDead && monster.timenum==3){
      monsters.splice(monsters.indexOf(monster),1) //讓怪物從怪物倉庫內移除
    }
    monster.draw()
    monster.update()
    //++++++++++由此判斷，每隻怪物有沒有接觸每一個飛彈++++++++++
    for(let bullet of bullets){
      if(monster.isBallInRanger(bullet.p.x,bullet.p.y)) //判斷monster與bullet有沒有碰觸
      {
        score = score + 1 //分數扣一分
        // elephant_sound.play()
        //  monsters.splice(monsters.indexOf(monster),1) //讓怪物從怪物資料倉庫內移除
        monster.IsDead = true //已經被打到了，準備執行爆炸後的畫面
        bullets.splice(bullets.indexOf(bullet),1) //讓飛彈從飛彈倉庫內移除
      }
    }
    //+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
  }

  textSize(50)
  text(score,50,50)
  //++++++++++劃出中間三角形的炮台++++++++++
  push()
    let dx = mouseX-width/2  //滑鼠座標到中心點座標的x軸距離
    let dy = mouseY-height/2  //滑鼠座標到中心點座標的y軸距離
    let angle = atan2(dy,dx)  //利用反tan算出角度

    
    //translate(width/2,height/2) //砲台的位置
    translate(shipP.x,shipP.y) //砲台的位置，使用shipP的向量值
    rotate(angle)  //讓三角形翻轉一個angle角度
    noStroke()
    fill("#9ff595")
    ellipse(0,0,50) //劃出中間的圓
    fill("#6f5165")
    triangle(50,0,-25,-25,-25,25) //劃出三角形
  pop()
  //+++++++++++++++++++++++++++++++++

}

function mousePressed(){
  // ++++++++++++++++++++++++++++++++++++++++++++++++++
  // 按下滑鼠產生一個程式碼
  // ball = new Obj({
  //   p:{x: mouseX, y:mouseY }
  //  p:createVector(mouseX,mouseY)
  // }) //產生一個新物件，"暫時"放入到ball變數中
  // balls.push(ball)
  // ++++++++++++++++++++++++++++++++++++++++++++++++++

  // //按下滑鼠後，刪除該物件
  // for(let ball of balls){
  //   if (ball.isBallInRanger(mouseX,mouseY)){
  //     // 把倉庫的這個物件刪除
  //     score = score + 1
  //     balls.splice(balls.indexOf(ball),1) //把倉庫內編號第幾個刪除，只刪除1個(indexOf()找出ball的編號)
  //   }
  // }

  //新增一筆飛彈資料(還沒有顯示)
  bullet = new Bullet({
    // r:random(10,30) //不一樣大小的飛彈
    // color:random(stroke_colors) //飛彈的顏色
  })
  bullets.push(bullet) //把這一筆資料放到飛彈倉庫
  bullet_sound.play()
}

function keyPressed(){
  if(key==" "){
  //新增一筆飛彈資料(還沒有顯示)
    bullet = new Bullet({})
    bullets.push(bullet) //把這一筆資料放到飛彈倉庫
  // bullet_sound.play()
  }
  
}
```
### OBJ
```javascript=
class Obj{
  constructor(args){ //預設值，基本資料(包含有物件的顏色、位置、速度、大小...)
    // this.p = args.p || {x: random(width), y: random(height) } //一個物件開始的位置
    this.p = args.p || createVector(random(width),random(height))
    // this.v = {x: random(-1,1), y: random(-1,1) } //速度，x、y移動的速度為亂數產生-1，1之間的數字
    this.v = createVector(random(-1,1), random(-1,1)) //產生一個x座標值為random(-1,1),y座標值為random(-1,1)
    this.size = random(5,20) //放大倍率
    this.color = random(fill_colors) //充滿顏色
    this.stroke = random(stroke_colors) //線條顏色
  }
  draw() //把物件畫出來的函數
  {
    push() //重新設定，設定新的原點與顏色設定
      translate(this.p.x,this.p.y) //原點設定在物件所在位置
      scale((this.v.x<0?1:-1),-1) //放大縮小的指令，this.v.x<0?1:-1 ==>this.v.x<0條件成立的話，則值為1，否則為-1
      fill(this.color)
      stroke(this.stroke)
      beginShape()
        for(var i =0;i<points.length-1;i=i+1){
          // line(points[i][0]*this.size,points[i][1]*this.size,points[i+1][0]*this.size,points[i+1][1]*this.size)
          // vertex(points[i][0]*this.size,points[i][1]*this.size) //直角的線
          curveVertex(points[i][0]*this.size,points[i][1]*this.size) //不是直角的線
        }
      endShape(CLOSE) //把第一個點和最後一個點關起來

    pop()

  }
  update(){  //移動後設定位置資料值為何
    //移動的程式碼++++++++++++++++++++++++++++++++
    // this.p.x = this.p.x + this.v.x
    // this.p.y = this.p.y + this.v.y
    this.p.add(this.v)  //此行的效果跟上面兩行一樣，add為加
    //++++++++++++++++++++++++++++++++++++++++++++

    // 算出滑鼠位置的向量
    //+++++++++++++++++++++++隨著滑鼠方向移動，每隻大象移動速度都一樣(.limit(3)固定為3，向著滑鼠游標移動)
    // let mouseV = createVector(mouseX,mouseY) //把目前滑鼠的位置轉成向量值
    // let delta = mouseV.sub(this.p).limit(3) //delta值紀錄與滑鼠方向移動的"單位"距離，sub為向量減法，limit為每次移動單位
    //++++++++++++++++++++++++
    
    //++++++++++++++++++++++++隨著滑鼠方向移動，每隻大象移動速度隨著原本大象既有的速度移動
    // let mouseV = createVector(mouseX,mouseY) //把目前滑鼠的位置轉成向量值
    // let delta = mouseV.sub(this.p).limit(this.v.mag()*2) //與原本物件的速度有關，取得物件的速度值
    // this.p.add(delta)
    //++++++++++++++++++++++++
    
    //碰壁的處理程式碼++++++++++++++++++++++++++++
    if(this.p.x<=0 || this.p.x >= width) //<0碰到左邊，>width為碰到右邊
    {
      this.v.x = -this.v.x
    }
    if(this.p.y<=0 || this.p.y >= height) //<0碰到左邊，>width為碰到右邊
    {
      this.v.y = -this.v.y
    }
    //++++++++++++++++++++++++++++++++++++++++++++
  }
  

  //++++++++++++碰撞函數
  isBallInRanger(x,y){ //判斷滑鼠由沒有按到
    let d = dist(x,y,this.p.x,this.p.y) //計算滑鼠按下的點與此物件之間的距離
    if(d<this.size*4){ //4的由來:去看座標點最大的值，以此作為被點選方框的高與寬
      return true //代表距離有在範圍內
    }else{
      return false //代表距離沒有在範圍內
    }
  }
  //+++++++++++++++++++
    
}
```
### Monster
```javascript=
var monster_colors = "4f6d7a-c0d6df-dbe9ee-4a6fa5-166088".split("-").map(a=>"#"+a)
class Monster{
  constructor(args){ //預設值，基本資料(包含有物件的顏色、位置、速度、大小...)
    this.r = args.r || random(50,70) //怪物的外圓
    this.p = args.p || createVector(random(width),random(height)) //怪物起始的位置
    this.v = args.v || createVector(random(-1,1), random(-1,1)) //怪物的速度
    this.color = args.color || random(monster_colors) //怪物顏色
    this.mode = random(["happy","bad"])
    this.IsDead = false //false代表怪物還活著
    this.tomenum=0
  }

  draw(){ //畫怪物
    if(this.IsDead==false){ //活著時，畫的畫面
      push()
        translate(this.p.x,this.p.y)
        fill(this.color)
        noStroke() //不要有框線
        ellipse(0,0,this.r)
        if(this.mode == "happy"){ //眼睛為全圓
          fill(255)
          ellipse(0,0,this.r/2)
          fill(0)
          ellipse(0,0,this.r/3)   
        }else{ //眼睛為半圓
          fill(255)
          arc(0,0,this.r/2,this.r/2,0,PI)
          fill(0)
          arc(0,0,this.r/3,this.r/3,0,PI)
            
        }
        //產生腳
        stroke(this.color)
        strokeWeight(4)
        // line(this.r/2,0,this.r,0)
        noFill();
        for(var j=0;j<8;j++){
          rotate(PI/4) //因為要產生八隻腳，一隻腳要旋轉45度，PI代表180，PI/4代表45度
          beginShape()
            for(var i=0;i<(this.r/2);i++){
              vertex(this.r/2+i,sin(i/5+frameCount/10)*10)
            }
          endShape()   
        }    
      pop()
    }else{ //死後爆炸的畫面圖
      this.timenum = this.timenum+1
      push()
        translate(this.p.x,this.p.y)
        fill(this.color)
        noStroke() //不要有框線
        ellipse(0,0,this.r)
        stroke(255)
        line(-this.r/3,0,this.r/3,0)  //眼睛的線
        //產生腳
        stroke(this.color)
        strokeWeight(4)
        // line(this.r/2,0,this.r,0)
        noFill();
        for(var j=0;j<8;j++){
          rotate(PI/4) //因為要產生八隻腳，一隻腳要旋轉45度，PI代表180，PI/4代表45度
          line(this.r/2,0,this.r,0)  //八隻腳產生一個直線
        }
      pop()
    }

  }

  update(){
    this.p.add(this.v)
    if(this.p.x<=0 || this.p.x >= width) //<0碰到左邊，>width為碰到右邊
    {
      this.v.x = -this.v.x
    }
    if(this.p.y<=0 || this.p.y >= height) //<0碰到左邊，>width為碰到右邊
    {
      this.v.y = -this.v.y
    }
  }
  isBallInRanger(x,y){ //判斷滑鼠由沒有按到
    let d = dist(x,y,this.p.x,this.p.y) //計算飛彈與此物件(怪物)之間的距離
    if(d<this.r/2){ //飛彈與怪物間的距離如果小於半徑(this.r/2)，代表碰撞到
      return true //代表距離有在範圍內
    }else{
      return false //代表距離沒有在範圍內
    }
  }
}
```
### Bullet
```javascript=
class Bullet{
  constructor(args){ //預設值，基本資料(包含有物件的顏色、位置、速度、大小...)
    this.r = args.r || 10  //如果飛彈有傳回直徑的大小，就以參數為直徑，否則預設為10
    this.p = args.p || shipP.copy() //createVector(width/2,height/2) //飛彈起始的位置(以向量方式表示該座標)，要以中間砲台發射，所以座標為(width/2,height/2)
    this.v = args.v || createVector(mouseX-width/2,mouseY-height/2).limit(10) //飛彈的速度
    this.color = args.colors || "red" //飛彈顏色
  }
  draw(){ //劃出飛彈
    push()
      translate(this.p.x,this.p.y)
      fill(this.color)
      noStroke()
      ellipse(0,0,this.r)
      // rectMode(CENTER)
      // rect(0,0,20,40)
      // triangle()
    pop()
  }
  update(){ //計算移動後的位置
    this.p.add(this.v)
  }
}
```

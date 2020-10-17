
//defining variables
var monkey , monkey_running;
var banana ,bananaImage, obstacle, obstacleImage;
var foodGroup, obstacleGroup;
var score;
var banana;
var obstacle;
var food;

function preload(){
  
  //loading all animations, and images before everything else
  monkey_running =            loadAnimation("sprite_0.png","sprite_1.png","sprite_2.png","sprite_3.png","sprite_4.png","sprite_5.png","sprite_6.png","sprite_7.png","sprite_8.png");
  
  bananaImage = loadImage("banana.png");
  obstacleImage = loadImage("obstacle.png");
  
}



function setup() {
  
  //creating the canvas
  createCanvas(600, 600);
  
  //initializing the score
  score = 0;
  
  //creating the monkey
  monkey = createSprite(100, 400, 50, 50);
  monkey.addAnimation("moving", monkey_running);
  monkey.scale = 0.2;
 
  //creating the ground
  ground = createSprite(150, 460, 900, 10);
  ground.velocityX = -(4 + score/1000);
  ground.x = ground.width/2;

  //creating the groups
  foodGroup = createGroup();
  obstacleGroup = createGroup();

  monkey.debug = true
  
  //setting the collider for the monkey  
  monkey.setCollider("rectangle", 8, 20, 410, 500)
}

 


function draw() {
  
  //setting the background
  background("180");
  
  //drawing sprites
  drawSprites();
  
  //resetting the ground
  if(ground.x < 300){
    ground.x = ground.width/2;
  }
  
  //making the monkey stay on the ground
  monkey.collide(ground);
  
  //giving the monkey points if it touches a banana
  if(monkey.isTouching(foodGroup)){
    foodGroup.destroyEach();
    score = score+100;
    
  }
  
  
  if(!monkey.isTouching(obstacleGroup)){
    obstacles();
    food();
    monkey.velocityY = monkey.velocityY + 0.8;
    score = score +          
     Math.round(3.15*getFrameRate()/190);
    if(keyDown("space")&& monkey.y >= 300) {
        monkey.velocityY = -15;
    }
  }
  
  //stopping everything and displaying text when monkey is touching an obstacle
  else {
    obstacleGroup.setVelocityXEach(0)
    obstacleGroup.setLifetimeEach(-1)
    foodGroup.setVelocityXEach(0)
    foodGroup.setLifetimeEach(-1)
    score = 0
    stroke("black")
    textSize(40)
    fill("black")
    text("GAME OVER", 290, 300)
    stroke("black")
    textSize(20)
    fill("black")
    text("Press R to restart", 290, 350)
 }
  
  //making the game reset if you press r
  if(keyDown("r") && monkey.isTouching(obstacleGroup)){
    obstacleGroup.destroyEach();
    foodGroup.destroyEach();
    obstacles();
    food();
    score = score +          
     Math.round(3.15*getFrameRate()/190);
    monkey.velocityY = monkey.velocityY + 0.8;
    if(keyDown("space")&& monkey.y >= 300) {
        monkey.velocityY = -15;
  }
  }
  
  // showing the survival time
  stroke("black");
  textSize(20);
  fill("black");
  text("Survival Time: " + score, 50, 50);
  

}

//making the food function
function food() {
  if(frameCount%80 === 0){
     banana = createSprite(400, 400, 30, 30);
     banana.addImage(bananaImage);
     banana.y = Math.round(random(120, 400));
     foodGroup.add(banana);
     banana.scale = 0.1
     banana.velocityX = -(10 + score/1000)
     banana.setLifetime = 100
  }
}

//creating the obstacles function
function obstacles() {
  if(frameCount%300 === 0){
    obstacle = createSprite(800, 420, 30, 30);
    obstacle.addImage(obstacleImage)
    obstacleGroup.add(obstacle)
    obstacle.velocityX = -(10 + score/1000)
    obstacle.scale = 0.25
    obstacle.setLifetime = 100
  } 
} 

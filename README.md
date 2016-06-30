
<script src="processing.js"></script>
<script type="text/processing" data-processing-target="mycanvas">
final int nombre_de_boule = 75;
int[][] boule = new int[nombre_de_boule][5];//1ere case pos en x, 2 en y, 3 diametre, 4  mouvement en x, 5 en y
int[][] direction_boule = new int[nombre_de_boule][2];//3 = de gauche a droite ou de haut en bas 1 = inverse             1ere case en x, 2eme en y
color[] boule_color = new color[nombre_de_boule];
color[] couleur = {color(255, 0, 0), color(0, 255, 0), color(0, 0, 255), color(33, 64, 124), color(109, 7, 26), color(244, 102, 27), color(0, 255, 255), color(233, 56, 63), color(255, 255, 0), color(255, 0, 255), color(88, 41, 0), color(127, 0, 255)};
int mass_player = 20;
color color_player = color(255, 255, 255);
float distanceX, distanceY;
boolean lose = false;
boolean pause = false;

void setup() {
  frameRate(60);
  size(1200, 650);
  noCursor();
  for (int i = 0; i < nombre_de_boule; i++) {
    assigner_les_boules(i);
  }
}

void detect_collision() {
  for (int i = 0; i < nombre_de_boule; i++) {
    if (mouseX > boule[i][0]) {      
      distanceX = mouseX - boule[i][0];
    } else {      
      distanceX = boule[i][0] - mouseX;
    }
    if (mouseY > boule[i][1]) {      
      distanceY = mouseY - boule[i][1];
    } else {      
      distanceY = boule[i][1] - mouseY;
    }

    float distance = sqrt(distanceX*distanceX + distanceY*distanceY);
    if (distance < mass_player/2 + boule[i][2]/2) {
      if (mass_player > boule[i][2]) {
        mass_player = mass_player + 1;
        color_player = boule_color[i];
        assigner_les_boules(i);
      } else {
        lose = true;
      }
    }
  }
}
void assigner_les_boules(int i) {
  int alea = int ( random(0, 4));
  if (alea == 0) {
    boule[i][0] = choix_posX();
    boule[i][1] = 0;
    direction_boule[i][0] = int ( random ( 1, 3));
    direction_boule[i][1] =  3;
  } else if (alea == 1) {
    boule[i][0] = choix_posX();
    boule[i][1] = height;
    direction_boule[i][0] = int ( random ( 1, 3));
    direction_boule[i][1] =  1;
  } else if (alea == 2) {
    boule[i][0] = 0;
    boule[i][1] = choix_posY();
    direction_boule[i][0] = 3;
    direction_boule[i][1] =  int ( random ( 1, 3));
  } else if (alea == 3) {
    boule[i][0] = width;
    boule[i][1] = choix_posY();
    direction_boule[i][0] = 1;
    direction_boule[i][1] =  int ( random ( 1, 3));
  }
  boule[i][2] = choix_Masse ();
  boule[i][3] = choix_Speed_X();
  boule[i][4] = choix_Speed_Y();
  boule_color[i] = couleur[int ( random (0, 12))];
}

void detecter_outside() {
  for (int i = 0; i < nombre_de_boule; i++) {
    if (boule[i][0] > width+(boule[i][2]/2)+20 || boule[i][0] < -(boule[i][2]/2)-20 || boule[i][1] > height+(boule[i][2]/2)+20 || boule[i][1] < -(boule[i][2]/2)-20) {//si la boule est dehors
      assigner_les_boules(i);//en en recre une boule dans la map
    }
  }
}

void mouvement_de_la_boule() {
  for (int i = 0; i < nombre_de_boule; i++) {
    boule[i][0] = boule[i][0] + boule[i][3];
    boule[i][1] = boule[i][1] + ((direction_boule[i][1] - 2)*boule[i][4]);
  }
}

void dessin_des_boules() {
  for (int i = 0; i < nombre_de_boule; i++) {
    fill(boule_color[i]);
    ellipse(boule[i][0], boule[i][1], boule[i][2], boule[i][2]);
  }
}

void draw() {
  
  if(pause == false){
  background(0, 0, 0);
  textSize(30);
    fill(0,120,0);
  text("score = "+(mass_player-20),1000,100);
  detecter_outside();
  dessin_des_boules();
    mouvement_de_la_boule();
      if (lose == false) {
    dessin_player();
    detect_collision();
  } else {
    cursor();
    textSize(50);
    fill(80,80,80);
    text("Game Over", width/2-200, height/2-75);
    text("Size = " + (mass_player-20), width/2-170, height/2)
    text("Game made by Pablo",width/2-300,height/2+75)
  }
}else{
  text("Game Paused", width/2-200, height/2-75);
  
}
}

void mousePressed() {
  if (lose == true) {
    noCursor();
    lose = false;
    mass_player = 20;
      for (int i = 0; i < nombre_de_boule; i++) {
    assigner_les_boules(i);
  }
  }
}

void dessin_player() {
  fill(color_player);
  ellipse(mouseX, mouseY, mass_player, mass_player);
}
//-----------------------------------------------------------------FONCTIONS TIRAGE RANDOM DES BOULES-------------------------------------------------------------------------------\\ 
int choix_posX () {
  return int (random(50, width-50));
}
int choix_posY () {
  return int (random(50, height-50));
}
int choix_Masse () {
    return int ( random (mass_player*0.85,mass_player*1.8));
}
int choix_Speed_X() {
  return int ( random ( width/( frameRate * 20), width / (frameRate *8 )));
}
int choix_Speed_Y() {
  return int ( random ( height/( frameRate * 25), height / (frameRate *5 )));
}
</script>
<canvas id="mycanvas"></canvas>

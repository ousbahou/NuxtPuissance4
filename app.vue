<template>
  <h1 style="text-align: center;color:white;">PUISSANCE 4 | Pair Programming | Houda & Luc</h1>

  <!----------------------------GRILLE DE JEU------------------------------>
  <div class="grille">
    <div v-for="lignes in 6" :key="lignes" class="lignes">
      <div v-for="colonne in 7" :key="colonne" class="colonne" @click="Deplacements(colonne - 1)">

        <!----------------------------v if PIION ROUGE------------------------------>
        <div class="pion" v-if="grille[lignes - 1][colonne - 1] === 'rouge'">
          <img src="./.nuxt/assets/pionRouge.png" width="90" alt="pionRouge">
        </div>

        <!---------------------------- v if PION JAUNE------------------------------>
        <div class="pion" v-if="grille[lignes - 1][colonne - 1] === 'jaune'">
          <img src="./.nuxt/assets/pionJaune.png" width="90" alt="pionJauen">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //fire une grille vide si possible à la place de 0
      grille: [
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', '']
      ],
      Joueur1: 'rouge'
    }
  },
  methods: {
    Deplacements(colonne) {
      //déplacement de index5 à 0
      //faire qu'à chaque tour regarde si c'est gagnant
      for (let lignes = 5; lignes >= 0; lignes--) {
        if (this.grille[lignes][colonne] === '') {
          this.grille[lignes][colonne] = this.Joueur1;
          if (this.FonctionGagnant(lignes, colonne)) {
            setTimeout(() => {
              alert(this.Joueur1 + ' a gagné !');
            this.JeuAZero();
            }, 100); 
          } else {
            if (this.Joueur1 === 'rouge') {
              this.Joueur1 = 'jaune';
            } else {
              this.Joueur1 = 'rouge';
            }
          }
          break;
        }
      }
    },

    //faire toutes les vérifs ici
    FonctionGagnant(lignes, colonne) {
      let count = 0;
      let joueur = this.grille[lignes][colonne];


      // vérifs horizontales
      for (let i = 0; i < 7; i++) {
        if (this.grille[lignes][i] === joueur) {
          // console.log("count4");
          count++;
        } else {
          count = 0;
        }
        if (count === 4) {
          return true;
        }
      }
      count = 0;


      // Vérifs verticalement
      for (let i = 0; i < 6; i++) {
        //ok
        if (this.grille[i][colonne] === joueur) {
          count++;
        } else {
          //sinn tu le fou à zero
          count = 0;
        }
        if (count === 4) {
          // console.log("count4");
          return true;
        }
      }
      count = 0;

      // Verifs diagonales haut-gauche vers bas-droite
      let DebutLignes = lignes - Math.min(lignes, colonne);
      let Debutcolonne = colonne - Math.min(lignes, colonne);
      for (let i = DebutLignes, j = Debutcolonne; i < 6 && j < 7; i++, j++) {
        if (this.grille[i][j] === joueur) {
          count++;
        } else {
          count = 0;
        }
        if (count === 4) {
          return true;
        }
      }
      count = 0;

      // Vérification diagonale haut-droite vers bas-gauche
      DebutLignes = lignes - Math.min(lignes, 6 - colonne - 1);
      Debutcolonne = colonne + Math.min(lignes, 6 - colonne - 1);
      for (let i = DebutLignes, j = Debutcolonne; i < 6 && j >= 0; i++, j--) {
        if (this.grille[i][j] === joueur) {
          count++;
        } else {
          count = 0;
        }
        if (count === 4) {
          return true;
        }
      }
      return false;
    },

    //remise du jeu a zero apres avoir perdu ou gagné
    JeuAZero() {
      this.grille = [
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', ''],
        ['', '', '', '', '', '', '']
      ];
      this.Joueur1 = 'rouge';
    }
  }
}
</script>

<style>
.grille {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.lignes {
  display: flex;
  background-color: #452af7;
}

.colonne {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  height: 80px;
  border: 6px solid #452af7;
  border-radius: 50px;
  background-color: white;
}

.pion {
  display: flex;
  justify-content: center;
  align-items: center;
}

.pion img {
  width: 80px;
  height: 80px;
}

.alert {
  padding: 10px;
  background-color: #f2f2f2;
  text-align: center;
}

.alert-red {
  color: red;
}

.alert-jaune {
  color: yellow;
}

html {
  background-image: url("./.nuxt/assets/dessin.jpg");
  background-size: cover;
  width: 100%;
  height: 100vh;
}
</style>
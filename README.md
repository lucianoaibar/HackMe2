# HackMe2
Analyze this mini animation engine and modify "GameData" so that the crying face catches up with the happy face.

![screenshot](https://github.com/lucianoaibar/HackMe2/blob/main/screenshot.png?raw=true)

## Full source code
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HackMe 2</title>
  <style>
    #map { display: flex; flex-direction: row; flex-wrap: wrap; width: 256px; }
    .tile { width: 32px; height: 32px; }
    .grass { background-color: #060; }
    .water { background-color: #04A; }
    .wall { background-color: #443; }
  </style>
</head>
<body>
  <h2>HackMe 2</h2>
  <p>Analyze this mini "animation engine" and modify "GameData" so that the crying face catches up with the happy face.</p>
  <p>by Luciano Aibar - lucianoaibar@gmail.com</p>
  <div id="map"></div>
  <script>
    (() => {
      'use strict';

      let Map         = document.getElementById('map');
      let Terrain     = [];
      let Player      = { x: 5, y: 1, data: [] };
      const TileClass = [ 'grass', 'water', 'wall' ];
      const Moves     = [ [0, 0], [1, 0], [0, 1], [-1, 0], [0, -1] ];
      const GameData  = [ 57, 25, 35, 27, 65, 27, 17, 19, 17, 27, 81, 25, 19, 27, 81, 19, 57, 19, 57, 11, 145, 19, 49, 129, 33, 27, 27, 35, 145, 5 ];

      const $ = [
        () => GameData.forEach(x => $[((x >>= 1) & 3) + 1](x >> 2)),
        (a, b) => {
          if(b===undefined) b = a++ >> 3;
          a &= 7;
          Terrain.push(b);
          if(--a) $[1](a, b);
        },
        a => Player.data.push( Moves[a & 7] ),
        () => {
          Player.data = Player.data.reverse();
          var x = () => {
            let str = '';
            let n = 0;
            let position_in_map = Player.x + (Player.y << 3);

            Terrain.forEach(x => {
              str +=
                '<div class="tile ' + TileClass[x] + '">' +
                (n++==position_in_map ? '&#128557;' : (n==55 ? '&#128540;' : ' ')) +
                '</div>';
            });
            Map.innerHTML = str;

            let move = Player.data.pop();
            if(move) {
              Player.x += move[0];
              Player.y += move[1];
            }
          };
          x();
          window.setInterval(x, 500);
        }
      ];

      return $;
    })()[0]();
  </script>
</body>
</html>
```

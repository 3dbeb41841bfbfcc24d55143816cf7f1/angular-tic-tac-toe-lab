# LAB: Use AngularJS to create a Tic-Tac-Toe game in a CodePen

The purpose of this lab it to let the students get more practice with using an AngularJS controllers.
The `ticTacToeCtrl` provides data and methods for the view to display and interact with.

In addtion this lab uses the following built-in AngularJS directives:

* ng-app
* ng-controller
* ng-repeat
* ng-class
* ng-click
* ng-hide
* ng-show

## Codepens

* [Starter](http://codepen.io/drmikeh/pen/GJvBEy)
* [Solution](http://codepen.io/drmikeh/pen/OyjZPX?editors=101)

## Starter Code

### HTML

```html
<html ng-app='ticTacToeApp'><head>
  <script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script src="//maxcdn.bootstrapcdn.com/bootstrap/3.3.0/js/bootstrap.min.js"></script>
  <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script>
  <script src="//cdnjs.cloudflare.com/ajax/libs/underscore.js/1.7.0/underscore-min.js"></script>
</head>

<body ng-controller="ticTacToeCtrl as ctrl">
  <h1>Tic Tac Toe!</h1>

  <div class="container">
    <div class="well">
      <div ng-repeat="row in ctrl.board">
        <button class="btn-lg"
                ng-repeat="cell in row"
                ng-disabled="isTaken(cell);"
                ng-class="cell.value"
                ng-click="ctrl.move(cell);">{{ cell.value }}</button>
      </div>
      <br/>
      <p>
        <span ng-hide="ctrl.winner || ctrl.cat">Current Player: {{ ctrl.currentPlayer }}</span>
        <span ng-show="ctrl.winner">Player {{ ctrl.currentPlayer }} won!</span>
        <span ng-show="ctrl.cat">Cat</span>
      </p>
    </div>
  </div>

  <button ng-click="ctrl.reset();" class="btn btn-primary btn-large">New Game</button>
</body>
</html>
```

### CSS

```css
* {
  text-align: center;
  font-family: "Courier"
}

body {
  padding: 20px 40px;
  font-family: "Verdana";
}

h1 {
  margin: 20px;
}

button {
  background-color: white;
}

.t3cell {
  width: 24px;
  height: 24px;
  min-width: 24px;
  min-height: 24px;
}

.X {
  color: green;
}

.O {
  color: red;
}
```

### JavaScript

```javascript
angular.module('ticTacToeApp', []);

angular.module('ticTacToeApp')
.controller('ticTacToeCtrl', function() {
  var vm = this;
  var emptyCell = '?';

  vm.board = [
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ],
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ],
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ]
  ];

  vm.reset = function() {
    _.each(vm.board, function(row) {
      _.each(row, function(cell) {
        cell.value = emptyCell;
      });
    });
    vm.currentPlayer = 'X';
    vm.winner = false;
    vm.cat = false;
  };

  vm.reset();

  vm.isTaken = function(cell) {
    return cell.value !== emptyCell;
  };

  var checkForMatch = function(cell1, cell2, cell3) {
    // TODO: return true of cell1, cell2, and cell3 all match and are not the emptyCell
  };

  var isBoardFull = function() {
    // TODO: return true if the board is full (no empty cells)
  };

  var checkForEndOfGame = function() {
    // TODO: update the booleans $scope.winner and $scope.cat

    return vm.winner || vm.cat;
  };

  vm.move = function(cell) {
    cell.value = vm.currentPlayer;
    if (checkForEndOfGame() === false) {
      vm.currentPlayer = vm.currentPlayer === 'X' ? 'O' : 'X';
    }
  };
});
```

## Solution Code

### HTML

See starter HTML.

### CSS

See starter CSS.


### JavaScript

```javascript
angular.module('ticTacToeApp', []);

angular.module('ticTacToeApp')
.controller('ticTacToeCtrl', function() {

  var emptyCell = '?';
  var vm = this;

  vm.board = [
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ],
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ],
    [ { value: emptyCell }, { value: emptyCell }, { value: emptyCell } ]
  ];

  vm.reset = function() {
    vm.board.forEach(function(row) {
      row.forEach(function(cell) {
        cell.value = emptyCell;
      });
    });
    vm.currentPlayer = 'X';
    vm.winner = false;
    vm.cat = false;
  };

  vm.reset();

  vm.isTaken = function(cell) {
    return cell.value !== emptyCell;
  };

  function checkForMatch(cell1, cell2, cell3) {
    return cell1.value === cell2.value &&
           cell1.value === cell3.value &&
           cell1.value !== emptyCell;
  };

  function isBoardFull() {
    for (var row=0; row<=2; row++) {
      for (var col=0; col<=2; col++) {
        if (vm.board[row][col].value === emptyCell) {
          return false;
        }
      }
    }
    return true;
  }

  function checkForEndOfGame() {
    var rowMatch = checkForMatch(vm.board[0][0], vm.board[0][1], vm.board[0][2]) ||
                   checkForMatch(vm.board[1][0], vm.board[1][1], vm.board[1][2]) ||
                   checkForMatch(vm.board[2][0], vm.board[2][1], vm.board[2][2]);

    var columnMatch = checkForMatch(vm.board[0][0], vm.board[1][0], vm.board[2][0]) ||
                      checkForMatch(vm.board[0][1], vm.board[1][1], vm.board[2][1]) ||
                      checkForMatch(vm.board[0][2], vm.board[1][2], vm.board[2][2]);

    var diagonalMatch = checkForMatch(vm.board[0][0], vm.board[1][1], vm.board[2][2]) ||
                        checkForMatch(vm.board[2][0], vm.board[1][1], vm.board[0][2]);

    vm.winner = rowMatch || columnMatch || diagonalMatch;
    vm.cat = vm.winner === false && vm.isBoardFull();
    return vm.winner || vm.cat;
  };

  vm.move = function(cell) {
    cell.value = vm.currentPlayer;
    if (vm.checkForEndOfGame() === false) {
      vm.currentPlayer = vm.currentPlayer === 'X' ? 'O' : 'X';
    }
  };
});
```

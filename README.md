# Flutter_Tic-Tac-
/*
 * TODO:
 * 4. Checking winner.
 * 5. Checking draw game.
 * 6. Keep track of player score.
 * */

// I edited this

import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      debugShowCheckedModeBanner: false,
      home: TicTacEmo(),
    ),
  );
}

class Players {
  static String? X;
  static String? O;
  static String noPlayer = '';

  static void setPlayers(String X, String O) {
    Players.X = X;
    Players.O = O;
  }
}

class Utils {
  // Convert List<M> to List<Widget> where M is any type.
  static List<Widget> modelBuilder<M>(
      List<M> models, Widget Function(int index, M model) builder) {
    return models
        .asMap()
        .map<int, Widget>(
            (index, model) => MapEntry(index, builder(index, model)))
        .values
        .toList();
  }
}

class TicTacEmo extends StatefulWidget {
  @override
  _TicTacEmoState createState() => _TicTacEmoState();
}

class _TicTacEmoState extends State<TicTacEmo> {
  late List<List<String>> matrix;
  final int matrixSize = 3;
  String lastMove = Players.noPlayer;

  @override
  void initState() {
    super.initState();
    Players.setPlayers('👺', '😇');
    setEmptyFields();
  }

  void setEmptyFields() {
    matrix = List.generate(
        matrixSize, (_) => List.generate(matrixSize, (_) => Players.noPlayer));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blueAccent,
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: Utils.modelBuilder(
          matrix,
          (x, value) => buildRow(x),
        ),
      ),
    );
  }

  Widget buildRow(int x) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: Utils.modelBuilder(
        matrix[x],
        (y, value) => buildField(x, y),
      ),
    );
  }

  Widget buildField(int x, int y) {
    return Container(
      margin: EdgeInsets.all(4),
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(
          minimumSize: Size(92, 92),
          primary: Colors.white,
        ),
        onPressed: () {
          setField(matrix[x][y], x, y);
        },
        child: Text(
          matrix[x][y],
          style: TextStyle(fontSize: 42),
        ),
      ),
    );
  }

  void setField(String value, int x, int y) {
    if (value == Players.noPlayer) {
      final nextPlayer = lastMove == Players.X ? Players.O : Players.X;
      setState(() {
        matrix[x][y] = nextPlayer!;
        lastMove = nextPlayer;
      });
    }
    if(isWinner(x,y)){
      print('Player $lastMove has won') playerXScore++;
      else playerOScore++;
    }
    else if(isDraw()){
      print('Undecided Game!');
    }
  }
  bool isDraw(){
    return matrix.every((row)=> row.every((element) => element !=Players.noPlayer));
  }
  bool isWinner(int x,int y){
    final int n= matrixSize;
    final String player =matrix[x][y];
    int row = 0, col = 0,diag = 0,antiDiag = 0;
    for(int i=0; i<n; i++){
      if(matrix[x][i]==player) row++;
      if(matrix[i][x]==player) col++;
      if(matrix[i][i]==player) diag++;
      if(matrix[i][n-i-1]==player) antiDiag++;
    }
    return row ==n || col ==n || diag ==n || antiDiag ==n;
  }
}

# O Puzzle

Separa a lógica do puzzle da lógica de impressão.

O código inical é dificil de testar, pois ele retornava void, separando a lógica de impressão, podemos testar a lógica do puzzle separadamente.

``` java
   public class Puzzle {

    private int input;
    private int output;

    private List<Number> queue;
    private Set<Integer> visited;

    public Puzzle() {
        this.queue = new ArrayList<>();
        this.visited = new HashSet<>();
    }

    public void generatePath(int input, int output) {
        this.input = input;
        this.output = output;

        formatOutput(searchForSolution());
    }

    private Number searchForSolution() {

        addRootToTheQueue();

        while(thereAreNumbersInTheQueue()) {
            Number currentNumber = removeFromTheQueue();

            if(foundTheOutput(currentNumber)) return currentNumber;
            addToQueue(
                    multiplyByTwo(currentNumber),
                    (isEven(currentNumber)? divideByTwo(currentNumber) : null),
                    plusTwo(currentNumber)
            );
        }

        return null;
    }

    private boolean isEven(Number number) {
        return number.getValue()%2==0;
    }

    private boolean foundTheOutput(Number number) {
        return number.getValue() == output;
    }

    private boolean thereAreNumbersInTheQueue() {
        return queue.size()!=0;
    }

    private void addRootToTheQueue() {
        queue.add(new Number(input, null));
    }

    private void addToQueue(Number... numbers) {
        for(Number number : numbers) {
            if(number !=null) {
                if(!visited.contains(number.getValue())) {
                    queue.add(number);
                    visited.add(number.getValue());
                }
            }
        }
    }

    private void formatOutput(Number solution) {
        String output = "";
        while(solution!=null) {
            output = solution.getValue() + " " + output;
            solution = solution.getParent();
        }
        System.out.println(output);
    }

    private Number multiplyByTwo(Number number) {
        return new Number(number.getValue()*2, number);
    }

    private Number divideByTwo(Number number) {
        return new Number(number.getValue()/2, number);
    }

    private Number plusTwo(Number number) {
        return new Number(number.getValue()+2, number);
    }

    private Number removeFromTheQueue() {
        Number head = queue.get(0);
        queue.remove(0);
        return head;
    }


}
 ```

criar uma classe separada chamada `PuzzleOutput` que tem a logica de impressão. Ela recebe somente o objeto `Number` e imprime a saída formatada.

renomeasmo a classe `Puzzle` para `PuzzleSolver`, pois ela é responsável por resolver o puzzle, e a classe `PuzzleOutput` é responsável por imprimir a saída. temos uma nova clase chamada `PuzzleRunner` que é responsável por executar o puzzle e imprimir a saída.

``` java

class PuzzleRunner {

 private PuzzleSolver puzzleSolver;

 private PuzzleOutput puzzleOutput;

 public PuzzleRunner(PuzzleSolver puzzleSolver, PuzzleOutput puzzleOutput) {
  this.puzzleSolver = puzzleSolver;
  this.puzzleOutput = puzzleOutput;
 }

 public void run(int input, int output) {
  Number number = puzzleSolver.generatePath(input, output);
  puzzleOutput.print(puzzleSolver.getSolution());
 }

}

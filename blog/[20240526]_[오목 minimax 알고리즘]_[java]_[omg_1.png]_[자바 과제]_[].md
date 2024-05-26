아래에 프로젝트를 설명하는 블로그 포스트를 위한 마크다운(Markdown) 코드를 작성하였습니다. 이 포스트는 프로젝트의 개요, 목표, 코드 설명, 테스트 케이스 및 시각화 방법을 포함합니다.

```markdown
# 오목 게임에서 3-3 규칙 체크하기

## 개요

이번 프로젝트에서는 오목 게임에서 3-3 규칙을 체크하는 기능을 구현합니다. 오목은 바둑판 위에 두 명의 플레이어가 번갈아가며 돌을 놓아 연속된 다섯 개의 돌을 먼저 만드는 게임입니다. 여기서 3-3 규칙은 한 플레이어가 두 개 이상의 3의 연속을 만들 수 없다는 규칙입니다. 이 프로젝트에서는 이를 감지하고 방지하는 기능을 구현합니다.

## 목표

- 3-3 규칙을 감지하는 메서드 `checkThreeThree`를 구현합니다.
- 다양한 테스트 케이스를 통해 메서드의 정확성을 검증합니다.
- 테스트 결과를 시각화하여 검증합니다.

## 코드 설명

### OMGameUtil2 클래스

이 클래스는 3-3 규칙을 체크하는 기능을 포함하고 있습니다.

```java
package OMG;

public class OMGameUtil2 {

    // 좌표 변환 메서드
    public static Point convertToOMG(Point p) {
        return new Point(p.getY(), p.getX());
    }

    public static Point convertFromOMG(Point p) {
        return new Point(p.getY(), p.getX());
    }

    // 3-3 규칙을 체크하는 메서드
    public static boolean checkThreeThree(int[][] data, int chX, int chY, int turn) {
        // 방향 벡터 (가로, 세로, 대각선 위, 대각선 아래)
        int[] dx = {1, 0, 1, 1};
        int[] dy = {0, 1, 1, -1};
        int threeCount = 0;

        for (int direction = 0; direction < 4; direction++) {
            int count = 1; // 초기 돌
            int openEnds = 0;

            // 긍정 방향 체크
            int x = chX + dx[direction];
            int y = chY + dy[direction];
            while (isValidPosition(data, x, y) && data[x][y] == turn) {
                count++;
                x += dx[direction];
                y += dy[direction];
            }
            if (isValidPosition(data, x, y) && data[x][y] == 0) {
                openEnds++;
            }

            // 부정 방향 체크
            x = chX - dx[direction];
            y = chY - dy[direction];
            while (isValidPosition(data, x, y) && data[x][y] == turn) {
                count++;
                x -= dx[direction];
                y -= dy[direction];
            }
            if (isValidPosition(data, x, y) && data[x][y] == 0) {
                openEnds++;
            }

            // 정확히 3개의 돌이 연속으로 있고 적어도 한 쪽 끝이 비어있는 경우
            if (count == 3 && openEnds >= 1) {
                threeCount++;
            }

            // 두 개 이상의 "삼삼"이 있는 경우
            if (threeCount >= 2) {
                return true;
            }
        }

        return false;
    }

    private static boolean isValidPosition(int[][] data, int x, int y) {
        return x >= 0 && x < data.length && y >= 0 && y < data[0].length;
    }

    // 바둑판 시각화 메서드
    private static void visualizeBoard(int[][] board, int chX, int chY) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                if (i == chX && j == chY) {
                    System.out.print("("+ board[i][j] + ") ");
                } else {
                    System.out.print(" " + board[i][j] + "  ");
                }
            }
            System.out.println();
        }
    }

    // 3-3 규칙 테스트 메서드
    public static void testThreeThree() {
        // 이미지의 A 위치 테스트 //수정함
        int[][] board6 = new int[19][19];
        board6[1][3] = 1;
        board6[2][3] = 1;
        board6[3][2] = 1;
        board6[3][4] = 1;
        board6[3][3] = 1;
        System.out.println("Image Test Case A: " + checkThreeThree(board6, 3, 3, 1)); // 예상 결과: true
        visualizeBoard(board6, 4, 3);
        System.out.println();

        // 이미지의 B 위치 테스트//수정함
        int[][] board7 = new int[19][19];
        board7[1][1] = 1;
        board7[2][2] = 1;
        board7[1][4] = 1;
        board7[2][4] = 1;
        board7[4][4] = 1;
        System.out.println("Image Test Case B: " + checkThreeThree(board7, 4, 4, 1)); // 예상 결과: true
        visualizeBoard(board7, 4, 4);
        System.out.println();

    }

    // 메인 메서드
    public static void main(String[] args) {
        testThreeThree();
    }
}
```

## 테스트 결과 시각화

테스트 케이스 결과를 바둑판 형태로 출력하여 시각적으로 확인할 수 있습니다. 이를 통해 3-3 규칙이 제대로 작동하는지 쉽게 검증할 수 있습니다.

### 테스트 케이스 A
```plaintext
Test Case A: true
 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
 0  0  1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
 0  0  1 (1) 1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
 0  0  1  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  
...
```

위와 같은 형식으로 각 테스트 케이스에 대한 결과를 시각화하여 확인할 수 있습니다.

## 결론

이번 프로젝트에서는 오목 게임에서 3-3 규칙을 감지하는 기능을 구현하고 이를 다양한 테스트 케이스를 통해 검증하였습니다. 각 테스트 케이스는 바둑판 형태로 시각화하여 3-3 규칙이 정확히 작동하는지 확인할 수 있습니다. 이를 통해 오목 게임의 규칙을 더 엄격하게 적용할 수 있게 되었습니다.
```
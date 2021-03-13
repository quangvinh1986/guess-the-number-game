# guess-the-number-game
building guess the number game by python language

Trò chơi "Đoán số" là một trò chơi số học với luật chơi rất đơn giản. Có hai người chơi, quản trò viết một số trong phạm vi 0-10 (phạm vi có thể thay đổi) ra giấy, người chơi sẽ đoán một số trong phạm vi trên. Quản trò sẽ trả lời là Đúng hay Sai, nếu đáp án là sai thì quản trò sẽ đưa ra một gợi ý để người chơi đoán tiếp. Trò chơi sẽ dừng lại nếu đoán đúng hoặc đoán sai 3 lần.

Với luật chơi như trên, để có thể xây dựng thành trò chơi trên máy tính, chúng ta sẽ cùng thực hiện phân tích đề bài.

1. Quản trò viết một số trong phạm vi 0-10
Dữ liệu đầu tiên là "quản trò viết một số trong phạm vi 0-10", đây là một số nguyên ngẫu nhiên trong phạm vi 0-10, để sinh ra một số nguyên ngẫu nhiên trong Python, chúng ta sẽ sử dụng thư viện random với function randint(0, 10)

```Python
import random

number = random.randint(0, 10)
print(number)

```

Kết quả sẽ ra như này:
```
>>> import random
>>>
>>> number = random.randint(0, 10)
>>> print(number)
7
>>> number = random.randint(0, 10)
>>> print(number)
5
>>> number = random.randint(0, 10)
>>> print(number)
4
>>> number = random.randint(0, 10)
>>> print(number)
9
>>> number = random.randint(0, 10)
>>> print(number)
6
```

Ngoài function randint, chúng ta có thể sử dụng function .choice như bên dưới
```Python
valid_numbers = [_ for _ in range(0, 11)]
number = random.choice(valid_numbers)
print(number)
```

Kết quả thu được: 
```
>>> valid_numbers = [_ for _ in range(0, 11)]
>>> number = random.choice(valid_numbers)
>>> print(number)
3
>>> valid_numbers
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Ở trên, chúng ta khởi tạo list valid_numbers các số từ 0-10, valid_numbers sẽ dùng để thực hiện xác thực dữ liệu người chơi nhập vào.

2. Người chơi dự đoán số

Để người chơi có thể dự đoán số, chúng ta sẽ cung cấp một giao diện console để nhập dữ liệu vào. Khi người chơi nhập vào, chúng ta sẽ thực hiện xác thực dữ liệu nhập vào bằng cách:
- Kiểm tra dữ liệu mới nhập vào có phải là dạng số không.
- Kiểm tra số vừa được nhập có nằm trong danh sách valid_numbers (mà quản trò và người chơi đã thống nhất từ trước) không.

Với yêu cầu đầu tiên, chúng ta sẽ xây dựng function như này: 
```Python
def guess_number_input():
    guess_number_raw = input("Guess the number: ")
    if guess_number_raw.isnumeric():
        print("correct!")
    else:
        print("wrong number")
```
Kết quả bước đầu:
```
>>> guess_number_input()
Guess the number: 10
correct!
>>> guess_number_input()
Guess the number: 1
correct!
>>> guess_number_input()
Guess the number: 9.5
wrong number
>>> guess_number_input()
Guess the number: 9,5
wrong number
>>> guess_number_input()
Guess the number: invalid
wrong number
```
Với yêu cầu thứ 2, chúng ta sẽ truyền danh sách các số đã xác thực từ trước

```Python
def guess_number_input(valid_numbers):
    guess_number_raw = input("Guess the number: ")
    if guess_number_raw.isnumeric():
        guess_number = int(guess_number_raw)
        if guess_number in valid_numbers:
            return guess_number
        else:
            print("your guess number isn't in valid numbers, please try again!")
            return -1
    else:
        print("wrong number, please try again!")
        return -1
```
```
>>> guess_number_input(valid_numbers)
Guess the number: 10
10
>>> guess_number_input(valid_numbers)
Guess the number: 11
your guess number isn't in valid numbers, please try again!
-1
>>> guess_number_input(valid_numbers)
Guess the number: 8.5
wrong number, please try again!
-1
```

Ở function trên, chúng ta có phần return -1 với các trường hợp nhập sai điều kiện số, dữ liệu này cho phép người chơi có thể retry lại lượt chơi hiện tại (vì số lượng lượt chơi là 3)

Sau khi có số của quản trò và số của người chơi dự đoán, chúng ta sẽ tiếp tục thực hiện việc kiểm tra số, phần này thì khá đơn giản, chỉ cần so sánh 2 số với nhau là được.


Ghép nối các đoạn code lại, chúng ta sẽ có đoạn chương trình như dưới đây:
```Python
import random


def guess_number_input(valid_numbers):
    guess_number_raw = input("Guess the number: ")
    if guess_number_raw.isnumeric():
        guess_number = int(guess_number_raw)
        if guess_number in valid_numbers:
            return guess_number
        else:
            print("your guess number isn't in valid numbers, please try again!")
            return -1
    else:
        print("wrong number, please try again!")
        return -1


def guess_game():
    valid_numbers = [_ for _ in range(0, 11)]
    number_game_master = random.choice(valid_numbers)
    guess_number_player = guess_number_input(valid_numbers)
    if guess_number_player == -1:
        print("retry guess the number")
    elif number_game_master == guess_number_player:
        print(f"Hurray!!! You guessed the number right, it's {guess_number_player}")
    else:
        print(f"Your guess number is incorrect, the number is {number_game_master}")


if __name__ == "main":
    guess_game()
```



3. Xử lý phần cho phép retry và kiểm soát lượt chơi
Số lượng lượt chơi hợp lệ được đưa ra là 3 lượt, chúng ta sẽ đặt một biến để lưu trữ giá trị này

`turn_total = 3`

Và một biến turn_counter để lưu số lượt chơi hợp lệ. Khởi đầu thì turn_counter = 0 Với mỗi lượt chơi hợp lệ, chúng ta sẽ sẽ cộng thêm 1.
Để thực hiện được kiểm soát số lượt chơi, chúng ta sẽ khởi tạo một vòng lặp while (để có thể cho phép người dùng retry nếu nhập sai định dạng số).
Nếu người đùng đoán đúng số, chúng ta sẽ in ra lời chúc và thực hiện thoát khỏi vòng lặp.
Nếu trong suốt quá trình đoán, người chơi không đoán đúng, chúng ta sẽ in ra màn hình số mà quản trò đã đưa ra. Có nhiều phương án để xây dựng việc in ra màn hình này, ví dụ như so sánh turn_counter >= turn_total (đoán sai cả 3 lần) hoặc sử dụng syntax while ... else ... mà tôi đã từng giới thiệu trong bài viết này 
[Từ Khóa Else Trong Python - 3 Điều Có Thể Bạn Chưa Biết?] (https://codelearn.io/sharing/su-dung-else-keyword-trong-python)

function guess_game() sẽ được thay đổi như sau: 

```Python
def guess_game(turn_total):
    valid_numbers = [_ for _ in range(0, 11)]
    number_game_master = random.choice(valid_numbers)
    
    turn_counter = 0
    while turn_counter < turn_total:
        guess_number_player = guess_number_input(valid_numbers)
        if guess_number_player == -1:
            print("retry guess the number")
        elif number_game_master == guess_number_player:
            print(f"Hurray!!! You guessed the number right, it's {guess_number_player}")
            break
        else:
            print("Sorry! Your guess number is incorrect")
            turn_counter += 1
    else:
        print(f"All your guess number is incorrect, the number of game master is {number_game_master}")
```
Đây là kết quả đến hiện tại:
```
Guess the number: 5
Sorry! Your guess number is incorrect
Guess the number: 7
Sorry! Your guess number is incorrect
Guess the number: 9
Sorry! Your guess number is incorrect
All your guess number is incorrect, the number of game master is 6
```
4. Gợi ý cho người chơi

Để gợi ý cho người chơi thì có nhiều cách, cách thông thường là cung cấp cho người chơi về việc so sánh số dự đoán và số của quản trò.
Ví dụ: Số bạn vừa dự đoán lớn hơn / nhỏ hơn số của tôi

Chúng ta sẽ bổ sung function như bên dưới

def suggest_number(guess_number_player, number_game_master):
    if guess_number_player < number_game_master:
        print("Your guess number is less than my number")
    else:
        print("Your guess number is great than my number")

và đặt vào sau dòng lệnh print("Sorry! Your guess number is incorrect")

Đến đây, các bạn có thể bắt đầu trò chơi được rồi.
```
Guess the number: 5
Sorry! Your guess number is incorrect
Your guess number is great than my number
Guess the number: 3
Sorry! Your guess number is incorrect
Your guess number is great than my number
Guess the number: 1
Sorry! Your guess number is incorrect
Your guess number is great than my number
All your guess number is incorrect, the number of game master is 0

Guess the number: 5
Sorry! Your guess number is incorrect
Your guess number is less than my number
Guess the number: 7
Sorry! Your guess number is incorrect
Your guess number is great than my number
Guess the number: 6
Hurray!!! You guessed the number right, it's 6

Guess the number: 5
Sorry! Your guess number is incorrect
Your guess number is less than my number
Guess the number: 8
Sorry! Your guess number is incorrect
Your guess number is less than my number
Guess the number: 9
Sorry! Your guess number is incorrect
Your guess number is less than my number
All your guess number is incorrect, the number of game master is 10
```
Thực sự là một trò chơi...khó nhằn :))
Các bạn có thể giảm bớt độ khó của trò chơi bằng cách bổ sung thêm các gợi ý vào function suggest_number, ví dụ như số của quản trò là số chẵn hay số lẻ, có là số nguyên tố hay không,...

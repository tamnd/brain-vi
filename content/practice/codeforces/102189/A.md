---
title: "CF 102189A - \u041e\u0432\u043e\u0449\u0438"
description: "Món ăn có hai loại rau. Đơn vị đầu tiên đóng góp X đơn vị tâm trạng cho mỗi gam và có A gam trong số đó. Thứ hai đóng góp Y đơn vị tâm trạng trên mỗi gam và có B gam. Sự thay đổi tâm trạng tổng thể chỉ đơn giản là tổng đóng góp của cả hai loại rau: A X + B Y."
date: "2026-08-19T06:16:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "A"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 469
verified: true
draft: false
---

[CF 102189A - \u041e\u0432\u043e\u0449\u0438](https://codeforces.com/problemset/problem/102189/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Món ăn có hai loại rau. Đóng góp đầu tiên`X`đơn vị tâm trạng cho mỗi gam, và có`A`gam của nó. Thứ hai đóng góp`Y`đơn vị tâm trạng trên mỗi gram, và có`B`gram. Sự thay đổi tâm trạng tổng thể chỉ đơn giản là tổng đóng góp của cả hai loại rau:`A * X + B * Y`. 

Dòng đầu tiên đưa ra số tiền`A`Và`B`, trong khi cái thứ hai đưa ra những thay đổi về tâm trạng trên mỗi gam`X`Và`Y`. Đầu ra yêu cầu là một số nguyên, tổng số thay đổi về tâm trạng sau khi ăn toàn bộ món ăn. Câu trả lời tiêu cực nghĩa là tâm trạng giảm sút, trong khi câu trả lời tích cực nghĩa là tâm trạng tăng lên. 

Số lượng`A`Và`B`nhiều nhất là`30000`, Và`X`Và`Y`nằm giữa`-30000`Và`30000`. Ngay cả sự đóng góp tuyệt đối lớn nhất từ ​​một loại rau cũng là`30000 * 30000 = 900000000`, vậy tổng kết quả tuyệt đối nhiều nhất là`1800000000`. Điều này phù hợp với số nguyên 32 bit đã ký, mặc dù số nguyên Python không có vấn đề tràn. Quan trọng hơn, các ràng buộc không yêu cầu bất kỳ phép lặp, tìm kiếm, lập trình động hoặc thuật toán đồ thị nào. Một số phép tính số học không đổi là đủ. 

Các trường hợp cạnh chính đến từ các giá trị đã ký thay vì từ kích thước của đầu vào. Ví dụ, với```
2 3
2 -1
```câu trả lời là`2 * 2 + 3 * (-1) = 1`. Việc thực hiện bất cẩn dẫn đến`Y = -1`như một cường độ dương sẽ tạo ra`7`, mất đi sự thật là loại rau thứ hai làm giảm tâm trạng. 

Trường hợp thứ hai là khi cả hai sự thay đổi tâm trạng đều tiêu cực:```
1 1
-30000 -30000
```Câu trả lời đúng là`-60000`. Phép nhân phải bảo toàn dấu của`X`Và`Y`; lấy giá trị tuyệt đối trước khi tính sẽ cho kết quả sai. 

Zero cũng là một sự thay đổi tâm trạng hợp lệ. Vì```
30000 30000
0 30000
```câu trả lời đúng là`900000000`. Loại rau đầu tiên không đóng góp được gì, mặc dù số lượng của nó khác 0. Bất kỳ cách triển khai nào giả định mọi loại rau đều thay đổi tâm trạng sẽ xử lý sai trường hợp này. 

Cuối cùng, các giá trị lớn nhất có thể tạo ra kết quả gần với ranh giới số nguyên:```
30000 30000
30000 30000
```Câu trả lời đúng là`1800000000`. Việc tính toán phải được thực hiện bằng cách sử dụng loại số nguyên có khả năng biểu thị kết quả đầy đủ. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực trực tiếp sẽ xử lý từng gram riêng biệt. Chúng ta có thể thêm`X`để trả lời`A`lần rồi thêm vào`Y`với nó`B`lần. Điều này đúng vì mỗi gam đóng góp độc lập, vì vậy sau khi tất cả gam được xử lý, bộ tích lũy sẽ chứa chính xác toàn bộ sự thay đổi tâm trạng. 

Vấn đề là điều này thực hiện`A + B`bổ sung. Với cả hai số tiền bằng`30000`, đó là`60000`lần lặp lại. Trong vấn đề cụ thể này, 60000 thao tác vẫn đủ nhỏ để chạy thoải mái trong giới hạn một giây, vì vậy phiên bản bạo lực này thực sự sẽ được chấp nhận. Tuy nhiên, đây là công việc không cần thiết và che khuất cấu trúc toán học đơn giản của bài toán. 

Quan sát quan trọng là tất cả`A`gam của loại rau đầu tiên có tác dụng tương tự,`X`. Thay vì thêm`X`nhiều lần, chúng ta có thể kết hợp những đóng góp giống hệt đó vào sản phẩm`A * X`. Lý luận tương tự cho`B * Y`cho loại rau thứ hai. Vì món ăn chỉ có hai loại rau này nên việc thêm hai sản phẩm này vào sẽ là câu trả lời hoàn chỉnh. 

Điều này biến toàn bộ tính toán thành hai phép nhân và một phép cộng. Brute-force hoạt động vì nó tính tổng rõ ràng mọi đóng góp của từng cá nhân, nhưng không khai thác được thực tế là nhiều đóng góp liên tiếp giống hệt nhau. Nhận thức được sự lặp lại đó cho phép chúng ta thay thế các phép cộng lặp lại bằng phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(A + B) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`A`Và`B`, số lượng của hai loại rau. Các giá trị này xác định số gam của mỗi loại góp phần vào sự thay đổi tâm trạng cuối cùng. 
2. Đọc`X`Và`Y`, sự thay đổi tâm trạng do một gram mỗi loại rau gây ra. Chúng có thể dương, bằng 0 hoặc âm, vì vậy dấu của chúng phải được bảo toàn. 
3. Tính toán`A * X`. Điều này kết hợp sự đóng góp giống hệt nhau của tất cả gam của loại rau đầu tiên thành một phép nhân. 
4. Tính toán`B * Y`cho loại rau thứ hai vì lý do tương tự. 
5. Thêm hai sản phẩm và in kết quả. Hai sản phẩm chiếm từng gam trong món ăn, vì vậy tổng của chúng chính xác là sự thay đổi tâm trạng tổng thể. 

### Tại sao nó hoạt động 

Phần đóng góp của loại rau đầu tiên là tổng của`X`lặp đi lặp lại`A`lần, chính xác là`A * X`. Tương tự, phần đóng góp của loại rau thứ hai là`B * Y`. Mỗi gram thuộc về một trong hai nhóm này nên tổng đóng góp chính xác là`A * X + B * Y`. Thuật toán tính toán trực tiếp biểu thức này, nghĩa là nó không thể bỏ sót một gam hoặc đếm một gam hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())
    X, Y = map(int, input().split())

    print(A * X + B * Y)

if __name__ == "__main__":
    solve()
```Thao tác nhập đầu tiên đọc hai số lượng và thao tác thứ hai đọc hai thay đổi tâm trạng trên mỗi gam.`map(int, ...)`là đủ vì cả bốn giá trị đều là số nguyên, bao gồm cả giá trị âm. 

biểu hiện`A * X + B * Y`là lời giải toán học hoàn chỉnh. Không có vòng lặp, mảng, đệ quy hoặc trường hợp đặc biệt vì số học số nguyên có dấu thông thường đã xử lý chính xác các đóng góp dương, âm và 0. 

Thứ tự thực hiện cũng đơn giản. Mỗi số tiền được nhân với hiệu ứng trên mỗi gram tương ứng trước khi cộng hai khoản đóng góp độc lập. Các số nguyên có độ chính xác tùy ý của Python khiến cho các ràng buộc này không thể tràn được, mặc dù kết quả tối đa đã đủ nhỏ để vừa với số nguyên 32 bit có dấu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
2 3
2 -1
```Thuật toán tính toán phần đóng góp của từng loại rau riêng biệt. 

| A | B | X | Y | A*X | B*Y | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 2 | -1 | 4 | -3 | 1 | 

Loại rau đầu tiên làm tăng tâm trạng`4`, trong khi cái thứ hai hạ thấp nó xuống`3`. Tác dụng kết hợp của chúng là`1`. 

### Mẫu 2 

Đầu vào là:```
1 1
1 -2
```| A | B | X | Y | A*X | B*Y | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | -2 | 1 | -2 | -1 | 

Mỗi loại rau xuất hiện đúng một gram. Việc đầu tiên làm tăng tâm trạng bằng cách`1`, thứ hai giảm nó đi`2`, đưa ra sự thay đổi cuối cùng của`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một số phép tính số học cố định. | 
| Không gian | O(1) | Chỉ có bốn giá trị đầu vào và một kết quả được lưu trữ. | 

Các ràng buộc cho phép một giải pháp thời gian không đổi cực kỳ nhỏ. Ngay cả ở các giá trị tối đa, việc tính toán chỉ bao gồm hai phép nhân và một phép cộng, do đó cả giới hạn thời gian và bộ nhớ đều dễ dàng được đáp ứng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    A, B = map(int, input().split())
    X, Y = map(int, input().split())
    print(A * X + B * Y)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("2 3\n2 -1\n") == "1\n", "sample 1"
assert run("1 1\n1 -2\n") == "-1\n", "sample 2"

# Minimum amounts, both positive contributions
assert run("1 1\n1 1\n") == "2\n", "minimum-size positive case"

# Maximum amounts and maximum positive effects
assert run("30000 30000\n30000 30000\n") == "1800000000\n", "maximum values"

# Maximum amounts and maximum negative effects
assert run("30000 30000\n-30000 -30000\n") == "-1800000000\n", "minimum result"

# Zero contribution and mixed signs
assert run("30000 1\n0 -30000\n") == "-30000\n", "zero and negative contribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`2`| Số tiền tối thiểu và đóng góp tích cực thông thường | 
|`30000 30000 / 30000 30000`|`1800000000`| Giới hạn tối đa và kết quả dương tính lớn nhất | 
|`30000 30000 / -30000 -30000`|`-1800000000`| Giá trị âm và kết quả âm nhất | 
|`30000 1 / 0 -30000`|`-30000`| Không có đóng góp và dấu hiệu hỗn hợp | 

## Vỏ cạnh 

Khi một loại rau làm giảm tâm trạng, sự đóng góp của nó phải duy trì ở mức tiêu cực. Vì```
2 3
2 -1
```sản phẩm đầu tiên là`4`và thứ hai là`-3`, vậy kết quả là`1`. Thuật toán không bao giờ lấy giá trị tuyệt đối hoặc thay đổi dấu, do đó hiệu ứng tiêu cực được xử lý một cách tự nhiên. 

Khi cả hai tác động đều âm, công thức tương tự vẫn được áp dụng. Vì```
1 1
-30000 -30000
```các sản phẩm là`-30000`Và`-30000`, sản xuất`-60000`. Không có nhánh riêng cho các giá trị âm vì phép nhân số nguyên đã áp dụng dấu đúng. 

Hiệu ứng bằng 0 là một trường hợp khác không yêu cầu nhánh đặc biệt. Với```
30000 30000
0 30000
```sản phẩm đầu tiên là`0`, và thứ hai là`900000000`. Kết quả là`900000000`. Thuật toán xử lý chính xác loại rau đầu tiên không ảnh hưởng đến tâm trạng. 

Tại ranh giới dương cực đại,```
30000 30000
30000 30000
```cả hai sản phẩm đều`900000000`, cho`1800000000`. Phép tính sử dụng đầy đủ các giá trị đầu vào mà không có bất kỳ sự điều chỉnh riêng biệt nào, bởi vì`A`gram đóng góp chính xác`A`bản sao của`X`. 

Ranh giới tương tự áp dụng cho cực âm:```
30000 30000
-30000 -30000
```Cả hai sản phẩm đều`-900000000`, vậy câu trả lời là`-1800000000`. Điều này xác nhận rằng việc triển khai sẽ bảo toàn các dấu hiệu và xử lý phạm vi số hoàn chỉnh được phép.

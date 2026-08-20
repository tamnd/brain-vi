---
title: "CF 102163D - Cúp bóng đá"
description: "Kết quả trận đấu chỉ phụ thuộc vào số bàn thắng cuối cùng. Đối với mỗi trường hợp thử nghiệm, X là số bàn thắng mà đội của Bashar ghi được và Y là số bàn thắng mà đội của Hamada ghi được."
date: "2026-08-19T14:43:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "D"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 279
verified: false
draft: false
---

[CF 102163D - Cúp bóng đá](https://codeforces.com/problemset/problem/102163/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Kết quả trận đấu chỉ phụ thuộc vào số bàn thắng cuối cùng. Đối với mỗi trường hợp thử nghiệm,`X`là số bàn thắng mà đội của Bashar ghi được và`Y`là con số mà đội Hamada ghi được. Nhiệm vụ là so sánh hai số nguyên này và in`Bashar`khi`X`lớn hơn,`Hamada`khi`Y`lớn hơn và`Iskandar`khi chúng bằng nhau. 

Câu chuyện gốc chứa nhiều chi tiết về cầu thủ, chấn thương, số lần thay người và thời gian trận đấu, nhưng không có giá trị nào trong số đó ảnh hưởng đến kết quả được yêu cầu. Khi hai điểm cuối cùng được cung cấp, toàn bộ vấn đề sẽ giảm xuống còn một so sánh. 

Mỗi điểm nằm trong khoảng`0`Và`100000`, vì vậy các giá trị vừa khít với các kiểu số nguyên tiêu chuẩn. Quan trọng hơn, phạm vi không cần phải khám phá chút nào. Ngay cả khi có nhiều trường hợp thử nghiệm, một giải pháp thực hiện lượng công việc không đổi cho mỗi trường hợp sẽ dễ dàng đủ nhanh trong giới hạn 1 giây. Một cách tiếp cận dành`O(100000)`Các thao tác trên mọi trường hợp kiểm thử sẽ tốn kém một cách không cần thiết, trong khi mọi phép tính bậc hai trong phạm vi điểm sẽ hoàn toàn không phù hợp. 

Có một vài trường hợp đơn giản trong đó việc triển khai có thể gặp trục trặc. Khi Bashar có nhiều mục tiêu hơn, chẳng hạn như```
1
6 2
```đầu ra đúng là`Bashar`. Việc thực hiện bất cẩn chỉ kiểm tra xem điểm số có bằng nhau hay không có thể coi mọi kết quả không hòa đều giống nhau. 

Khi Hamada có nhiều bàn thắng hơn, chẳng hạn như```
1
1 5
```đầu ra đúng là`Hamada`. Đảo ngược sự so sánh sẽ âm thầm tạo ra người chiến thắng sai. 

Trường hợp bình đẳng cũng rất cần thiết:```
1
3 3
```Đầu ra đúng là`Iskandar`. Việc triển khai chỉ sử dụng`if X > Y`Và`else`sẽ in sai`Hamada`, bởi vì sự bình đẳng phải được xử lý riêng biệt. 

Cuối cùng, số 0 là điểm hợp lệ cho một trong hai đội. Vì```
1
0 0
```kết quả là`Iskandar`, do đó việc so sánh phải có hiệu quả mà không cần giả định rằng một trong hai đội đã ghi bàn ít nhất một lần. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực có chủ ý có thể kiểm tra từng giá trị điểm một và cố gắng xác định điểm nào trong hai điểm đã cho lớn hơn bằng cách quét qua phạm vi có thể từ`0`ĐẾN`100000`. Điều này cuối cùng sẽ đưa ra câu trả lời đúng vì mọi điểm hợp lệ đều nằm trong phạm vi đó, nhưng nó hoàn toàn bỏ qua thực tế là hai giá trị thực tế đã có sẵn. Trong trường hợp xấu nhất, điều này thực hiện`100001`lặp đi lặp lại cho một trường hợp thử nghiệm. Nếu có`100000`trường hợp thử nghiệm, sẽ đạt khoảng`10^10`số lần lặp lại, vượt xa giới hạn 1 giây cho phép. 

Brute-force hoạt động vì cuối cùng nó gặp phải các giá trị điểm có liên quan, nhưng thất bại vì nó dành thời gian khám phá các giá trị không liên quan gì đến câu trả lời. Quan sát quan trọng là việc xác định người chiến thắng không cần phải tìm kiếm gì cả. Mối quan hệ giữa hai điểm số đã cho đã là câu trả lời hoàn chỉnh: lớn hơn nghĩa là đội thắng, nhỏ hơn nghĩa là đội kia thắng và bằng nhau nghĩa là hòa. 

Vì vậy, mỗi trường hợp kiểm thử cần chính xác một so sánh và một quyết định đầu ra. Điều này làm giảm công việc từ việc phụ thuộc vào phạm vi điểm thành một lượng không đổi cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(T * 100000)`|`O(1)`| Quá chậm và không cần thiết | 
| Tối ưu |`O(T)`|`O(T)`cho đầu ra thu thập | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`. Mỗi trường hợp kiểm tra sau đây chứa điểm số cuối cùng của hai đội. 
2. Với mỗi test case, hãy đọc`X`Và`Y`. Không cần mô phỏng trận đấu bóng đá vì đầu vào đã đưa ra kết quả cuối cùng của quá trình tính điểm. 
3. So sánh`X`Và`Y`. Nếu như`X > Y`, đội của Bashar có nhiều mục tiêu hơn nên câu trả lời là`Bashar`. 
4. Nếu`X < Y`, đội Hamada có nhiều bàn thắng hơn nên đáp án là`Hamada`. 
5. Nếu không có điểm nào lớn hơn thì chúng phải bằng nhau. Trong trường hợp đó, in`Iskandar`. 
6. Lưu trữ từng câu trả lời và in tất cả các câu trả lời sau khi xử lý dữ liệu đầu vào. Việc thu thập chuỗi cũng tránh được việc gọi liên tục`print`cho từng trường hợp thử nghiệm riêng lẻ. 

### Tại sao nó hoạt động 

Đối với mỗi trường hợp kiểm thử, có chính xác một trong ba mối quan hệ loại trừ lẫn nhau giữa`X`Và`Y`:`X > Y`,`X < Y`, hoặc`X = Y`. Thuật toán ánh xạ trực tiếp ba khả năng này tới ba kết quả cần có,`Bashar`,`Hamada`, Và`Iskandar`. Vì người chiến thắng chỉ được xác định bằng điểm cuối cùng nào cao hơn nên kết quả được chọn luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        x, y = map(int, input().split())

        if x > y:
            answers.append("Bashar")
        elif x < y:
            answers.append("Hamada")
        else:
            answers.append("Iskandar")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi`T`, cho chương trình biết chính xác có bao nhiêu cặp điểm theo sau. Sau đó, vòng lặp sẽ xử lý một kết quả khớp hoàn chỉnh tại một thời điểm. 

Ba chiều`if`cấu trúc trực tiếp thực hiện thuật toán. Sự so sánh chặt chẽ xử lý hai người có thể chiến thắng, trong khi trận chung kết`else`tượng trưng cho sự bình đẳng. Không cần xử lý đặc biệt điểm 0 hoặc điểm tối đa vì so sánh số nguyên thông thường đã xử lý chính xác cả hai ranh giới. 

Điểm số nhiều nhất là`100000`, do đó, tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Trên thực tế, chương trình không bao giờ thực hiện phép tính về điểm số mà chỉ so sánh. 

Các câu trả lời được tích lũy thành một danh sách và được nối với các ký tự dòng mới ở cuối. Điều này giúp xử lý đầu ra hiệu quả khi số lượng trường hợp thử nghiệm lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét hai trường hợp thử nghiệm đầu tiên từ Mẫu 1. 

| Trường hợp thử nghiệm |`X`|`Y`| So sánh | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 |`1 < 5`|`Hamada`| 
| 2 | 2 | 0 |`2 > 0`|`Bashar`| 

Trong trận đầu tiên, Hamada ghi được 5 bàn thắng vào lưới của Bashar, vì vậy nhánh thứ hai ghi được`Hamada`. Ở trận thứ hai, Bashar có hai bàn thắng trong khi Hamada không có bàn thắng nào, vì vậy nhánh đầu tiên tạo ra`Bashar`. Những trường hợp này thể hiện cả hai hướng so sánh. 

Trường hợp đẳng thức ở Mẫu 1 đưa ra một dấu vết hữu ích khác. 

| Trường hợp thử nghiệm |`X`|`Y`| So sánh | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 |`0 == 0`|`Iskandar`| 
| 2 | 3 | 3 |`3 == 3`|`Iskandar`| 

Không có sự so sánh nghiêm ngặt nào thành công đối với một trong hai hàng. Thuật toán đạt đến trường hợp đẳng thức và kết quả đầu ra`Iskandar`. Điều này xác nhận rằng điểm 0-0 và kết quả hòa tích cực được xử lý giống hệt nhau, theo yêu cầu. 

Hai hàng cuối cùng của Mẫu 1 thực hiện cả hướng ranh giới có điểm số lớn hơn và một chiến thắng thông thường khác. 

| Trường hợp thử nghiệm |`X`|`Y`| So sánh | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 2 |`6 > 2`|`Bashar`| 
| 2 | 2 | 0 |`2 > 0`|`Bashar`| 

Điều bất biến trong mỗi hàng là câu trả lời chỉ được xác định bằng thứ tự của hai điểm hiện tại. Không có thông tin nào từ một trường hợp thử nghiệm khác ảnh hưởng đến quyết định hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(T)`| Mỗi trường hợp kiểm thử thực hiện một lần so sánh và thực hiện công việc bổ sung liên tục | 
| Không gian |`O(T)`| Các chuỗi đầu ra được lưu trữ trước khi in | 

Giới hạn điểm của`100000`không ảnh hưởng đến thời gian chạy vì thuật toán không bao giờ lặp lại các điểm có thể có. Ngay cả một số lượng lớn các trường hợp thử nghiệm chỉ yêu cầu một so sánh thời gian không đổi cho mỗi trường hợp, phù hợp thoải mái với giới hạn 1 giây. Việc sử dụng bộ nhớ cũng nhỏ vì mỗi trường hợp thử nghiệm chỉ đóng góp một chuỗi đầu ra ngắn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    answers = []

    for _ in range(t):
        x, y = map(int, input().split())

        if x > y:
            answers.append("Bashar")
        elif x < y:
            answers.append("Hamada")
        else:
            answers.append("Iskandar")

    sys.stdout.write("\n".join(answers))

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

# Provided sample
assert run("""5
1 5
2 0
0 0
3 3
6 2
""") == """Hamada
Bashar
Iskandar
Iskandar
Bashar""", "sample 1"

# Minimum-size scores
assert run("""1
0 0
""") == "Iskandar", "minimum scores"

# Maximum-size scores
assert run("""2
100000 99999
99999 100000
""") == """Bashar
Hamada""", "maximum scores"

# Several equal scores
assert run("""3
1 1
50000 50000
100000 100000
""") == """Iskandar
Iskandar
Iskandar""", "all equal"

# Boundary and near-boundary comparisons
assert run("""4
0 1
1 0
99999 100000
100000 99999
""") == """Hamada
Bashar
Hamada
Bashar""", "boundary comparisons"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0`|`Iskandar`| Điểm tối thiểu và nhánh bình đẳng | 
|`2 / 100000 99999 / 99999 100000`|`Bashar / Hamada`| Giá trị tối đa cho phép và cả hai hướng so sánh | 
|`3 / 1 1 / 50000 50000 / 100000 100000`|`Iskandar`ba lần | Bình đẳng ở nhiều mức điểm | 
|`4 / 0 1 / 1 0 / 99999 100000 / 100000 99999`|`Hamada / Bashar / Hamada / Bashar`| Giá trị ngay lập tức xung quanh ranh giới và so sánh đảo ngược | 

## Vỏ cạnh 

Để có điểm bằng nhau, hãy xem xét đầu vào```
1
3 3
```Thuật toán kiểm tra đầu tiên`3 > 3`, sai thì kiểm tra`3 < 3`, điều này cũng sai. Khả năng còn lại là đẳng thức nên nó cho ra`Iskandar`. Lý luận tương tự cũng có tác dụng đối với`0 0`Và`100000 100000`. 

Đối với điểm 0 ở một bên, hãy xem xét```
1
0 7
```Sự so sánh đầu tiên,`0 > 7`, là sai. Sự so sánh thứ hai,`0 < 7`, là đúng, vì vậy đầu ra là`Hamada`. Không có trường hợp 0 ​​đặc biệt nào vì số 0 hoạt động bình thường khi so sánh số nguyên. 

Tình huống ngược lại là```
1
7 0
```Đây`7 > 0`là đúng, vì vậy đầu ra là`Bashar`. Kiểm tra cả hai`0 7`Và`7 0`nắm bắt các triển khai vô tình hoán đổi hai biến đầu vào hoặc tên người chiến thắng. 

Ở ranh giới trên,```
1
100000 99999
```cho`Bashar`, trong khi```
1
99999 100000
```cho`Hamada`. Thuật toán thực hiện hai phép so sánh giống như đối với các giá trị nhỏ hơn, do đó không có vấn đề gì xảy ra ở`100000`. 

Trường hợp cạnh trung tâm là bình đẳng, bởi vì việc triển khai hai nhánh như`if X > Y: Bashar else: Hamada`coi sự bình đẳng là chiến thắng của Hamada. Kết quả thứ ba rõ ràng là điều phân biệt một giải pháp đúng đắn với sai lầm phổ biến đó.

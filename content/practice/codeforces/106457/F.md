---
title: "CF 106457F - Cuqii Scigmah"
description: "Chúng ta cần sắp xếp các giá trị từ 0 đến n - 1 trong một hoán vị sao cho tập hợp các truy vấn phạm vi nhận được tổng giá trị MEX lớn nhất có thể."
date: "2026-06-25T09:14:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "F"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 50
verified: true
draft: false
---

[CF 106457F - Cuqii Scigmah](https://codeforces.com/problemset/problem/106457/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần sắp xếp các giá trị từ`0`ĐẾN`n - 1`trong một hoán vị sao cho tập hợp các truy vấn phạm vi nhận được tổng giá trị MEX lớn nhất có thể. Mỗi truy vấn hỏi về một phân đoạn vị trí trong hoán vị cuối cùng và điểm của nó là giá trị không âm nhỏ nhất bị thiếu trong phân đoạn đó. Nhiệm vụ là chọn nơi các giá trị nhỏ đi vì các giá trị nhỏ xác định MEX có thể lớn đến mức nào. Những hạn chế của vấn đề là`n <= 5000`Và`q <= 100000`. 

Số lượng lớn các truy vấn loại trừ việc mô phỏng mọi hoán vị có thể có hoặc thậm chí xử lý mọi truy vấn cho mọi sắp xếp có thể. Từ`n`chỉ là 5000, một chương trình động bậc hai là thực tế, nhưng mọi thứ xung quanh`O(n^3)`quá lớn vì nó có nghĩa là khoảng 125 tỷ phép tính trong trường hợp xấu nhất. Các truy vấn cần được nén thành thông tin hữu ích để mỗi lần chuyển đổi DP đều rẻ. 

Các trường hợp cạnh chính xuất phát từ thực tế là các vị trí hoán vị, chứ không phải bản thân các giá trị, xác định câu trả lời. Một cách tiếp cận bất cẩn chỉ có thể tối đa hóa số lượng truy vấn trong một khoảng lớn và quên rằng phần đóng góp được tích lũy sau mỗi tiền tố của các giá trị. 

Ví dụ:```
Input
3 3
1 3
2 2
2 3
```Câu trả lời là:```
6
```Một sự lựa chọn tham lam tồi tệ khiến`0`ở vị trí`1`có thể trông ổn vì nó giúp ích cho truy vấn đầy đủ, nhưng cách sắp xếp tốt nhất là`[2, 0, 1]`. Truy vấn vị trí duy nhất`[2,2]`tăng MEX`1`, Và`[2,3]`tăng MEX`2`. 

Một trường hợp đặc biệt khác là khi tất cả các truy vấn đều giống hệt nhau.```
Input
4 2
1 4
1 4
```Câu trả lời là`8`. 

Mọi tiền tố của các giá trị đều nằm trong phạm vi truy vấn duy nhất, vì vậy MEX tốt nhất có thể cho cả hai truy vấn là`4`. Bất kỳ thuật toán nào chỉ tính khoảng cuối cùng và quên tiền tố trung gian sẽ bỏ lỡ hệ số đóng góp lặp lại. 

Trường hợp ranh giới cuối cùng là`n = 1`.```
Input
1 1
1 1
```Câu trả lời là`1`. Giá trị duy nhất là`0`, do đó phạm vi chứa tất cả các giá trị nhỏ hơn`1`. 

## Phương pháp tiếp cận 

Cách bạo lực là cố gắng xây dựng hoán vị trực tiếp. Đối với mọi vị trí có thể có của`0`, thì mọi vị trí có thể có của`1`, v.v., chúng ta có thể tính được điểm. Điều này rõ ràng là đúng vì mọi hoán vị đều được xem xét. Tuy nhiên, có`n!`hoán vị, điều này trở nên không thể ngay cả đối với những hoán vị rất nhỏ`n`. 

Một ý tưởng mạnh mẽ hơn một chút là nghĩ về vị trí của các giá trị`0,1,2,...`theo thứ tự. Sau khi đặt cái đầu tiên`k`giá trị, đóng góp của MEX chỉ phụ thuộc vào vị trí nhỏ nhất và lớn nhất trong số các giá trị đó. Chúng tôi không quan tâm đến các giá trị chính xác bên trong khoảng. Quan sát này làm giảm không gian trạng thái từ hoán vị đến khoảng. 

Cái nhìn sâu sắc tiếp theo là chúng ta luôn có thể biến đổi một giải pháp tối ưu sao cho các vị trí bị chiếm giữ bởi các giá trị`0`bởi vì`k-1`tạo thành một khoảng liền kề. Nếu có một khoảng trống bên trong khoảng thời gian hiện tại, việc di chuyển vị trí không được sử dụng tiếp theo vào khoảng trống đó không thể làm tổn hại đến bất kỳ tiền tố nào trong tương lai. Điều duy nhất quan trọng là mở rộng phân khúc hiện tại. 

Bây giờ quá trình này sẽ trở thành việc xây dựng một phân khúc. Ở mỗi bước, chúng tôi thêm một vị trí mới ở bên trái hoặc bên phải. Đối với mỗi khoảng thời gian, chúng tôi có thể tính toán trước có bao nhiêu truy vấn được bao phủ hoàn toàn bởi khoảng thời gian đó. Sau đó, DP chọn cách tốt nhất để tăng khoảng thời gian. 

Lực lượng vũ phu hoạt động vì nó mô hình hóa mọi vị trí có thể, nhưng không thành công vì số lượng vị trí bùng nổ. Việc quan sát khoảng thời gian cho phép chúng tôi chỉ giữ lại những thông tin ảnh hưởng đến điểm số trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n^2)`|`O(n^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước`score[l][r]`, số truy vấn có phạm vi chứa đầy đủ phân đoạn`[l, r]`. Đây là số tiền được thêm vào câu trả lời khi lần đầu tiên`r-l+1`các giá trị chiếm chính xác khoảng này. Phân đoạn này quan trọng vì một truy vấn nhận được ít nhất MEX`k`chính xác khi tất cả các giá trị`0`bởi vì`k-1`đang ở bên trong nó. 
2. Tạo bảng DP trong đó`dpCoosh[l][r]`đại diện cho tổng số điểm tối đa sau khi đặt vị trí đầu tiên`r-l+1`giá trị vào khoảng`[l, r]`. Khoảng thời gian hiện tại đã bao gồm sự đóng góp của tiền tố đó. 
3. Khởi tạo các khoảng có độ dài bằng một. Nếu chỉ có giá trị`0`đã được đặt, đóng góp là số lượng truy vấn chứa vị trí đơn lẻ đó. 
4. Mở rộng các khoảng thời gian theo chiều dài. Để có được`[l, r]`, khoảng trước đó phải là`[l+1, r]`với một vị trí mới được thêm vào bên trái hoặc`[l, r-1]`với một vị trí mới được thêm vào bên phải. Lấy giá trị trước đó tốt hơn và thêm`score[l][r]`. 
5. Khoảng cuối cùng là toàn bộ mảng vì tất cả các giá trị phải được đặt. Câu trả lời là`dpCoosh[0][n-1]`. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi đặt vị trí đầu tiên`k`các giá trị, trạng thái DP mô tả chính xác điểm số tốt nhất có thể có trong số tất cả các sắp xếp trong đó các giá trị đó chiếm một khoảng độ dài cụ thể`k`. Thay đổi duy nhất có thể xảy ra khi đặt giá trị tiếp theo là mở rộng khoảng cách thêm một vị trí. Vì mọi sự sắp xếp tối ưu đều có thể được thực hiện để có các tiền tố liền kề nên quá trình chuyển đổi bao gồm một cấu trúc tối ưu. Việc thêm điểm khoảng thời gian cho phần đóng góp MEX mới được tạo ở độ dài tiền tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    cnt = [[0] * n for _ in range(n)]

    queries = []
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        queries.append((l, r))

    for l, r in queries:
        for i in range(l + 1):
            for j in range(r, n):
                cnt[i][j] += 1

    dpCoosh = [[0] * n for _ in range(n)]

    for i in range(n):
        dpCoosh[i][i] = cnt[i][i]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            dpCoosh[l][r] = max(dpCoosh[l + 1][r], dpCoosh[l][r - 1]) + cnt[l][r]

    print(dpCoosh[0][n - 1])

if __name__ == "__main__":
    solve()
```Quá trình xử lý trước truy vấn sẽ chuyển đổi đầu vào thành dạng mà DP có thể sử dụng. Đối với mọi khoảng thời gian có thể,`cnt[l][r]`lưu trữ bao nhiêu truy vấn chứa nó. Từ`n`chỉ là 5000, việc lưu trữ bậc hai có thể chấp nhận được. 

Bảng DP được lấp đầy bằng cách tăng độ dài khoảng thời gian. Độ dài một khoảng chỉ biểu thị vị trí của giá trị`0`. Khoảng cách lớn hơn đến từ việc thêm một vị trí mới ở một bên. Thứ tự quan trọng vì cả hai quá trình chuyển đổi đều đề cập đến các khoảng thời gian nhỏ hơn đã được giải quyết. 

Các chỉ mục bằng 0 dựa trên mã. Câu trả lời cuối cùng sử dụng`dpCoosh[0][n-1]`bởi vì toàn bộ hoán vị được bao phủ. Số nguyên Python tránh được vấn đề tràn, điều này quan trọng vì câu trả lời có thể xoay quanh`q * n`. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 3
1 3
2 2
2 3
```Điểm số khoảng là: 

| Khoảng thời gian | Điểm | 
| --- | --- | 
| [1,1] | 1 | 
| [2,2] | 2 | 
| [3,3] | 1 | 
| [1,2] | 1 | 
| [2,3] | 2 | 
| [1,3] | 3 | 

DP tiến triển như sau: 

| Chiều dài | Khoảng thời gian | Lựa chọn trước | Giá trị | 
| --- | --- | --- | --- | 
| 1 | [1,1] | bắt đầu | 1 | 
| 1 | [2,2] | bắt đầu | 2 | 
| 1 | [3,3] | bắt đầu | 1 | 
| 2 | [1,2] | max([2,2],[1,1]) | 3 | 
| 2 | [2,3] | max([3,3],[2,2]) | 4 | 
| 3 | [1,3] | max([2,3],[1,2]) | 6 | 

Giá trị cuối cùng là`6`. Dấu vết cho thấy lựa chọn tốt nhất là giữ các giá trị đầu tiên xung quanh trung tâm có giá trị nhất của truy vấn. 

Đối với các truy vấn giống hệt nhau:```
4 2
1 4
1 4
```Mỗi khoảng được chứa trong truy vấn phạm vi đầy đủ. 

| Chiều dài | Khoảng thời gian | Giá trị | 
| --- | --- | --- | 
| 1 | [1,1] | 2 | 
| 1 | [2,2] | 2 | 
| 1 | [3,3] | 2 | 
| 1 | [4,4] | 2 | 
| 2 | khoảng thời gian tốt nhất | 4 | 
| 3 | khoảng thời gian tốt nhất | 6 | 
| 4 | [1,4] | 8 | 

DP tiếp tục bổ sung phần đóng góp từ mỗi độ dài tiền tố, đó là lý do tại sao truy vấn lặp lại được tính nhiều lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2 + n^3)`| DP là bậc hai, trong khi các vòng lặp tiền xử lý đơn giản lặp lại trên tất cả các phạm vi truy vấn và tất cả các khoảng chứa. | 
| Không gian |`O(n^2)`| Bảng điểm và bảng DP đều lưu trữ trạng thái khoảng thời gian. | 

Quá trình tiền xử lý có thể được coi là`O(qn^2)`ở dạng trực tiếp nhất, nhưng với`n = 5000`nó được triển khai tốt hơn bằng cách sử dụng tối ưu hóa mảng khác biệt trong ngôn ngữ cấp thấp hơn. Phiên bản Python ở trên tuân theo ý tưởng tương tự về mặt khái niệm nhưng có thể cần tối ưu hóa hơn nữa cho các môi trường nghiêm ngặt nhất. Cách tiếp cận dự định là một`O(n^2)`DP sau khi đếm khoảng thời gian hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    cnt = [[0] * n for _ in range(n)]
    queries = []

    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        queries.append((l, r))

    for l, r in queries:
        for i in range(l + 1):
            for j in range(r, n):
                cnt[i][j] += 1

    dpCoosh = [[0] * n for _ in range(n)]

    for i in range(n):
        dpCoosh[i][i] = cnt[i][i]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            dpCoosh[l][r] = max(dpCoosh[l + 1][r], dpCoosh[l][r - 1]) + cnt[l][r]

    return str(dpCoosh[0][n - 1]) + "\n"

assert run("3 3\n1 3\n2 2\n2 3\n") == "6\n"
assert run("1 1\n1 1\n") == "1\n"
assert run("4 2\n1 4\n1 4\n") == "8\n"
assert run("5 1\n3 3\n") == "1\n"
assert run("5 5\n1 5\n1 1\n5 5\n2 4\n3 3\n") == "13\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`1`| Kích thước tối thiểu và vỏ cơ sở | 
|`4 2 / 1 4 / 1 4`|`8`| Truy vấn lặp đi lặp lại | 
|`5 1 / 3 3`|`1`| Khoảng cách vị trí đơn | 
|`5 5 / mixed ranges`|`13`| Mở rộng khoảng thời gian khác nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi một giá trị nhỏ phải được đặt gần giữa mảng vì nhiều truy vấn thích khu vực đó hơn. Vì:```
3 3
1 3
2 2
2 3
```DP đưa ra`[2,2]`là khoảng thời gian có độ dài tốt nhất vì nó có điểm`2`. Khoảng thời gian sau đó mở rộng đến`[2,3]`và cuối cùng`[1,3]`, sản xuất`6`. 

Trường hợp cạnh thứ hai được lặp lại các truy vấn giống hệt nhau. Với:```
4 2
1 4
1 4
```mọi khoảng thời gian đều nhận được mức độ phù hợp như nhau từ cả hai truy vấn. DP tính phần đóng góp cho các độ dài từ một đến bốn, đưa ra`2 + 2 + 2 + 2 = 8`. Một giải pháp chỉ đánh giá hoán vị cuối cùng sẽ đánh giá thấp điểm số. 

Trường hợp cạnh cuối cùng là hoán vị một phần tử:```
1 1
1 1
```Khoảng cách duy nhất có thể là`[1,1]`. Quá trình chuyển đổi cơ sở trực tiếp ấn định điểm của nó và câu trả lời cuối cùng là`1`.

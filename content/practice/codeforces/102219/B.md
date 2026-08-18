---
title: "CF 102219B - SpongeBob SquarePants"
description: "Mỗi trường hợp thử nghiệm mô tả một hình có bốn cạnh với các góc vuông sử dụng chiều rộng w và chiều cao h. Vì mỗi hình như vậy đều là hình chữ nhật nên câu hỏi duy nhất là liệu nó có phải là loại hình chữ nhật đặc biệt có hai cạnh dài bằng nhau hay không."
date: "2026-08-17T22:46:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "B"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 156
verified: false
draft: false
---

[CF 102219B - SpongeBob SquarePants](https://codeforces.com/problemset/problem/102219/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hình có bốn cạnh với các góc vuông sử dụng chiều rộng của nó`w`và chiều cao`h`. Vì mỗi hình như vậy đều là hình chữ nhật nên câu hỏi duy nhất là liệu nó có phải là loại hình chữ nhật đặc biệt có hai cạnh dài bằng nhau hay không. 

Nếu như`w == h`, hình là hình vuông nên đáp án cần tìm là`YES`. Nếu hai kích thước khác nhau thì hình đó là hình chữ nhật thông thường, vì vậy câu trả lời là`NO`. 

Kích thước là số nguyên dương giữa`1`Và`1,000,000`. Bản thân các giá trị đủ nhỏ để vừa vặn thoải mái với số nguyên Python, do đó không có vấn đề tràn. Quan trọng hơn, độ lớn thực tế của các kích thước hoàn toàn không cần ảnh hưởng đến thuật toán. Quyết định chỉ phụ thuộc vào một so sánh bình đẳng, đưa ra công việc không đổi cho mỗi trường hợp thử nghiệm. Ngay cả khi số lượng ca kiểm thử lớn, việc quét tuyến tính qua các ca kiểm thử là giới hạn tự nhiên vì mọi cặp kích thước đều phải được đọc và phân loại. 

Các trường hợp đặc biệt rất đơn giản nhưng vẫn đáng được xử lý một cách rõ ràng. Hình vuông nhỏ nhất có thể là`1 1`, phải tạo ra`YES`. Việc triển khai bất cẩn để kiểm tra xem kích thước có lớn hơn một kích thước hay không có thể từ chối nó một cách không chính xác. 

Một hình có thể có kích thước rất lớn trong khi vẫn là hình vuông. Ví dụ,`1000000 1000000`sản xuất`YES`. Việc triển khai vô tình sử dụng một giới hạn cố định nhỏ hoặc xử lý đặc biệt giá trị tối đa sẽ không thành công ngay cả khi sự bình đẳng là tất cả những gì quan trọng. 

Thứ tự của kích thước không quan trọng. Ví dụ,`3 7`Và`7 3`đều là hình chữ nhật và đều tạo ra`NO`. Một sự so sánh như`w < h`hoặc`w > h`riêng nó không xác định được hình dạng đó có phải là hình vuông hay không, bởi vì có thể sắp xếp theo một trong hai cách. 

Cuối cùng, các kích thước chỉ khác nhau một vẫn phải bị từ chối. Vì`5 6`, đầu ra đúng là`NO`. Việc triển khai sử dụng điều kiện riêng lẻ như`abs(w - h) <= 1`sẽ phân loại không chính xác nó thành hình vuông. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể tưởng tượng hình chữ nhật như một tập hợp các ô đơn vị và kiểm tra toàn bộ hình dạng trước khi quyết định xem nó có phải là hình vuông hay không. Đối với một`w × h`hình chữ nhật, có nghĩa là kiểm tra`w * h`tế bào. Cách tiếp cận này đúng vì kích thước hoàn toàn xác định lưới hình chữ nhật, vì vậy sau khi xử lý từng ô, chúng ta có thể so sánh chiều rộng và chiều cao thu được. Tuy nhiên, với cả hai chiều bằng`1,000,000`, điều này đòi hỏi chính xác`1,000,000,000,000`kiểm tra tế bào cho một trường hợp thử nghiệm. Điều đó vượt xa những gì một chương trình cuộc thi một giây có thể thực hiện được. 

Brute-force hoạt động vì cuối cùng nó xử lý tất cả thông tin mô tả hình dạng, nhưng nó thất bại vì hầu như tất cả công việc đó đều không liên quan. Điều kiện bình phương không phụ thuộc vào bất kỳ ô riêng lẻ nào. Nó phụ thuộc trực tiếp vào độ dài hai bên. Việc quan sát thấy một hình chữ nhật là hình vuông chính xác khi chiều rộng của nó bằng chiều cao sẽ làm giảm toàn bộ bài toán thành một phép so sánh số nguyên. 

Đối với mỗi trường hợp thử nghiệm, hãy đọc`w`Và`h`, so sánh chúng và in`YES`khi chúng bằng nhau và`NO`nếu không thì. Không có sự lặp lại về kích thước và không có cấu trúc hình học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(w × h) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`, bởi vì quyết định tương tự phải được đưa ra một cách độc lập cho mọi hình dạng. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc chiều rộng`w`và chiều cao`h`. 
3. So sánh`w`Và`h`. Bình đẳng chính xác là điều kiện toán học để hình chữ nhật là hình vuông, do đó không cần tính toán hình học nào khác. 
4. In`YES`nếu như`w == h`; nếu không thì in`NO`. Mỗi trường hợp kiểm thử tạo ra chính xác một dòng đầu ra, duy trì sự tương ứng cần thiết giữa hình dạng đầu vào và câu trả lời. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thuộc tính xác định của hình vuông: chiều rộng và chiều cao của nó bằng nhau. Đối với mọi trường hợp thử nghiệm, nếu thuật toán in`YES`, sau đó`w == h`, do đó hình có độ dài các cạnh bằng nhau và là hình vuông. Nếu nó in`NO`, sau đó`w != h`, do đó độ dài các cạnh khác nhau và hình dạng không thể là hình vuông. Hai trường hợp này bao gồm mọi cặp kích thước dương có thể có, do đó thuật toán không thể phân loại sai đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        w, h = map(int, input().split())
        print("YES" if w == h else "NO")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên được đọc là`t`, điều khiển chính xác số lượng cặp thứ nguyên được xử lý. Điều này tránh việc dựa vào hành vi ở cuối tệp và khớp với định dạng đầu vào. 

Bên trong vòng lặp,`map(int, input().split())`chuyển đổi hai chiều trực tiếp thành số nguyên. Số nguyên Python xử lý một cách an toàn giá trị tối đa đã cho của`1,000,000`, do đó không cần kiểu số đặc biệt hoặc xử lý tràn. 

Biểu thức điều kiện thực hiện phép so sánh tương tự được mô tả trong thuật toán. Không có trường hợp ranh giới nào liên quan đến các vòng lặp trên các kích thước, do đó không có điều kiện riêng lẻ nào cần quản lý. Sự so sánh bình đẳng cũng xử lý`1 1`Và`1000000 1000000`đúng, không có trường hợp đặc biệt nào. 

sử dụng`sys.stdin.readline`tuân theo mẫu nhập nhanh được yêu cầu. Đối với vấn đề cụ thể này, việc phân tích cú pháp đầu vào vốn đã rất nhỏ so với hầu hết các tác vụ lập trình cạnh tranh, nhưng cách tiếp cận này phù hợp khi có thể có nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bốn trường hợp thử nghiệm được xử lý độc lập. 

| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 9 | Đúng |`YES`| 
| 2 | 16 | 30 | Sai |`NO`| 
| 3 | 200 | 33 | Sai |`NO`| 
| 4 | 547 | 547 | Đúng |`YES`| 

Hình thứ nhất và thứ tư có kích thước bằng nhau nên chúng là hình vuông. Hai cái ở giữa có kích thước khác nhau nên chúng là những hình chữ nhật không đủ tiêu chuẩn. Dấu vết thể hiện tính bất biến trung tâm: đầu ra được xác định hoàn toàn bởi sự bằng nhau của cặp hiện tại. 

### Đã thi công mẫu 2 

Hãy xem xét đầu vào:```
4
1 1
1 2
999999 1000000
1000000 1000000
```| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Đúng |`YES`| 
| 2 | 1 | 2 | Sai |`NO`| 
| 3 | 999999 | 1000000 | Sai |`NO`| 
| 4 | 1000000 | 1000000 | Đúng |`YES`| 

Dấu vết này thực hiện cả hai đầu của phạm vi cho phép và cũng kiểm tra trường hợp các kích thước khác nhau chính xác một. Không cần xử lý đặc biệt đối với bất kỳ trường hợp nào trong số đó vì cùng một bài kiểm tra tính bằng nhau được áp dụng thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một phép so sánh sau khi đọc hai số nguyên. | 
| Không gian | O(1) | Chỉ có kích thước hiện tại và trạng thái vòng lặp được lưu trữ. | 

Kích thước có thể lớn như`1,000,000`, nhưng giá trị của chúng không làm tăng số lượng tính toán. Ngay cả đối với một số lượng lớn các trường hợp thử nghiệm, thuật toán chỉ thực hiện công việc không đổi cho mỗi trường hợp, do đó tổng thời gian chạy của nó tăng tuyến tính với kích thước đầu vào. Mức sử dụng bộ nhớ không đổi và thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        w, h = map(int, input().split())
        print("YES" if w == h else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run(
    """4
9 9
16 30
200 33
547 547
"""
) == """YES
NO
NO
YES
""", "sample 1"

assert run(
    """4
1 1
1 2
999999 1000000
1000000 1000000
"""
) == """YES
NO
NO
YES
""", "minimum and maximum boundaries"

assert run(
    """5
1 1
2 2
5 5
100 100
1000000 1000000
"""
) == """YES
YES
YES
YES
YES
""", "all equal values"

assert run(
    """5
1 2
2 1
999999 1000000
1000000 999999
5 6
"""
) == """NO
NO
NO
NO
NO
""", "off-by-one and reversed dimensions"

assert run(
    """3
1000000 1
1 1000000
999998 1000000
"""
) == """NO
NO
NO
""", "large unequal dimensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`,`1000000 1000000`|`YES`,`YES`| Kích thước hình vuông hợp lệ tối thiểu và tối đa | 
| Một số cặp bằng nhau | Tất cả`YES`| Bình đẳng là đủ bất kể tầm quan trọng | 
|`1 2`,`2 1`,`5 6`| Tất cả`NO`| Kích thước đảo ngược và sự khác biệt từng cái một | 
|`1000000 1`,`1 1000000`| Cả hai`NO`| Kích thước lớn không bằng nhau ở các hướng ngược nhau | 

## Vỏ cạnh 

Đối với hình dạng có kích thước tối thiểu, đầu vào`1 1`so sánh thuật toán`1 == 1`, điều này đúng nên nó in ra`YES`. Không có lý do hình học nào để loại trừ một hình vuông có cạnh dài bằng 1 và thuật toán chấp nhận nó một cách chính xác mà không cần điều kiện đặc biệt. 

Đối với hình vuông có kích thước tối đa, đầu vào`1000000 1000000`cũng sản xuất`YES`. Việc so sánh vẫn là một phép toán số nguyên trong thời gian không đổi bất kể kích thước số của các giá trị, do đó giới hạn trên không tạo ra vấn đề về hiệu suất hoặc độ chính xác. 

Đối với các kích thước khác nhau một, đầu vào`5 6`cho`5 == 6`, đó là sai, vì vậy đầu ra là`NO`. Thuật toán không nhầm lẫn các thứ nguyên "gần như bằng nhau" với các thứ nguyên bằng nhau, tránh được lỗi thường gặp. 

Đối với kích thước đảo ngược,`7 3`sản xuất`NO`bởi vì`7 != 3`, Và`3 7`cũng sản xuất`NO`bởi vì`3 != 7`. Hình vuông có kích thước bằng nhau theo cả hai hướng, do đó không có lý do gì để chuẩn hóa cặp bằng cách sử dụng`min`Và`max`trước khi so sánh nó. 

Thuật toán xử lý mọi trường hợp cạnh thông qua cùng một bất biến, cụ thể là`YES`được in chính xác khi độ dài hai cạnh bằng nhau. Không cần tính toán hình học bổ sung hoặc các nhánh trường hợp đặc biệt.

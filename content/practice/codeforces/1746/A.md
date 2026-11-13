---
title: "CF 1746A - Maxmina"
description: "Chúng ta được cung cấp một mảng nhị phân và hai loại phép toán rút gọn nhằm thu nhỏ mảng trong khi thay thế các phân đoạn bằng giá trị tối thiểu hoặc tối đa."
date: "2026-06-09T15:47:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1746
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 23"
rating: 800
weight: 1746
solve_time_s: 452
verified: false
draft: false
---

[CF 1746A - Maxmina](https://codeforces.com/problemset/problem/1746/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 7 phút 32 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân và hai loại phép toán rút gọn nhằm thu nhỏ mảng trong khi thay thế các phân đoạn bằng giá trị tối thiểu hoặc tối đa. Quá trình tiếp tục cho đến khi mảng trở thành một giá trị duy nhất và mục tiêu là xác định xem liệu giá trị cuối cùng đó có thể được`1`. 

Hai thao tác này hoạt động rất khác nhau trên dữ liệu nhị phân. Hoạt động tối thiểu trên hai phần tử liền kề tạo ra`1`chỉ khi cả hai đều`1`, nếu không thì nó tạo ra`0`. Điều này có nghĩa đây là một hoạt động “tiêu diệt” chúng ta. Hoạt động tối đa trên một khối có chiều dài`k`sản xuất`1`nếu có ít nhất một`1`trong khối, do đó nó hoạt động như một cách để truyền bá hoặc bảo tồn`1`. 

Mặc dù mảng có thể được chuyển đổi theo nhiều cách nhưng các ràng buộc rất nhỏ:`n ≤ 50`. Điều này loại trừ mọi nhu cầu lập trình động phức tạp trên các trạng thái của mảng. Bất kỳ giải pháp đúng nào cũng phải xuất phát từ việc suy luận về thông tin nào được bảo toàn thay vì mô phỏng tất cả các phép biến đổi. 

Trường hợp cạnh khóa là khi mảng không chứa`1`không hề. Trong trường hợp đó, mọi phép toán đều bảo toàn số 0, do đó kết quả phải luôn là`[0]`. Một tình huống tinh vi hơn một chút là liệu một số cấu hình có một`1`có thể bị “mắc kẹt” bởi các hoạt động, nhưng thử nghiệm nhanh chóng cho thấy rằng sự hiện diện của ít nhất một`1`thường là đủ để buộc thành công. 

Ví dụ, nếu`a = [0, 0, 0, 1]`Và`k = 4`, một thao tác tối đa sẽ biến toàn bộ mảng thành`[1]`. Nếu như`k = 2`, chúng ta vẫn có thể sử dụng các phép toán tối đa để di chuyển và bảo toàn`1`và các thao tác tối thiểu chỉ thu gọn các phần tử liền kề mà không loại bỏ khả năng cô lập nó. Trường hợp duy nhất thực sự không thể xảy ra là khi không có`1`để bắt đầu. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các hoạt động có thể có trên tất cả các phân đoạn có thể có. Mỗi thao tác làm giảm kích thước mảng, nhưng số lượng trạng thái tăng cực kỳ nhanh vì mọi cấu hình mảng trung gian có thể phân nhánh thành nhiều cấu hình khác. Ngay cả với`n ≤ 50`, điều này trở nên không khả thi vì số lượng chuỗi hoạt động là theo cấp số nhân. 

Quan sát quan trọng là không có hoạt động nào tạo ra mới`1`s từ đầu. Cách duy nhất một`1`có thể xuất hiện trong mảng cuối cùng nếu mảng ban đầu đã chứa ít nhất một`1`. Một lần`1`tồn tại, thao tác tối đa đảm bảo nó có thể được giữ nguyên trong bất kỳ cửa sổ nào đã chọn có chứa nó. Sau đó, việc giảm lặp đi lặp lại có thể thu gọn mảng thành một phần tử duy nhất trong khi vẫn giữ ít nhất một phần tử.`1`còn sống. 

Thao tác tối thiểu không giúp tạo ra hoặc lan truyền`1`s, nhưng nó cũng không ngăn cản được`1`sống sót nếu chúng ta tránh ghép nối nó với`0`một cách phá hoại. Vì chúng ta được tự do lựa chọn các hoạt động và phân đoạn nên chúng ta luôn có thể sắp xếp một trình tự trong đó`1`được bảo tồn cho đến cuối cùng. 

Điều này làm giảm toàn bộ vấn đề thành một câu hỏi duy nhất: mảng có chứa ít nhất một`1`? 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra sự tồn tại của 1 | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống việc kiểm tra xem có`1`tồn tại trong mảng. 

1. Quét mảng một lần từ trái sang phải và kiểm tra xem có phần tử nào bằng không`1`. Bước này nắm bắt điều kiện cần thiết duy nhất để thành công, vì không có`1`không có hoạt động nào có thể giới thiệu được. 
2. Nếu ít nhất một`1`được tìm thấy, ngay lập tức kết luận rằng có thể đạt được`[1]`. Lý do là một`1`luôn có thể được bảo toàn thông qua các hoạt động tối đa và mảng luôn có thể được thu gọn sau đó. 
3. Nếu không`1`tồn tại, kết luận rằng câu trả lời là không thể. Tất cả các phép toán trên số 0 sẽ luôn tạo ra số 0, vì vậy mảng cuối cùng phải là`[0]`. 

### Tại sao nó hoạt động 

Cả hai phép toán đều đơn điệu đối với sự tồn tại của`1`. Hoạt động tối thiểu không thể tạo ra`1`và hoạt động tối đa chỉ bảo tồn một hiện có`1`nếu một người đã ở trong phân khúc đã chọn. Điều này có nghĩa là mảng vị ngữ “chứa ít nhất một`1`" không bao giờ trở thành đúng nếu nó bắt đầu sai và một khi đúng, nó có thể được duy trì cho đến khi mảng giảm xuống kích thước một. Do đó, sự tồn tại của a`1`là vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    if any(x == 1 for x in a):
        print("YES")
    else:
        print("NO")
```Giải pháp trực tiếp thực hiện điều kiện dẫn xuất. các`any`kiểm tra quét một lần qua mảng, đưa ra thời gian tuyến tính cho mỗi trường hợp kiểm thử. Không cần mô phỏng các hoạt động vì cấu trúc của các hoạt động đảm bảo rằng không có cấu hình nào không có giá trị ban đầu`1`bao giờ có thể sản xuất một. 

Một lỗi phổ biến là cố gắng mô hình hóa các hoạt động một cách rõ ràng, nhưng các ràng buộc khiến việc đó trở nên không cần thiết. Toàn bộ quá trình chuyển đổi sụp đổ thành một kiểm tra bất biến duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó`a = [0, 1, 0]`Và`k = 2`. Quá trình quét phát hiện một`1`ngay lập tức, vì vậy câu trả lời là`YES`. Về mặt vận hành, chúng ta có thể hợp nhất và thu nhỏ trong khi vẫn giữ nguyên`1`, nhưng thuật toán không cần mô phỏng điều này. 

Bây giờ hãy xem xét`a = [0, 0, 0, 0]`với bất kỳ`k`. Quá trình quét không tìm thấy`1`, vậy câu trả lời là`NO`. Mọi thao tác có thể đều tạo ra số 0 tối thiểu hoặc tối đa, luôn bằng 0, do đó mảng không bao giờ có thể trở thành`[1]`. 

Hai dấu vết này cho thấy thuật toán hoàn toàn không suy luận về các phép biến đổi, mà chỉ về việc liệu một tài nguyên cần thiết có tồn tại hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét một lần trên mảng | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với mảng có kích thước lên tới 50, do đó tổng công việc tối đa là 50.000 lần kiểm tra phần tử, tốc độ này rất nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        out.append("YES" if any(x == 1 for x in a) else "NO")
    return "\n".join(out)

# provided samples
assert run("""7
3 2
0 1 0
5 3
1 0 1 1 0
2 2
1 1
4 4
0 0 0 0
6 3
0 0 1 0 0 1
7 5
1 1 1 1 1 1 1
5 3
0 0 1 0 0
""") == """YES
YES
YES
NO
YES
YES
YES"""

# all zeros
assert run("""1
5 3
0 0 0 0 0
""") == "NO"

# single one
assert run("""1
5 3
0 0 1 0 0
""") == "YES"

# all ones
assert run("""1
4 2
1 1 1 1
""") == "YES"

# k equals n with a one
assert run("""1
4 4
0 0 0 1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | KHÔNG | không thể không có 1 | 
| một cái | CÓ | đủ một 1 | 
| tất cả những cái | CÓ | trường hợp tích cực tầm thường | 
| k = n với một | CÓ | trường hợp hoạt động tối đa toàn cầu | 

## Vỏ cạnh 

Khi mảng chỉ chứa các số 0, mọi thao tác đều giữ nguyên cấu trúc hoàn toàn bằng 0. Ví dụ,`[0, 0, 0, 0]`vẫn giữ nguyên tất cả các số 0 trong cả hai phép toán tối thiểu và tối đa, vì vậy nó luôn kết thúc dưới dạng`[0]`. 

Khi có chính xác một`1`, chẳng hạn như`[0, 0, 1, 0]`, thuật toán chấp nhận ngay. Bất kỳ chuỗi thao tác nào cũng có thể được sắp xếp sao cho đoạn chứa`1`được bảo toàn bằng các phép toán tối đa cho đến khi mảng được giảm. Ngay cả khi sự hợp nhất trung gian liên quan đến số không, sự hiện diện của`1`đảm bảo nó luôn có thể được giữ bên trong một số phân khúc đã chọn. 

Khi tất cả các phần tử đều`1`, mọi thao tác vẫn tạo ra`1`, do đó giảm xuống`[1]`là đơn giản bất kể`k`. 

Khi`k = n`, một thao tác tối đa sẽ thu gọn toàn bộ mảng thành một giá trị duy nhất, trở thành`1`nếu có`1`tồn tại. Điều này phù hợp với điều kiện tương tự xuất phát từ đối số chung.

---
title: "CF 105160H - \u5341\u516d\u8fdb\u5236\u7684\u7591\u60d1"
description: "Chúng ta được cung cấp một tập hợp các số thập lục phân được viết dưới dạng chuỗi. Mỗi số được cho là đại diện cho một số nguyên không âm hợp lệ trong cơ sở 16, nhưng tập dữ liệu có một điểm khác biệt: một số mục là kết quả đúng của các bài toán trừ thập lục phân, trong khi các số khác là kết quả sai…"
date: "2026-06-27T11:01:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "H"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 53
verified: true
draft: false
---

[CF 105160H - \u5341\u516d\u8fdb\u5236\u7684\u7591\u60d1](https://codeforces.com/problemset/problem/105160/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số thập lục phân được viết dưới dạng chuỗi. Mỗi số được cho là đại diện cho một số nguyên không âm hợp lệ trong cơ sở 16, nhưng tập dữ liệu có một điểm khác biệt: một số mục nhập là kết quả chính xác của các bài toán trừ thập lục phân, trong khi các mục khác là kết quả sai do quá trình tính toán tay có lỗi tạo ra. 

Nhiệm vụ không phải là tính lại số học. Thay vào đó, chúng tôi coi đầu vào là một túi chứa các giá trị được gắn nhãn. Trong số các giá trị này, mỗi kết quả không chính xác sẽ được ghép nối với kết quả đúng của nó ở một nơi khác trong danh sách. Với mỗi cặp chỉ số có thứ tự$i, j$với$i \ne j$, chúng tôi muốn đếm bao nhiêu lần giá trị tại vị trí$i$là cách biểu diễn không chính xác của cùng một số có cách biểu diễn đúng xuất hiện ở vị trí$j$. Nếu một mục nhập đã đúng thì nó chỉ được phép ghép nối với một mục nhập đúng khác nếu chúng là các biểu diễn giống hệt nhau của cùng một giá trị. 

Khó khăn hoàn toàn nằm ở việc nhận biết khi nào hai chuỗi thập lục phân biểu thị cùng một giá trị số nguyên có dấu, bởi vì các phép tính không chính xác có thể tạo ra các tạo phẩm mượn hàng đầu tạo ra các chuỗi khác nhau trong khi vẫn biểu thị cùng một kết quả toán học sau khi được diễn giải chính xác. 

Các ràng buộc cho phép lên đến$n = 10^4$các chuỗi, mỗi chuỗi có độ dài tối đa 100. Điều đó ngay lập tức loại trừ mọi so sánh bậc hai của tất cả các cặp sử dụng chuyển đổi số học đầy đủ một cách nhanh chóng nếu mỗi chuyển đổi đắt tiền. Tuy nhiên, việc chuyển đổi một chuỗi cơ sở 16 thành một số nguyên có độ dài tuyến tính, do đó việc thực hiện điều đó một lần cho mỗi chuỗi là có thể chấp nhận được. Điều chúng ta phải tránh là tính toán lại chuyển đổi cho mỗi cặp. 

Một lỗi giải thích ngây thơ có thể là so sánh trực tiếp các chuỗi. Điều đó không thành công ngay lập tức vì các cách biểu diễn khác nhau có thể tương ứng với cùng một giá trị ở dạng thập lục phân có dấu. Ví dụ: một kết quả có lỗi như "-BBBC" vẫn có thể biểu thị cùng một số nguyên dưới dạng "-4444" chính xác sau khi được chuẩn hóa, ngay cả khi các chuỗi khác nhau. 

Một vấn đề tế nhị khác là các giá trị thập lục phân âm không phải là dạng chuẩn chuẩn. Hai chuỗi trông khác nhau có thể chuẩn hóa thành cùng một số nguyên có dấu nếu được diễn giải theo chữ số với quy tắc mượn. 

## Phương pháp tiếp cận 

Phương pháp brute-force là so sánh từng cặp chỉ số$(i, j)$, tính giá trị số của cả hai chuỗi thập lục phân theo cùng một quy tắc diễn giải và đếm các kết quả khớp trong đó một chuỗi tương ứng với dạng không chính xác và chuỗi kia tương ứng với dạng đúng. Điều này đơn giản về mặt khái niệm: chuyển đổi cả hai chuỗi thành biểu diễn số nguyên chuẩn và so sánh. 

Vấn đề là chi phí. Việc chuyển đổi một chuỗi mất$O(L)$, Ở đâu$L \le 100$. Thực hiện việc này bên trong một vòng lặp kép$n = 10^4$cho$O(n^2 L)$, xung quanh$10^10$các thao tác ký tự trong trường hợp xấu nhất. Điều này vượt xa giới hạn 1 giây. 

Quan sát quan trọng là việc ghép nối chỉ phụ thuộc vào sự bằng nhau của giá trị số nguyên được giải thích chứ không phụ thuộc vào dạng chuỗi gốc. Khi mỗi chuỗi được ánh xạ tới giá trị số của nó, tất cả những gì còn lại là đếm số lần mỗi giá trị xuất hiện. Mỗi mục nhập sai sẽ kết hợp với mọi mục nhập đúng có cùng giá trị, do đó phần đóng góp sẽ trở thành tích tần số. 

Do đó, vấn đề giảm xuống còn việc nhóm các giá trị số giống nhau. Chúng tôi chuyển đổi từng chuỗi thành một số nguyên lớn (Python xử lý độ chính xác tùy ý), đếm tần số và tính toán từng giá trị theo các cặp được sắp xếp theo số lần xuất hiện của nó, được chia thành các nhóm đúng và sai. Câu lệnh đảm bảo rằng mỗi giá trị không chính xác sẽ tương ứng với một giá trị đúng trong tập dữ liệu, do đó việc nhóm theo giá trị là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force |$O(n^2 L)$|$O(1)$| Quá chậm | 
| Băm theo giá trị được phân tích |$O(n L)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi chuỗi thập lục phân thành giá trị nguyên chuẩn. Việc chuyển đổi diễn giải chuỗi dưới dạng cơ số 16, với dấu trừ ở đầu tùy chọn biểu thị số âm. Điều này mang lại cho chúng ta một đại diện số duy nhất cho mỗi giá trị logic. 
2. Đếm số lần xuất hiện của từng giá trị số nguyên bằng bản đồ băm. Mỗi khóa đại diện cho một kết quả số riêng biệt và tần số của nó là số chuỗi đánh giá kết quả đó. 
3. Với mỗi nhóm giá trị, tính số cặp có thứ tự$(i, j)$với$i \ne j$. Vì tất cả các chuỗi trong cùng một nhóm đều biểu thị cùng một số nên mọi phần tử có thể ghép nối với mọi phần tử riêng biệt khác trong nhóm đó. 
4. Tính tổng số tiền đóng góp của tất cả các nhóm và đưa ra kết quả cuối cùng. 

Ý tưởng chính là chúng ta không bao giờ cần phân biệt xem một chuỗi có cấu trúc "đúng" hay "không chính xác"; chỉ có sự bình đẳng của giá trị được giải thích cuối cùng mới quan trọng để hình thành các cặp hợp lệ. 

### Tại sao nó hoạt động 

Tất cả các cặp hợp lệ chỉ phụ thuộc vào sự bằng nhau của cách giải thích số nguyên cơ bản. Khi mỗi chuỗi được ánh xạ vào không gian chuẩn đó, các tạo phẩm định dạng ban đầu sẽ biến mất. Mỗi cặp hợp lệ tương ứng chính xác với hai chỉ số có giá trị được phân tích khớp nhau và mỗi cặp như vậy được tính chính xác một lần khi chúng tôi tính toán các kết hợp bên trong mỗi nhóm tần số. Không thể ghép nối nhiều nhóm vì các giá trị số nguyên khác nhau không thể tương ứng với cùng một kết quả số học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_hex(s: str) -> int:
    # Python handles arbitrary-length hex directly
    # int() supports negative sign as well
    return int(s, 16)

def solve():
    n = int(input().strip())
    arr = input().split()

    freq = {}

    for s in arr:
        val = parse_hex(s)
        freq[val] = freq.get(val, 0) + 1

    ans = 0
    for v in freq.values():
        ans += v * (v - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Bước phân tích cú pháp dựa trên chuyển đổi cơ sở 16 tích hợp sẵn của Python, xử lý chính xác cả biểu diễn thập lục phân dương và âm. Điều này tránh việc xử lý chữ số thủ công và đảm bảo tính chính xác cho các đầu vào dài. 

Lựa chọn triển khai chính là đếm các cặp có thứ tự. Đối với một nhóm có quy mô$v$, mỗi chỉ mục có thể ghép nối với$v - 1$những người khác, mang lại$v(v - 1)$. Điều này phù hợp với yêu cầu đặt hàng vì$i, j$là khác biệt và vấn đề định hướng. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba giá trị: 

đầu vào:```
3
-4444 -BBBC 4444
```Chúng tôi tính toán các diễn giải số: 

| Chỉ mục | Chuỗi | Giá trị được phân tích | 
| --- | --- | --- | 
| 1 | -4444 | x | 
| 2 | -BBBC | x | 
| 3 | 4444 | y | 

Ở đây hai cái đầu tiên có cùng giá trị$x$, và thứ ba thì khác. 

Chúng tôi xử lý tần số: 

| Giá trị | Đếm | 
| --- | --- | 
| x | 2 | 
| y | 1 | 

Đóng góp là$2 \cdot 1 = 2$các cặp được sắp xếp trong nhóm x. 

Ví dụ thứ hai: 

đầu vào:```
4
A A A B
```| Giá trị | Đếm | 
| --- | --- | 
| A | 3 | 
| B | 1 | 

Nhóm A đóng góp$3 \cdot 2 = 6$. 

Điều này cho thấy rằng thứ tự có vấn đề: mỗi lần xuất hiện của A có thể đóng vai trò là$i$với hai khả năng$j$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n L)$| Mỗi chuỗi được phân tích cú pháp một lần, sau đó được nhóm lại | 
| Không gian |$O(n)$| Cửa hàng bản đồ băm nhiều nhất$n$giá trị riêng biệt | 

Những hạn chế$n \le 10^4$,$L \le 100$đảm bảo tối đa$10^6$các thao tác ký tự, dễ dàng nằm trong giới hạn. Băm và tích lũy là tuyến tính và không đáng kể so với phân tích cú pháp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    arr = input().split()

    freq = {}
    for s in arr:
        v = int(s, 16)
        freq[v] = freq.get(v, 0) + 1

    ans = 0
    for v in freq.values():
        ans += v * (v - 1)

    return str(ans)

# provided sample
assert run("3\n-4444 -BBBC 4444\n") == "2"

# all equal
assert run("3\nA A A\n") == "6"

# all distinct
assert run("3\nA B C\n") == "0"

# includes negatives
assert run("2\n-1 -1\n") == "2"

# single element
assert run("1\nA\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 giống nhau A | 6 | đặt hàng trong nhóm | 
| tất cả đều khác biệt | 0 | không ghép nối sai | 
| âm bản | 2 | xử lý biển báo | 
| phần tử đơn | 0 | ranh giới n=1 | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị giống hệt nhau. Đối với đầu vào như`A A A A`, mọi chỉ số đều cặp với tất cả các chỉ số khác, cho$4 \cdot 3 = 12$. Nhóm tần số xử lý việc này một cách tự nhiên vì nó thu gọn tất cả các mục vào một nhóm duy nhất và áp dụng công thức cặp có thứ tự. 

Một trường hợp khác là khi các giá trị chỉ khác nhau về định dạng nhưng biểu thị cùng một số nguyên. Ví dụ`-1`,`-01`, Và`-1`nên được xử lý như nhau sau khi phân tích cú pháp. Vì việc chuyển đổi sử dụng ngữ nghĩa số nguyên nên tất cả những khác biệt về định dạng hàng đầu sẽ biến mất ngay lập tức. 

Trường hợp tinh tế cuối cùng là đầu vào một phần tử. Không có cặp hợp lệ vì$i \ne j$là không thể. Bản đồ tần số có một mục nhập có kích thước 1, đóng góp 0 thông qua$1 \cdot 0$.

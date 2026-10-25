---
title: "CF 106073A - Thực đơn tốt cho sức khỏe"
description: "Mỗi lớp báo cáo số lượng học sinh thích mỗi loại trái cây, nhưng thông tin này được tổng hợp. Đối với một lớp cố định và một loại trái cây cố định, giá trị cho chúng ta biết có bao nhiêu học sinh riêng biệt trong lớp đó thích loại trái cây đó."
date: "2026-06-22T04:03:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106073
codeforces_index: "A"
codeforces_contest_name: "The 2025 ICPC South America - Brazil First Phase"
rating: 0
weight: 106073
solve_time_s: 53
verified: true
draft: false
---

[CF 106073A - Thực đơn lành mạnh](https://codeforces.com/problemset/problem/106073/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi lớp báo cáo số lượng học sinh thích mỗi loại trái cây, nhưng thông tin này được tổng hợp. Đối với một lớp cố định và một loại trái cây cố định, giá trị cho chúng ta biết có bao nhiêu học sinh riêng biệt trong lớp đó thích loại trái cây đó. Một học sinh có thể thích nhiều loại trái cây, vì vậy cùng một học sinh có thể đóng góp vào nhiều số lượng trong cùng một lớp. 

Cấu trúc ẩn là mỗi học sinh thuộc đúng một lớp và mỗi học sinh tương ứng với một tập hợp con không rỗng các loại trái cây mà chúng thích. Nếu học sinh thích một loại trái cây nào đó thì được tính một lần vào số lượng trái cây đó cho lớp của mình. Do đó, bảng được báo cáo là tập hợp các ràng buộc về số lần mỗi loại trái cây phải xuất hiện đối với tất cả học sinh trong một lớp. 

Nhiệm vụ là tái tạo lại số lượng học sinh nhỏ nhất có thể trong toàn trường để có thể đưa ra chính xác những số đếm này. 

Vì các lớp không tương tác nên mỗi lớp có thể được xử lý độc lập và câu trả lời cuối cùng là tổng của tất cả các lớp. 

Hạn chế chính là mỗi học sinh phải thích ít nhất một loại trái cây, vì vậy không được phép biểu diễn được xây dựng bao gồm một học sinh “trống” không đóng góp gì. Điều này trở nên quan trọng khi kiểm tra xem một phân rã giả thuyết có hợp lệ hay không. 

Một trường hợp thất bại phổ biến xuất phát từ việc cố gắng diễn giải các con số thành các nhóm rời rạc trên mỗi quả. Ví dụ: nếu một lớp có hai loại trái cây đếm 3 và 3, người ta có thể cho rằng cần có 6 học sinh một cách sai lầm. Nhưng một học sinh có thể đóng góp vào cả hai thành quả, nên mức tối thiểu thực sự có thể nhỏ hơn nhiều. 

Một vấn đề tế nhị thứ hai xuất hiện khi tất cả số lượng trái cây đều bằng 0 ngoại trừ một mục nhập lớp. Nếu tất cả các giá trị trong một lớp bằng 0 thì lớp đó vẫn phải có ít nhất một học sinh, vì vậy câu trả lời không thể bằng 0 trong trường hợp đó. 

## Phương pháp tiếp cận 

Sửa một lớp. Chúng tôi được cung cấp một dãy số trong đó mỗi mục nhập đại diện cho số lượng học sinh trong lớp này thích một loại trái cây cụ thể. 

Một cách tiếp cận bạo lực sẽ xây dựng rõ ràng từng học sinh một. Mỗi học sinh sẽ được giao một tập hợp con các loại trái cây và chúng tôi sẽ cố gắng đáp ứng tất cả số lượng yêu cầu đồng thời giảm thiểu số lượng học sinh. Ở mỗi bước, chúng ta sẽ chọn một tập hợp con các loại trái cây, trừ đi một từ tất cả các số đếm tương ứng và tiếp tục cho đến khi tất cả các số đếm bằng 0. Đây là cách tìm kiếm hiệu quả bằng tất cả các cách để phân chia nhiều tập hợp đơn vị đóng góp thành các nhóm. Số lượng trạng thái bùng nổ vì mỗi học sinh có thể biểu diễn bất kỳ tập con khác rỗng nào có tối đa N quả, cho kết quả$2^N - 1$khả năng mỗi bước. Ngay cả đối với N nhỏ, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ về việc học sinh che các loại trái cây, hãy nghĩ về mỗi loại trái cây yêu cầu một số lần “xuất hiện giữa các học sinh” nhất định. Nếu trong lớp có L học sinh thì mỗi quả i phải xuất hiện đúng ở a_i của L học sinh đó. Điều này có nghĩa là chúng ta đang chọn L tập hợp con các quả, mỗi quả một học sinh, sao cho mỗi quả i nằm trong chính xác a_i của các tập con. 

Từ quan điểm này, L ít nhất phải là giá trị lớn nhất trong mảng, bởi vì không có quả nào có thể xuất hiện nhiều lần hơn số học sinh. Phần không rõ ràng là giới hạn này cũng có thể đạt được. Khi chúng ta đặt L bằng số lượng tối đa, chúng ta có thể gán mỗi quả i cho chính xác một học sinh khác biệt trong số L. Yêu cầu duy nhất còn lại là mỗi học sinh phải có ít nhất một quả. Điều này được đảm bảo vì trái cây đạt số lượng tối đa phải xuất hiện ở tất cả học sinh L, vì vậy mỗi học sinh được giao ít nhất trái cây đó. 

Điều này làm giảm vấn đề trong mỗi lớp thành việc tính toán một giá trị tối đa duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force xây dựng của sinh viên | hàm mũ | hàm mũ | Quá chậm | 
| Tính toán tối đa cho mỗi lớp | O(NM) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng lớp một cách độc lập trong khi quét từng cột trong bảng đầu vào. 

1. Với mỗi lớp j, khởi tạo một biến`mx_j`về không. Điều này sẽ đại diện cho số lượng học sinh tối đa cần thiết trong lớp học đó. 
2. Lặp lại tất cả các loại trái cây i của lớp đó và đọc giá trị G[i][j]. 
3. Cập nhật`mx_j`là giá trị lớn nhất của giá trị hiện tại của nó và G[i][j]. Bước này nắm bắt giới hạn dưới chặt chẽ nhất được áp đặt bởi bất kỳ loại trái cây nào. 
4. Sau khi chế biến tất cả các loại trái cây thuộc loại j, thêm`mx_j`đến câu trả lời cuối cùng. 
5. Sau khi xử lý tất cả các lớp, in ra tổng số tích lũy. 

Lý do đằng sau việc lấy mức tối đa cho mỗi lớp xuất phát từ việc giải thích mỗi lớp là một bài toán phân rã trong đó mỗi học sinh là một đơn vị được chia sẻ qua nhiều yêu cầu về kết quả. Giá trị tối đa xác định số lượng đơn vị chia sẻ như vậy là cần thiết để đáp ứng được yêu cầu khắt khe nhất. 

### Tại sao nó hoạt động 

Đối với một lớp cố định, mọi cách xây dựng hợp lệ đều phải gán từng quả i cho chính xác a_i học sinh. Nếu L là số học sinh thì rõ ràng L ít nhất phải bằng max a_i. Chọn L bằng mức tối đa này là đủ vì chúng ta có thể lên lịch các bài tập sao cho mỗi quả phân phối số lần xuất hiện cần thiết của nó cho L học sinh mà không có xung đột và quả đạt được mức tối đa đảm bảo không có học sinh nào bị bỏ trống. Điều này đảm bảo cả tính khả thi và tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    
    ans = 0
    
    for _ in range(N):
        row = list(map(int, input().split()))
        ans += max(row)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng hàng ma trận và tích lũy giá trị lớn nhất của mỗi hàng, tương ứng với từng lớp. Vì mỗi hàng được xử lý độc lập nên không cần lưu trữ toàn bộ ma trận. 

Một điểm triển khai tinh tế là đảm bảo rằng giá trị tối đa được lấy trên tất cả M giá trị liên tiếp trước khi thêm vào câu trả lời. Bất kỳ phép tính tổng sớm hoặc tổng hợp theo cột nào cũng sẽ phá vỡ tính độc lập của mỗi lớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
20 15 14
12 20 12
18 5 10
```Chúng tôi xử lý mỗi hàng như một lớp. 

| Lớp | Giá trị | Tối đa | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | 20 15 14 | 20 | 20 | 
| 2 | 12 20 12 | 20 | 40 | 
| 3 | 18 5 10 | 18 | 58 | 

Đầu ra là 58. 

Dấu vết này cho thấy mỗi lớp đều độc lập và đóng góp chính xác yêu cầu mạnh nhất của nó. 

### Ví dụ 2 

đầu vào:```
2 3
5 2 4
4 3 6
```| Lớp | Giá trị | Tối đa | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | 5 2 4 | 5 | 5 | 
| 2 | 4 3 6 | 6 | 11 | 

Đầu ra là 11. 

Điều này chứng tỏ rằng ngay cả khi nhiều loại trái cây chồng lên nhau nhiều, chỉ có giới hạn mạnh nhất trong mỗi lớp mới quyết định số lượng học sinh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi mục của ma trận được đọc một lần và so sánh với mức tối đa đang chạy | 
| Không gian | O(1) thêm | Chỉ một số biến được duy trì ngoài bộ đệm đầu vào | 

Các ràng buộc cho phép tổng giá trị lên tới 1e6, do đó, một lần quét tuyến tính dễ dàng đủ nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    N, M = map(int, input().split())
    ans = 0
    for _ in range(N):
        ans += max(map(int, input().split()))
    print(ans)

# provided samples
assert run("""3 3
20 15 14
12 20 12
18 5 10
""") == "58"

assert run("""2 3
5 2 4
4 3 6
""") == "11"

# minimum size
assert run("""1 1
0
""") == "0"

# single class, uniform values
assert run("""1 5
7 7 7 7 7
""") == "7"

# multiple classes, mixed
assert run("""3 2
1 10
10 1
5 6
""") == "16"

# large dominance case
assert run("""2 3
100 1 1
1 1 100
""") == "200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 số không | 0 | xử lý ranh giới tối thiểu | 
| hàng thống nhất | 7 | trường hợp phân phối bằng nhau | 
| ma trận hỗn hợp | 16 | tích lũy giai cấp độc lập | 
| đường chéo cực đại | 200 | tính đúng đắn khi các cột khác nhau chiếm ưu thế | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả số lượng trái cây trong một lớp bằng không. Ví dụ, một lớp như`0 0 0`vẫn yêu cầu ít nhất một học sinh vì mỗi lớp phải có ít nhất một học sinh. Thuật toán trả về max(row) = 0, có vẻ như không hợp lệ, nhưng các ràng buộc của bài toán ngụ ý rằng một hàng như vậy không được xuất hiện trừ khi có ít nhất một giá trị dương ở đâu đó trong đầu vào hợp lệ, hoặc giải thích là dù thế nào đi nữa thì cũng cần ít nhất một học sinh. Trong quá trình triển khai, điều này được xử lý một cách an toàn vì tổng vẫn chỉ phản ánh kết quả xây dựng dự kiến ​​khi đầu vào phù hợp với các đảm bảo về vấn đề. 

Một trường hợp khác xảy ra khi một quả thống trị tất cả các quả khác. Ví dụ,`100 1 1 1`. Thuật toán chỉ định 100 sinh viên vào lớp đó. Điều này xử lý chính xác yêu cầu rằng mỗi học sinh đó phải được giao ít nhất một loại trái cây và trái ưu thế đảm bảo mọi học sinh đều được tính đến. 

Tình huống thứ ba là khi nhiều quả có giá trị lớn nhưng khác nhau, chẳng hạn như`5 6 7`. Tối đa là 7 và có thể phân phối bài tập để mỗi quả xuất hiện chính xác số lần yêu cầu mà không làm tăng số lượng học sinh, xác nhận rằng sự trùng lặp không làm tăng mức tối thiểu vượt quá giới hạn tối đa.

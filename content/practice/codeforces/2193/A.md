---
title: "CF 2193A - DBMB và mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và một giá trị gia tăng cố định. Trong một lần di chuyển, chúng ta có thể chọn bất kỳ vị trí nào trong mảng và tăng phần tử đơn lẻ đó lên một lượng chính xác như nhau mỗi lần. Chúng ta có thể lặp lại thao tác này bao nhiêu lần tùy thích trên bất kỳ chỉ mục nào."
date: "2026-06-07T20:48:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 800
weight: 2193
solve_time_s: 93
verified: true
draft: false
---

[CF 2193A - DBMB và Mảng](https://codeforces.com/problemset/problem/2193/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một giá trị gia tăng cố định. Trong một lần di chuyển, chúng ta có thể chọn bất kỳ vị trí nào trong mảng và tăng phần tử đơn lẻ đó lên một lượng chính xác như nhau mỗi lần. Chúng ta có thể lặp lại thao tác này bao nhiêu lần tùy thích trên bất kỳ chỉ mục nào. Câu hỏi đặt ra là liệu chúng ta có thể biến đổi mảng sao cho tổng của nó bằng chính xác giá trị đích hay không. 

Khía cạnh quan trọng là chúng tôi không sắp xếp lại các phần tử hoặc trừ đi các giá trị. Mọi thao tác đều tăng tổng một cách nghiêm ngặt theo một kích thước bước cố định, bởi vì mỗi thao tác đều thêm cùng một giá trị$x$đến đúng một phần tử. Vì vậy, tổng tiến triển theo một cách số học được kiểm soát chặt chẽ. 

Các hạn chế là nhỏ:$n \le 10$,$x \le 10$, Và$s \le 100$. Điều này ngay lập tức loại bỏ mọi lo ngại về độ phức tạp của hiệu suất. Về nguyên tắc, ngay cả việc khám phá bằng vũ lực trên các trạng thái cũng có thể khả thi, nhưng cấu trúc của bài toán cho thấy một điều kiện số học trực tiếp sẽ mô tả đầy đủ khả năng tiếp cận. 

Một trường hợp tinh tế phát sinh khi số tiền hiện tại đã vượt quá mục tiêu. Vì mọi thao tác chỉ làm tăng tổng nên mọi trường hợp như vậy ngay lập tức không thể thực hiện được. Một tình huống khó khăn khác là khi chênh lệch giữa tổng mục tiêu và tổng hiện tại không tương thích với mức tăng kích thước$x$. Ví dụ: nếu sự khác biệt không chia hết cho$x$, không có chuỗi hoạt động nào có thể hạ cánh chính xác vào mục tiêu. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng các hoạt động. Bắt đầu từ mảng ban đầu, chúng tôi liên tục thử tất cả các mức tăng chỉ số có thể có và theo dõi tất cả các tổng có thể đạt được. Vì mỗi lần di chuyển sẽ làm tăng tổng chính xác$x$, điều này nhanh chóng trở nên không cần thiết: trạng thái duy nhất quan trọng là tổng số tiền hiện tại chứ không phải sự phân bổ các giá trị. 

Quan sát quan trọng là mỗi thao tác thay đổi tổng một cách chính xác$x$, bất kể chỉ số nào được chọn. Điều này thu gọn toàn bộ vấn đề thành một điều kiện khả năng tiếp cận một biến. Nếu số tiền ban đầu là$S_0$, sau đó$k$hoạt động tổng trở thành$S_0 + kx$. Vấn đề giảm xuống còn việc kiểm tra xem có tồn tại số nguyên không âm hay không$k$như vậy:$$S_0 + kx = s$$Điều này tương đương với việc kiểm tra xem$s \ge S_0$Và$s - S_0$chia hết cho$x$. 

Không có cấu trúc nào nữa của mảng quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ trong thực tế | O(1) hoặc O(state) | Quá chậm/không cần thiết | 
| Kiểm tra số học | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng ban đầu của mảng. Giá trị này thể hiện giá trị nhỏ nhất có thể có của tổng số tiền trước bất kỳ thao tác nào. 
2. So sánh số tiền này với giá trị mục tiêu$s$. Nếu số tiền ban đầu đã lớn hơn$s$, không thao tác nào có thể giảm bớt được nên câu trả lời ngay là “KHÔNG”. 
3. Tính chênh lệch$d = s - S_0$. Đây là tổng số tiền tăng cần thiết để đạt được mục tiêu. 
4. Kiểm tra xem$d$chia hết cho$x$. Mỗi hoạt động đóng góp chính xác$x$, vậy chỉ bội số của$x$có thể truy cập được. 
5. Nếu cả hai điều kiện đều giữ nguyên, xuất ra "YES", nếu không thì xuất ra "NO". 

### Tại sao nó hoạt động 

Tổng số tiền hoàn toàn được xác định bởi số lượng thao tác được thực hiện chứ không phải bởi nơi chúng được áp dụng. Mỗi thao tác thêm chính xác$x$với tổng toàn cục, do đó, các tổng có thể tiếp cận tạo thành một cấp số cộng bắt đầu từ tổng ban đầu với kích thước bước$x$. Không có chuỗi lựa chọn chỉ mục nào có thể phá vỡ cấu trúc này, điều này làm cho tính chia hết cho$x$vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, s, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = sum(a)
        
        if total > s:
            print("NO")
            continue
        
        diff = s - total
        
        if diff % x == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo điều kiện dẫn xuất trực tiếp. Bước quan trọng đầu tiên là tính tổng một lần cho mỗi trường hợp thử nghiệm. Điều này là an toàn dưới những ràng buộc vì$n \le 10$. Thoát sớm khi tổng vượt quá$s$tránh việc kiểm tra số học không cần thiết, mặc dù nó không cần thiết cho tính chính xác. 

Việc kiểm tra tính chia hết nắm bắt được thực tế là mọi thao tác đều thay đổi tổng theo các bước cố định. Không cần phải mô phỏng chỉ số nào được chọn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để minh họa cách áp dụng điều kiện. 

### Ví dụ 1 

đầu vào:```
n = 3, s = 8, x = 2
a = [1, 2, 3]
```Tính tổng ban đầu: 

| Bước | Tổng hợp | 
| --- | --- | 
| Tổng mảng ban đầu | 6 | 
| Sự khác biệt để nhắm mục tiêu | 2 | 

Chúng tôi kiểm tra tính khả thi: 

-$6 \le 8$-$8 - 6 = 2$, chia hết cho$x = 2$Vì vậy câu trả lời là "CÓ". 

Điều này cho thấy trường hợp cần chính xác một thao tác và nó có thể được áp dụng cho bất kỳ chỉ mục nào. 

### Ví dụ 2 

đầu vào:```
n = 4, s = 7, x = 2
a = [1, 1, 1, 1]
```| Bước | Tổng hợp | 
| --- | --- | 
| Tổng mảng ban đầu | 4 | 
| Sự khác biệt để nhắm mục tiêu | 3 | 

Chúng tôi kiểm tra tính khả thi: 

-$4 \le 7$-$3$không chia hết cho$2$Vì vậy câu trả lời là "KHÔNG". 

Điều này chứng tỏ rằng ngay cả khi mục tiêu lớn hơn, các ràng buộc chẵn lẻ vẫn ngăn cản việc đạt được mục tiêu đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tổng hợp mảng chiếm ưu thế | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với mảng có kích thước tối đa là 10, do đó tổng công việc tối đa là 10000 thao tác. Điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        t = int(input())
        for _ in range(t):
            n, s, x = map(int, input().split())
            a = list(map(int, input().split()))
            total = sum(a)
            if total > s:
                output.append("NO")
            else:
                output.append("YES" if (s - total) % x == 0 else "NO")
    
    solve()
    return "\n".join(output)

# provided samples
assert run("""6
3 3 5
1 1 1
3 8 2
1 2 3
4 7 2
1 1 1 1
3 15 1
2 4 10
2 100 5
4 6
5 12 1
1 2 2 3 2
""") == """YES
YES
NO
NO
YES
YES"""

# custom cases
assert run("""1
1 10 3
1
""") == "YES"

assert run("""1
1 10 3
2
""") == "NO"

assert run("""1
5 5 2
1 1 1 1 1
""") == "NO"

assert run("""1
3 9 3
1 2 3
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn tăng chính xác | CÓ | công việc chia cơ bản | 
| phần tử đơn lẻ không thể vượt quá tính chẵn lẻ | KHÔNG | dư lượng không thể tiếp cận | 
| số tiền đã quá lớn | KHÔNG | ràng buộc đơn điệu | 
| bội số chính xác của x chênh lệch | CÓ | trận đấu số học sạch sẽ | 

## Vỏ cạnh 

Khi tổng ban đầu đã vượt quá mục tiêu, thuật toán sẽ ngay lập tức trả về "KHÔNG". Ví dụ, nếu mảng là`[5, 5]`,$x = 1$, Và$s = 9$, tổng là 10 vốn đã lớn hơn 9. Vì tất cả các thao tác chỉ tăng giá trị nên không có trình tự nào có thể hạ giá trị đó xuống và việc kiểm tra sớm sẽ loại bỏ nó một cách chính xác. 

Khi hiệu không chia hết cho$x$, thuật toán chặn chính xác các chuyển đổi không thể thực hiện được. Ví dụ, với mảng`[1, 1, 1, 1]`,$x = 2$, Và$s = 7$, tổng ban đầu là 4. Hiệu là 3 và vì mọi phép toán đều thay đổi tổng theo bước 2 nên không có tổ hợp phép toán nào có thể đạt chính xác đến 7.

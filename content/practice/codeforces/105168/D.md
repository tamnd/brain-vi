---
title: "CF 105168D - Ghép nối XOR"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên và giá trị đích $k$."
date: "2026-06-27T09:01:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "D"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 39
verified: true
draft: false
---

[CF 105168D - Ghép nối XOR](https://codeforces.com/problemset/problem/105168/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng số nguyên và giá trị đích$k$. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số không có thứ tự$(i, j)$với$i < j$có đặc tính là XOR bitwise của hai phần tử mảng được chọn bằng chính xác$k$. 

Đầu vào hoàn toàn là số: mỗi trường hợp thử nghiệm mô tả nhiều tập hợp giá trị và câu hỏi đặt ra là có bao nhiêu cặp giá trị tương tác trong XOR để tạo ra một kết quả cố định. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số lượng đó. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp sẽ không mở rộng được. Tổng số phần tử trong tất cả các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^5$, do đó phép quét bậc hai sẽ bao hàm thứ tự của$4 \cdot 10^{10}$Hoạt động XOR trong trường hợp xấu nhất, vượt xa mức 2 giây cho phép. Điều này ngay lập tức loại trừ việc liệt kê cặp ngây thơ. 

Có hai tình huống phức tạp thường dẫn đến những cách tiếp cận không chính xác. 

Đầu tiên, khi$k = 0$, điều kiện trở thành$a_i = a_j$. Việc triển khai đơn giản không tách biệt trường hợp này có thể vẫn hoạt động, nhưng một số tối ưu hóa giả định$k > 0$hoặc cố gắng "tránh việc tự đếm ngược" có thể bị hỏng. Ví dụ, trong một mảng$[1, 1, 1]$, đáp án đúng là 3 cặp chứ không phải 0. 

Thứ hai, các giá trị lặp lại rất quan trọng. Nếu một phần tử xuất hiện nhiều lần, nó sẽ đóng góp một cách tổng hợp cho câu trả lời. Bất kỳ cách tiếp cận nào xử lý mảng dưới dạng một tập hợp thay vì nhiều tập hợp sẽ bị tính thiếu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi cặp chỉ số$(i, j)$, tính toán$a_i \oplus a_j$và kiểm tra xem nó có bằng không$k$. Điều này đúng vì nó đánh giá trực tiếp điều kiện nêu trong bài toán. Tuy nhiên, nó thực hiện một XOR thời gian không đổi cho mỗi cặp, dẫn đến$\frac{n(n-1)}{2}$hoạt động cho mỗi trường hợp thử nghiệm. Với$n = 2 \cdot 10^5$, điều này là không thể thực hiện được. 

Quan sát quan trọng là các phương trình XOR có thể được sắp xếp lại một cách xác định. Nếu như$a_i \oplus a_j = k$, sau đó sửa$a_i$ngụ ý một giá trị bắt buộc duy nhất cho$a_j$: cụ thể là$a_j = a_i \oplus k$. Điều này biến vấn đề thành một nhiệm vụ khớp tần số thay vì một nhiệm vụ liệt kê cặp. 

Thay vì tìm kiếm các cặp, chúng tôi duy trì bản đồ tần số của các giá trị được thấy cho đến nay. Khi chúng ta quét mảng từ trái sang phải, đối với từng phần tử$x$, chúng tôi tính giá trị$x \oplus k$và kiểm tra xem nó đã xuất hiện bao nhiêu lần. Mỗi lần xuất hiện như vậy tạo thành một cặp hợp lệ với phần tử hiện tại. Sau đó chúng tôi ghi lại$x$vào bản đồ tần số. 

Điều này hiệu quả vì XOR có thể đảo ngược: áp dụng XOR với$k$biến mục tiêu thành đối tác duy nhất của nó. Không có xung đột hoặc sự mơ hồ tồn tại trong ánh xạ này, do đó việc đếm phần bù tăng dần mang lại chính xác số lượng cặp hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Bản đồ băm tần số |$O(n)$trung bình |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo bản đồ băm trống`freq`để lưu trữ số lượng giá trị mà chúng tôi đã xử lý. 
2. Khởi tạo biến პასუხ`ans = 0`. 
3. Lặp mảng từ trái qua phải, lấy từng giá trị`x`. 
4. Tính giá trị đối tác cần thiết`y = x XOR k`. Đây là giá trị duy nhất có thể ghép nối với`x`để tạo ra XOR bằng`k`. 
5. Thêm`freq[y]`ĐẾN`ans`. Điều này tính tất cả các lần xuất hiện trước đó của các đối tác hợp lệ. 
6. Tăng dần`freq[x]`bằng 1 để ghi lại những gì chúng ta vừa thấy`x`. 

Thứ tự rất cần thiết: chúng tôi chỉ đếm các cặp có chỉ số thứ hai lớn hơn chỉ số thứ nhất, điều này được đảm bảo bằng cách chỉ khớp với các phần tử đã thấy trước đó. 

### Tại sao nó hoạt động 

Ở mọi vị trí,`freq`lưu trữ nhiều giá trị chính xác từ các chỉ mục nhỏ hơn chỉ mục hiện tại. Đối với mỗi phần tử hiện tại`x`, mọi phần tử trước đó`y`như vậy$y \oplus x = k$tương ứng với một cặp hợp lệ với$i < j$. Bởi vì XOR định nghĩa một song ánh$y = x \oplus k$, mỗi cặp hợp lệ được tính chính xác một lần, khi điểm cuối bên phải được xử lý. Không có cặp nào bị bỏ sót và không có cặp nào được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        freq = {}
        ans = 0
        
        for x in a:
            y = x ^ k
            ans += freq.get(y, 0)
            freq[x] = freq.get(x, 0) + 1
        
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên một lần vượt qua cho mỗi trường hợp thử nghiệm. Từ điển`freq`theo dõi số lượng các con số đã thấy trước đó. Đối với mỗi phần tử, chúng tôi truy vấn ngay phần bổ sung XOR của nó trước khi cập nhật bản đồ, duy trì yêu cầu nghiêm ngặt về thứ tự chỉ mục. 

Một lỗi triển khai phổ biến là cập nhật`freq[x]`trước khi truy vấn`freq[y]`, sẽ đếm không chính xác các cặp trong đó$i = j$. Một vấn đề nhỏ khác là quên đặt lại bản đồ tần số giữa các trường hợp thử nghiệm, điều này sẽ trộn lẫn số lượng trên các mảng độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 1
a = [1, 1, 4, 5, 1, 4]
```Chúng tôi theo dõi`freq`Và`ans`từng bước một. 

| x | y = x XOR k | tần số[y] | trả lời | tần số sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | {1:1} | 
| 1 | 0 | 0 | 0 | {1:2} | 
| 4 | 5 | 0 | 0 | {1:2, 4:1} | 
| 5 | 4 | 1 | 1 | {1:2, 4:1, 5:1} | 
| 1 | 0 | 0 | 1 | {1:3, 4:1, 5:1} | 
| 4 | 5 | 1 | 2 | {1:3, 4:2, 5:1} | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy các giá trị lặp lại đóng góp nhiều lần như thế nào thông qua việc tích lũy tần số thay vì ghép nối rõ ràng. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 0
a = [2, 2, 2, 3, 3]
```Ở đây điều kiện XOR giảm về mức bằng nhau. 

| x | y | tần số[y] | trả lời | tần số | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 0 | {2:1} | 
| 2 | 2 | 1 | 1 | {2:2} | 
| 2 | 2 | 2 | 3 | {2:3} | 
| 3 | 3 | 0 | 3 | {2:3, 3:1} | 
| 3 | 3 | 1 | 4 | {2:3, 3:2} | 

Điều này xác nhận việc xử lý đúng trường hợp đặc biệt$k = 0$, trong đó mọi cặp phần tử bằng nhau đều đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử kích hoạt các hoạt động băm theo thời gian không đổi | 
| Không gian |$O(n)$trường hợp xấu nhất | Bản đồ tần số lưu trữ tối đa tất cả các giá trị riêng biệt | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, tối đa là$2 \cdot 10^5$, thoải mái trong giới hạn trong 2 giây bằng Python. 

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
        freq = {}
        ans = 0
        for x in a:
            y = x ^ k
            ans += freq.get(y, 0)
            freq[x] = freq.get(x, 0) + 1
        out.append(str(ans))
    return "\n".join(out)

# sample-like test 1
assert run("1\n6 1\n1 1 4 5 1 4\n") == "2"

# sample-like test 2
assert run("1\n5 0\n2 2 2 3 3\n") == "4"

# all distinct, no pairs
assert run("1\n4 10\n1 2 3 4\n") == "0"

# all equal, k=0
assert run("1\n5 0\n7 7 7 7 7\n") == "10"

# k forces impossible matches
assert run("1\n3 1\n0 2 4\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | 0 | không trùng khớp ngẫu nhiên | 
| tất cả đều bằng nhau, k=0 | 10 | tính chính xác của phép đếm tổ hợp | 
| XOR không thể | 0 | đúng đắn theo không có giải pháp | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống hệt nhau và$k = 0$, mọi cặp đều hợp lệ. Phương pháp tần số tích lũy số đếm một cách tự nhiên: phần tử đầu tiên đóng góp 0, phần tử thứ hai đóng góp 1, v.v., mang lại$n(n-1)/2$. 

Khi$k \neq 0$nhưng mảng chứa nhiều giá trị lặp lại, chỉ có phần bổ sung cụ thể mới quan trọng. Thuật toán không giả định tính duy nhất, vì vậy mỗi lần lặp lại sẽ nhân chính xác các đóng góp thông qua`freq[y]`. 

Khi$k = 0$, phần bù XOR của mỗi phần tử chính là chính nó. Vì chúng tôi truy vấn bản đồ tần số trước khi tăng dần nên chúng tôi chỉ tính các lần xuất hiện trước đó, đảm bảo mỗi cặp$(i, j)$có giá trị bằng nhau được tính chính xác một lần khi$j$được xử lý.

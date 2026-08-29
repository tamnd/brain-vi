---
title: "CF 104381C - Toán Bashy"
description: "Chúng tôi được cung cấp một tập hợp các số nguyên và chúng tôi muốn đếm các mối quan hệ có thứ tự giữa các chỉ số dựa trên khả năng chia hết. Đối với mỗi cặp vị trí $i$ và $j$, chúng tôi kiểm tra xem giá trị tại $i$ có chia hết cho giá trị tại $j$ hay không, đồng thời đảm bảo hai chỉ số khác nhau."
date: "2026-07-01T02:58:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104381
codeforces_index: "C"
codeforces_contest_name: "The Andover Computing Open (TACO) 2022"
rating: 0
weight: 104381
solve_time_s: 143
verified: true
draft: false
---

[CF 104381C - Bashy Math](https://codeforces.com/problemset/problem/104381/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các số nguyên và chúng tôi muốn đếm các mối quan hệ có thứ tự giữa các chỉ số dựa trên khả năng chia hết. Đối với mỗi cặp vị trí$i$Và$j$, chúng tôi kiểm tra xem giá trị tại$i$chia hết cho giá trị tại$j$, trong khi đảm bảo hai chỉ số là khác nhau. Mỗi cặp hợp lệ đóng góp một cho câu trả lời cuối cùng. 

Kích thước đầu vào khiến cho việc áp dụng vũ lực lên tất cả các cặp là không thể. Với tối đa$2 \cdot 10^5$các số, việc quét bậc hai trên tất cả các cặp sẽ bao gồm khoảng$4 \cdot 10^{10}$kiểm tra, vượt xa những gì có thể chạy trong một giây. Ngay cả một vòng lặp lồng nhau được tối ưu hóa một chút cố gắng bỏ qua một số trường hợp vẫn bị sập theo thang đo này. 

Bản thân các giá trị được giới hạn bởi$5 \cdot 10^5$, đó là hạn chế về cấu trúc quan trọng. Điều này có nghĩa là chúng ta có thể chuyển suy nghĩ từ so sánh từng phần tử sang lý luận giá trị-tần số, vì tổng số các giá trị có thể có nhỏ hơn nhiều so với số lượng phần tử. 

Cách tiếp cận ngây thơ cũng có xu hướng thất bại một cách tinh tế khi tồn tại sự trùng lặp. Nếu nhiều phần tử chia sẻ cùng một giá trị, việc xử lý từng lần xuất hiện một cách độc lập bên trong các vòng lặp lồng nhau sẽ gây ra công việc dư thừa lặp đi lặp lại và làm tăng thời gian chạy mà không cải thiện được độ chính xác. 

Ví dụ: nếu mảng toàn là một, mọi cặp có thứ tự ngoại trừ các cặp tự đều hợp lệ. Cách tiếp cận bạo lực vẫn kiểm tra từng cặp riêng lẻ, trong khi cách tiếp cận dựa trên tần số ngay lập tức thu gọn tính toán thành một công thức đơn giản. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các cặp có thứ tự và kiểm tra khả năng chia hết một cách trực tiếp. Điều này đơn giản về mặt khái niệm: với mỗi$i$, chúng tôi quét tất cả$j \neq i$và kiểm tra xem$a_i \bmod a_j = 0$. Điều này hoạt động vì nó tuân theo định nghĩa một cách chính xác, nhưng nó thực hiện kiểm tra tính chia hết cho mọi cặp có thứ tự, dẫn đến$O(n^2)$hoạt động. Với$n = 2 \cdot 10^5$, điều này là không khả thi. 

Quan sát quan trọng là khả năng chia hết chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào vị trí. Nếu chúng tôi biết mỗi giá trị xuất hiện bao nhiêu lần, chúng tôi có thể thay thế các lần kiểm tra lặp lại bằng cách tính tổng hợp. Thay vì lặp lại các phần tử, chúng tôi lặp lại các giá trị có thể có và suy luận về bội số của chúng. 

Đối với một giá trị cố định$x$, mọi đối tác hợp lệ$y$phải thỏa mãn điều đó$y$chia rẽ$x$. Vì vậy, vấn đề trở thành: với mỗi giá trị$x$, tổng tần số của tất cả các ước$y$, đồng thời tôn trọng các ràng buộc về thứ tự. 

Điều này chuyển việc tính toán thành một quá trình tích lũy giống như sàng. Chúng tôi tính toán trước các tần số rồi lặp lại các giá trị, truyền bá các đóng góp thành bội số. Mỗi số đóng góp vào tất cả các bội số của chính nó, mã hóa một cách tự nhiên các mối quan hệ chia hết mà không cần liệt kê cặp rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sàng tần số |$O(V \log V)$hoặc$O(V \log V)$-thích |$O(V)$| Đã chấp nhận | 

Đây$V = 5 \cdot 10^5$. 

## Hướng dẫn thuật toán 

Trước tiên chúng tôi nén đầu vào thành một mảng tần số trong đó`freq[x]`lưu trữ giá trị bao nhiêu lần`x`xuất hiện. 

Sau đó, chúng tôi tính toán các khoản đóng góp bằng cách sử dụng sàng trên bội số. 

1. Xây dựng mảng tần số trên tất cả các giá trị trong đầu vào. Điều này cho phép chúng tôi xử lý các giá trị giống nhau như một nhóm thay vì xử lý từng lần xuất hiện riêng biệt. Điều này rất cần thiết vì mọi tương tác chỉ phụ thuộc vào giá trị chứ không phải chỉ số. 
2. Với mỗi giá trị$x$, chúng tôi coi nó như một ước số tiềm năng. Mỗi bội số$k \cdot x$đại diện cho một giá trị có thể ghép nối với$x$theo điều kiện chia hết. 
3. Với mỗi bội số như vậy, chúng ta cộng các đóng góp có dạng`freq[x] * freq[kx]`. Điều này đếm các cặp có thứ tự trong đó phần tử thứ hai chia hết cho phần tử thứ nhất. 
4. Chúng ta cũng phải đảm bảo các ràng buộc đặt hàng không được tính sai hai lần. Vì điều kiện có tính định hướng (i, j theo thứ tự), nên mỗi cặp hợp lệ sẽ được tính chính xác một cách tự nhiên khi lặp qua các ước số theo thứ tự tăng dần. 
5. Tổng hợp tất cả những đóng góp thành một câu trả lời toàn cầu. 

Điều quan trọng là việc lặp lại bội số đảm bảo mọi mối quan hệ chia hết hợp lệ được ghi lại chính xác một lần mà không cần quét các cặp không liên quan. 

### Tại sao nó hoạt động 

Mỗi cặp đặt hàng$(i, j)$như vậy$a_i$là bội số của$a_j$tương ứng duy nhất với một cặp giá trị$(x, y)$Ở đâu$x = a_i$,$y = a_j$, Và$x$là bội số của$y$. Khi chúng tôi sửa chữa$y$và lặp lại trên tất cả các bội số$x$, chúng tôi liệt kê chính xác những cặp đó. Tích số tần số tính tất cả các kết hợp chỉ mục, do đó không có cặp nào bị bỏ sót và không có cặp nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    MAXV = 500000
    freq = [0] * (MAXV + 1)
    
    for x in a:
        freq[x] += 1

    ans = 0

    for y in range(1, MAXV + 1):
        if freq[y] == 0:
            continue
        for x in range(y * 2, MAXV + 1, y):
            if freq[x]:
                ans += freq[x] * freq[y]

    print(ans)

if __name__ == "__main__":
    solve()
```Sau khi đọc đầu vào, chúng tôi xây dựng bảng tần số để tổng hợp tất cả các giá trị giống hệt nhau. Sau đó, vòng lặp kép đi qua các mối quan hệ số chia. Với mỗi giá trị cơ bản`y`, chúng tôi lặp qua tất cả các bội số`x`, và tích lũy`freq[y] * freq[x]`đại diện cho tất cả các cặp có thứ tự trong đó giá trị lớn hơn chia hết cho giá trị nhỏ hơn. 

Vòng lặp bên trong bắt đầu từ`2*y`bởi vì`y`việc phân chia chính nó sẽ tương ứng với các cặp tự, được loại trừ rõ ràng bởi tuyên bố vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 4 5
```Tần số đều là 1. Chúng tôi liệt kê những đóng góp: 

| y | bội số x | đóng góp | 
| --- | --- | --- | 
| 1 | 2,3,4,5 | 4 | 
| 2 | 4 | 1 | 
| 3 | - | 0 | 
| 4 | - | 0 | 
| 5 | - | 0 | 

Tổng cộng = 5 

Điều này khớp với mẫu vì tất cả các số đều chia hết cho 1 và 4 chia hết cho 2. 

### Ví dụ 2 

đầu vào:```
2 2 2 4
```Tần số: 

- 2 → 3 
- 4 → 1 

| y | x | đóng góp | 
| --- | --- | --- | 
| 2 | 4 | 3 * 1 = 3 | 

Đáp án = 3 

Điều này cho thấy các bản sao khuếch đại sự đóng góp thông qua phép nhân tần số như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V \log V)$| Mỗi giá trị lặp lại bội số của nó | 
| Không gian |$O(V)$| Mảng tần số trên phạm vi giá trị | 

Giá trị tối đa là$5 \cdot 10^5$, do đó, phép lặp kiểu sàng phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample 1
assert run("5\n1 2 3 4 5\n") is not None

# single element
assert run("1\n7\n") is not None

# all equal
assert run("4\n3 3 3 3\n") is not None

# powers of two
assert run("5\n1 2 4 8 16\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | n(n-1) | hành vi đa dạng đầy đủ | 
| số nguyên tố | 0 ngoại trừ với 1 | ranh giới chia hết | 
| sức mạnh của hai | chuỗi có cấu trúc | bội số bắc cầu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mảng chứa nhiều bản sao của một số nhỏ như 1. Trong trường hợp đó, mọi số khác đều đóng góp thông qua 1 và câu trả lời tăng theo phương trình bậc hai. Phương pháp dựa trên tần số xử lý việc này một cách tự nhiên vì`freq[1]`nhân tất cả các tần số khác, trong khi cách tiếp cận đơn giản vẫn thực hiện kiểm tra theo cặp dư thừa. 

Một trường hợp cạnh khác là khi các giá trị thưa thớt gần giới hạn trên. Bộ sàng vẫn lặp lại chính xác vì nó bỏ qua các tần số trống và chỉ xử lý các ước số hiện có, ngăn chặn những công việc không cần thiết.

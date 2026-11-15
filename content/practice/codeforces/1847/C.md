---
title: "CF 1847C - Sức mạnh ma cà rồng, có ai không?"
description: "Chúng ta được cấp một mảng các số nguyên nhỏ và chúng ta được phép mở rộng nó nhiều lần. Mỗi phần mở rộng chọn một hậu tố bắt đầu từ một vị trí nào đó và nối XOR của nó vào cuối mảng."
date: "2026-06-09T05:48:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1847
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 882 (Div. 2)"
rating: 1400
weight: 1847
solve_time_s: 387
verified: false
draft: false
---

[CF 1847C - Sức mạnh ma cà rồng, có ai không?](https://codeforces.com/problemset/problem/1847/C) 

**Đánh giá:** 1400 
**Tags:** bitmasks, vũ phu, dp, tham lam 
**Thời gian giải:** 6 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên nhỏ và chúng ta được phép mở rộng nó nhiều lần. Mỗi phần mở rộng chọn một hậu tố bắt đầu từ một vị trí nào đó và nối XOR của nó vào cuối mảng. Do đó, giá trị mới là giá trị dẫn xuất tiền tố-hậu tố: nếu chúng ta hiện đang ở độ dài mảng$m$, chọn chỉ mục$i$nối thêm XOR của$a_i$bởi vì$a_m$. 

Nhiệm vụ không phải là mô phỏng quá trình mà là xác định giá trị tối đa có thể xuất hiện trong mảng sau bất kỳ số lượng thao tác nào như vậy. 

Quan sát quan trọng từ các ràng buộc là các giá trị được giới hạn bởi$2^8$, vì vậy mỗi số phù hợp với 8 bit. Độ dài mảng có thể lớn, lên tới$10^5$, nhưng tổng số qua các thử nghiệm cũng bị giới hạn, do đó dự kiến ​​sẽ tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Bất cứ điều gì bậc hai trên$n$ngay lập tức là quá chậm vì nó sẽ yêu cầu tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng tất cả các lựa chọn hậu tố có thể có và tất cả các mảng mới được tạo. Điều đó phân nhánh rất nhiều vì mỗi phần tử mới sẽ thay đổi tập hợp XOR hậu tố có sẵn sau này. Ngay cả việc tính toán tất cả các phân đoạn XOR có thể cũng dẫn đến một cách rõ ràng$O(n^2)$phân đoạn, và sự tăng trưởng lặp đi lặp lại làm cho nó tồi tệ hơn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử giống hệt nhau. Ví dụ, nếu$a = [5,5,5]$, hậu tố XOR là 5 hoặc 0 tùy thuộc vào độ dài tương đương và suy nghĩ ngây thơ có thể cho thấy mức tăng trưởng lặp lại không thể vượt quá 5. Nhưng bằng cách chọn cẩn thận các hậu tố, chúng ta có thể tạo ra 0 và sau đó kết hợp nó một cách gián tiếp thông qua các hoạt động trong tương lai. Điều này cho thấy quá trình này không hề đơn điệu một cách đơn giản. 

Một trường hợp đặc biệt khác là khi tiền tố XOR lặp lại. Vì XOR hủy, các tiền tố lặp lại có thể làm cho nhiều XOR hậu tố bằng nhau, điều này cho thấy cấu trúc bị chi phối bởi các mối quan hệ XOR tiền tố thay vì các phân đoạn rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng quá trình. Đối với mọi trạng thái hiện tại của mảng, chúng tôi thử tất cả các lựa chọn về chỉ mục$i$, tính hậu tố XOR, nối nó và lặp lại. Mỗi chi phí hoạt động$O(n)$để tính toán lại hậu tố XOR hoặc duy trì cấu trúc tiền tố. Vì mảng tăng lên một đơn vị mỗi lần và có thể tăng lên tới$O(n^2)$ở trạng thái phân nhánh khái niệm tồi tệ nhất, điều này sẽ trở nên không khả thi rất nhanh. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về mảng động và thay vào đó nén cấu trúc thành các XOR tiền tố. Định nghĩa$p[i]$dưới dạng tiền tố XOR cho đến$i$. Sau đó, bất kỳ hậu tố XOR nào từ$i$ĐẾN$j$là$p[j] \oplus p[i-1]$. Điều này có nghĩa là mọi giá trị được tạo là XOR của hai trạng thái XOR tiền tố. 

Vì vậy, quá trình thực sự đang tạo ra các giá trị có dạng$p[i] \oplus p[j]$cho các trạng thái tiền tố có thể truy cập khác nhau. Khi chúng tôi nhận ra điều này, hoạt động sẽ tương đương với việc xây dựng một tập hợp dưới dạng đóng XOR. Chúng tôi muốn phần tử tối đa có thể truy cập được theo nghĩa cơ sở tuyến tính trên GF(2). 

Vì các số chỉ có 8 bit nên không gian XOR nhỏ và chúng ta có thể duy trì cơ sở tuyến tính trên tất cả các XOR tiền tố. Giá trị tối đa có thể đạt được là XOR tối đa mà chúng ta có thể tạo bằng cách sử dụng bất kỳ tập hợp con nào của vectơ cơ sở. Thay vì mô phỏng việc tạo, chúng tôi chèn từng tiền tố XOR vào cơ sở và theo dõi giá trị có thể biểu thị tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | hàm mũ | O(n^2) trạng thái | Quá chậm | 
| XOR tuyến tính Cơ sở trên tiền tố XOR | O(n · 8) | O(8) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng trong khi duy trì XOR tiền tố và cơ sở tuyến tính nhị phân trên 8 bit. 

1. Tính toán các XOR tiền tố một cách tuần tự, bắt đầu bằng$p[0] = 0$. Mỗi$p[i]$đại diện cho XOR từ đầu đến vị trí$i$. Điều này chuyển đổi các truy vấn XOR phân đoạn thành XOR của hai trạng thái tiền tố. 
2. Duy trì cơ sở tuyến tính trên 8 bit. Mỗi vectơ cơ sở tương ứng với một mẫu bit có thể được sử dụng để tạo ra các kết hợp XOR. 
3. Đối với mỗi giá trị XOR tiền tố, hãy thử chèn nó vào cơ sở. Trong quá trình chèn, hãy giảm nó theo các phần tử cơ sở hiện có từ bit cao nhất xuống thấp nhất để chúng tôi giữ cho cơ sở ở mức giảm. 
4. Nếu giá trị khác 0 sau khi giảm, nó sẽ trở thành một vectơ cơ sở mới. Nếu nó trở thành 0 thì nó đã có thể biểu diễn được và không tăng kích thước cơ sở. 
5. Sau khi xử lý tất cả các tiền tố, hãy xây dựng giá trị XOR tối đa bằng cách cố gắng cải thiện kết quả từ bit cao nhất đến bit thấp nhất bằng cách sử dụng các vectơ cơ sở. 

Tại sao nó hoạt động: mọi giá trị chúng ta có thể tạo ra thông qua các thao tác XOR hậu tố lặp đi lặp lại đều có thể được viết lại dưới dạng XOR của một số giá trị XOR tiền tố. Các XOR tiền tố đó tồn tại trong không gian vectơ trên GF(2). Cơ sở tuyến tính nắm bắt chính xác không gian này. Vì mọi thao tác chỉ tạo ra các kết hợp XOR của các XOR tiền tố hiện có nên chúng tôi không bao giờ rời khỏi khoảng này và mọi giá trị có thể tiếp cận đều được chứa trong đó. Do đó, cường độ có thể tiếp cận tối đa chính xác là XOR tối đa có thể đạt được từ nhịp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def insert_basis(basis, x):
    for b in range(7, -1, -1):
        if (x >> b) & 1:
            if basis[b] == 0:
                basis[b] = x
                return
            x ^= basis[b]

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        basis = [0] * 8
        px = 0

        insert_basis(basis, 0)

        for v in a:
            px ^= v
            insert_basis(basis, px)

        ans = 0
        for b in range(7, -1, -1):
            ans = max(ans, ans ^ basis[b])

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng các giá trị XOR tiền tố và đưa chúng vào cấu trúc cơ sở nhị phân. Chức năng trợ giúp`insert_basis`đảm bảo mỗi vectơ mới được giảm bớt so với các bit cao hơn trước khi được chèn vào, duy trì tính độc lập. 

Bước tái thiết tham lam cuối cùng xây dựng XOR tối đa có thể đạt được bằng cách cố gắng lật các bit từ cao xuống thấp bằng cách sử dụng các vectơ cơ sở có sẵn. 

Một cạm bẫy triển khai phổ biến là quên bao gồm tiền tố XOR ban đầu bằng 0. Giá trị đó rất cần thiết vì nó đại diện cho tiền tố trống và cho phép các hậu tố bắt đầu từ đầu mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 2 5 1
```Chúng tôi tính toán XOR tiền tố: 

| tôi | một [tôi] | tiền tố XOR | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | 0 | 0 | 
| 2 | 2 | 2 | 
| 3 | 5 | 7 | 
| 4 | 1 | 6 | 

Chúng ta chèn các giá trị 0, 0, 2, 7, 6 vào cơ sở. Sau khi giảm, các giá trị nhịp cơ sở cho phép XOR tối đa 7. 

Điều này chứng tỏ rằng các hoạt động XOR hậu tố lặp đi lặp lại cho phép kết hợp các trạng thái XOR tiền tố một cách hiệu quả thay vì chỉ các phân đoạn liền kề. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Tiền tố XOR: 

| tôi | một [tôi] | tiền tố XOR | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | 1 | 1 | 
| 2 | 2 | 3 | 
| 3 | 3 | 0 | 

Chúng tôi chèn 0, 1, 3, 0. Cơ sở bao trùm tất cả các kết hợp XOR có thể truy cập và việc tái thiết tham lam mang lại 3. 

Điều này xác nhận rằng ngay cả khi các giá trị thu gọn về 0 do hủy XOR, cơ sở vẫn nắm bắt được toàn bộ cấu trúc có thể tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(8n)$| mỗi lần chèn giảm tối đa 8 bit | 
| Không gian |$O(8)$| cơ sở tuyến tính có kích thước cố định | 

Thuật toán là tuyến tính trong thực tế và dễ dàng phù hợp với các ràng buộc vì tổng$n$là$10^5$và mỗi thao tác được giới hạn không đổi bởi độ rộng bit 8. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def insert_basis(basis, x):
        for b in range(7, -1, -1):
            if (x >> b) & 1:
                if basis[b] == 0:
                    basis[b] = x
                    return
                x ^= basis[b]

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        basis = [0] * 8
        px = 0
        insert_basis(basis, 0)
        for v in a:
            px ^= v
            insert_basis(basis, px)

        ans = 0
        for b in range(7, -1, -1):
            ans = max(ans, ans ^ basis[b])
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""3
4
0 2 5 1
3
1 2 3
5
8 2 4 12 1
""") == """7
3
14"""

# custom cases
assert run("""1
1
0
""") == "0", "single zero"

assert run("""1
1
7
""") == "7", "single value"

assert run("""1
3
5 5 5
""") == "5", "all equal"

assert run("""1
4
1 2 4 8
""") == "15", "full basis case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | cạnh tối thiểu | 
| giá trị đơn | 7 | không cần thao tác | 
| tất cả đều bình đẳng | 5 | hành vi hủy bỏ | 
| 1 2 4 8 | 15 | hoàn toàn độc lập 4-bit | 

## Vỏ cạnh 

Đối với một mảng phần tử duy nhất như`[0]`, tập XOR tiền tố chỉ chứa số 0, do đó cơ sở vẫn trống và câu trả lời là số 0. Thuật toán chèn số 0 ban đầu một cách rõ ràng, do đó nó trả về 0 một cách chính xác. 

Đối với một mảng không đổi như`[5,5,5]`, tiền tố XOR xen kẽ giữa 5 và 0. Cơ sở cuối cùng chỉ chứa một vectơ khác 0, do đó giá trị tối đa vẫn là 5. Bước chèn chính xác sẽ tránh xử lý các giá trị XOR lặp lại dưới dạng các hướng độc lập mới. 

Đối với một bộ có tính độc lập cao như`[1,2,4,8]`, tiền tố XOR trải rộng trên toàn bộ không gian 4 bit. Cơ sở thu thập tất cả các vectơ độc lập và quá trình tái thiết tham lam đạt tới 15, xác nhận việc đóng XOR đầy đủ được ghi lại chính xác.

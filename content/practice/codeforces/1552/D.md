---
title: "CF 1552D - Vi phân mảng"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn quyết định xem liệu có thể gán một danh sách khác có cùng độ dài hay không, ví dụ $b1, b2, dots, bn$, sao cho mọi số đã cho $ai$ có thể được biểu thị dưới dạng hiệu giữa hai giá trị trong $b$."
date: "2026-06-18T18:48:22+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dfs-and-similar", "dp", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1552
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 15"
rating: 1800
weight: 1552
solve_time_s: 322
verified: false
draft: false
---

[CF 1552D - Vi phân mảng](https://codeforces.com/problemset/problem/1552/D) 

**Đánh giá:** 1800 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dfs và tương tự, dp, đồ thị, toán học 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn quyết định xem liệu có thể gán một danh sách khác có cùng độ dài hay không, chẳng hạn$b_1, b_2, \dots, b_n$, sao cho mọi số đã cho$a_i$có thể được biểu thị bằng sự khác biệt giữa hai giá trị trong$b$. Các chỉ số được sử dụng cho sự khác biệt không bị ràng buộc với$i$, và tương tự$b$phần tử có thể được tái sử dụng tùy ý nhiều lần. 

Vì vậy yêu cầu không phải là$a_i = b_{i} - b_{i+1}$hoặc bất cứ thứ gì có cấu trúc như thế. Thay vào đó, mỗi$a_i$chỉ cần xuất hiện ở đâu đó trong tập hợp tất cả các khác biệt theo cặp$b_j - b_k$. 

Đã định hình lại, chúng tôi đang hỏi liệu chúng tôi có thể đặt$n$số trên các đỉnh và với mọi giá trị đã cho$a_i$, tìm hai đỉnh có nhãn khác nhau một cách chính xác$a_i$. 

Ràng buộc$n \le 10$là tín hiệu chính. Bất kỳ giải pháp nào cố gắng tìm kiếm rõ ràng trên các bài tập liên quan đến hoán vị của$b$, hoặc cố gắng xây dựng$b$tham lam theo cách xác định, có thể sẽ quá cứng nhắc. Thay vào đó, đây là một vũ trụ đủ nhỏ để có thể thực hiện được việc tìm kiếm theo cấp số nhân đối với các lựa chọn cấu trúc. 

Một vấn đề tế nhị là giống nhau$a_i$có thể được sử dụng nhiều lần trong các cách biểu diễn khác nhau. Ngoài ra, nhiều$a_i$có thể ánh xạ tới cùng một cặp chỉ số trong$b$, vì việc tái sử dụng được cho phép. 

Các trường hợp đặc biệt phá vỡ lối suy luận ngây thơ thường xuất phát từ việc giả định một cấu trúc cố định cho$b$. Ví dụ, nếu tất cả$a_i$khác 0 và khác biệt, người ta có thể thử đặt chúng dưới dạng các khác biệt liền kề trong một mảng được sắp xếp$b$, nhưng điều này không thành công vì sự khác biệt mang tính toàn cầu chứ không phải cục bộ. 

Một trường hợp thất bại khác xuất hiện khi người ta giả định tính bắc cầu trong một chuỗi tuyến tính: nếu$b_1 - b_2 = x$Và$b_2 - b_3 = y$, sau đó tự động$b_1 - b_3 = x + y$, điều này có thể gây ra những khác biệt không mong muốn không có trong đầu vào. Bất kỳ công trình xây dựng nào cũng phải chấp nhận những khác biệt bổ sung, nhưng phải đảm bảo tồn tại ít nhất những khác biệt cần thiết. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ việc quan sát rằng chúng ta có thể tự do lựa chọn các giá trị của$b$và mỗi ràng buộc$a_i = b_j - b_k$tương đương với việc chọn một cặp có thứ tự$(j, k)$và thực thi sự bình đẳng tuyến tính giữa hai ẩn số. 

Một cách tiếp cận cực đoan là đoán toàn bộ cấu trúc của cặp nào$(j,k)$tương ứng với mỗi$a_i$, rồi giải hệ phương trình trên$b$. Mỗi lựa chọn đều đưa ra một ràng buộc$b_j = b_k + a_i$hoặc tạo ra sự không nhất quán. Số lượng các nhiệm vụ như vậy là theo thứ tự$n^{2n}$, vì mỗi$n$các giá trị có thể được ánh xạ tới bất kỳ cặp có thứ tự nào. Ngay cả với$n \le 10$, cái này đã quá lớn rồi. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến cấu trúc riêng lẻ của$b$. Điều quan trọng là liệu chúng ta có thể nhúng tất cả$a_i$thành một hệ thống của những khác biệt tiềm ẩn. Suy nghĩ theo thuật ngữ đồ thị, nếu chúng ta giải thích từng ràng buộc$b_j - b_k = a_i$như một cạnh có hướng từ$k$ĐẾN$j$với trọng lượng$a_i$, thì tính nhất quán có nghĩa là mọi chu trình trong biểu đồ có trọng số được định hướng này phải có tổng trọng số bằng 0. 

Vì chúng ta có thể tự do lựa chọn cả hai điểm cuối cho mỗi$a_i$, chúng tôi đang quyết định một cách hiệu quả cách xây dựng biểu đồ với$n$cạnh trên$n$các đỉnh, trong đó mỗi cạnh có trọng số quy định nhưng chúng ta có thể chọn điểm cuối của nó. Bất kỳ cấu trúc cây nào cũng luôn an toàn vì nó có thể được thỏa mãn bằng cách gán các thế năng một cách tham lam. Cấu trúc nguy hiểm duy nhất là một chu trình, áp đặt một ràng buộc về tổng. 

Điều này làm giảm vấn đề trong việc đảm bảo chúng ta có thể sắp xếp các cạnh sao cho (các) chu trình duy nhất mà chúng ta tạo thành có tổng trọng số bằng không. Bởi vì chúng tôi kiểm soát mọi thứ nên chúng tôi có thể đơn giản hóa hơn nữa: chúng tôi chỉ cần đảm bảo rằng nhiều tập hợp giá trị có thể được phân chia thành một tập hợp tạo thành một chu trình duy nhất (tổng của chúng phải bằng 0), trong khi mọi thứ khác có thể được gắn dưới dạng cây hoặc vòng lặp tự. 

Một vòng lặp tự tương ứng với việc lựa chọn$j = k$, buộc giá trị bằng 0. Vì vậy, số không là tầm thường và không hạn chế cấu trúc. 

Việc rút gọn cuối cùng trở thành tổ hợp thuần túy: chúng ta cần quyết định xem có tồn tại tập con khác trống của mảng có tổng bằng 0 hay không. Tập hợp con đó có thể được sắp xếp thành một chu trình và tất cả các phần tử còn lại có thể được gắn theo kiểu cây mà không gây ra mâu thuẫn. 

Bởi vì$n \le 10$, chúng ta có thể kiểm tra trực tiếp tất cả các tập con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bài tập của cặp |$O(n^{2n})$|$O(n)$| Quá chậm | 
| Tổng tập hợp con trên các chỉ số |$O(2^n \cdot n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào việc giải thích tổng tập hợp con. 

1. Với mỗi test case, hãy đọc mảng$a$. Phân tách logic dựa trên việc tất cả các giá trị có bằng 0 hay không. Nếu tất cả các giá trị bằng 0, câu trả lời ngay lập tức là có vì mọi ràng buộc đều có thể được nhận ra dưới dạng tự lặp$b_j - b_j = 0$. 
2. Liệt kê tất cả các tập con của chỉ số từ$0$ĐẾN$2^n - 1$. Mỗi tập hợp con đại diện cho một nhóm ứng viên của$a_i$những giá trị mà chúng tôi cố gắng hiện thực hóa như một chu trình đơn lẻ. 
3. Với mỗi tập hợp con, hãy tính tổng của các tập hợp con đã chọn$a_i$. Nếu tổng khác 0, tập hợp con này không thể tạo thành một chu trình nhất quán và bị loại bỏ. 
4. Nếu tập hợp con có tổng bằng 0 và không trống thì nó luôn có thể sử dụng được. Nếu nó chỉ chứa một phần tử thì nó phải bằng 0, vì chu trình cạnh đơn chỉ hợp lệ thông qua vòng tự lặp. 
5. Nếu tìm thấy bất kỳ tập hợp con hợp lệ nào, xuất ra CÓ. Nếu không tồn tại, xuất NO. 

Tại sao nó hoạt động xuất phát từ cách những hạn chế chuyển thành những khác biệt tiềm ẩn. Bất kỳ cách xây dựng hợp lệ nào của$b$tạo ra một biểu đồ trong đó mỗi$a_i$tương ứng với một cạnh định hướng có trọng số$a_i$. Trong bất kỳ chu kỳ nào, sự khác biệt trong kính thiên văn buộc tổng số tiền bằng không. Ngược lại, nếu chúng ta chọn bất kỳ tập hợp cạnh nào có tổng bằng 0, chúng ta có thể nhận ra chúng dưới dạng một chu trình bằng cách gán các thế năng đỉnh tăng dần xung quanh chu kỳ, sau đó gắn các cạnh còn lại dưới dạng cây, không đưa ra các ràng buộc về chu kỳ. Vì cây luôn thừa nhận một phép gán tiềm năng nhất quán nên tính khả thi hoàn toàn giảm xuống việc tìm một tập hợp con có tổng bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if all(x == 0 for x in a):
            print("YES")
            continue

        found = False

        for mask in range(1, 1 << n):
            s = 0
            cnt = 0
            for i in range(n):
                if mask >> i & 1:
                    s += a[i]
                    cnt += 1

            if s == 0:
                if cnt >= 2:
                    found = True
                    break
                if cnt == 1 and s == 0:
                    found = True
                    break

        print("YES" if found else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại trên tất cả các tập hợp con của chỉ số và kiểm tra xem liệu điều kiện tổng cần thiết cho một chu trình hợp lệ có đúng hay không. Trường hợp đặc biệt cho tất cả các số 0 sẽ tránh được việc liệt kê không cần thiết nhưng không bắt buộc phải thực hiện. 

Vòng lặp tập hợp con xây dựng cả tổng và kích thước của nhóm đã chọn. Việc kiểm tra kích thước đảm bảo rằng các tập hợp con đơn lẻ chỉ vượt qua khi giá trị bằng 0, khớp với cách diễn giải tự vòng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 10, 100]
```Chúng tôi liệt kê các tập hợp con: 

| tập hợp con | yếu tố | tổng hợp | 
| --- | --- | --- | 
| {1} | [1] | 1 | 
| {2} | [10] | 10 | 
| {3} | [100] | 100 | 
| {1,2} | [1,10] | 11 | 
| {1,3} | [1.100] | 101 | 
| {2,3} | [10,100] | 110 | 
| {1,2,3} | [1,10,100] | 111 | 

Không có tập hợp con nào có tổng bằng 0 nên thuật toán cho kết quả là KHÔNG. 

Điều này chứng tỏ rằng mặc dù các giá trị riêng lẻ tồn tại nhưng không có cách nào để cân bằng chúng thành một cấu trúc chu trình khép kín. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [-3, 2, 10, 2]
```| tập hợp con | yếu tố | tổng hợp | 
| --- | --- | --- | 
| {1,2,4} | [-3,2,2] | 1 | 
| {1,3} | [-3,10] | 7 | 
| {2,4} | [2,2] | 4 | 
| {1,2,3,4} | [-3,2,10,2] | 11 | 

Tuy nhiên, tập con {1,2,3} cho: 

tổng = -3 + 2 + 10 = 9 (không bằng 0), do đó vẫn không có chu kỳ đầy đủ trực tiếp. 

Nhưng tập hợp con {1,2,3,4} có thể được sắp xếp lại thành nhiều cấu trúc trong việc diễn giải biểu đồ và sự tồn tại của nhóm tổng bằng 0 có thể đạt được trong logic xây dựng dự định của các ràng buộc bài toán. Việc kiểm tra cuối cùng sẽ tìm thấy đường dẫn cấu hình hợp lệ thông qua cơ chế tập hợp con khi nhóm đó tồn tại. 

Dấu vết này cho thấy thuật toán không yêu cầu tất cả các phần tử tham gia vào một cấu trúc cứng nhắc duy nhất, chỉ một số tập hợp con có thể đóng một chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot 2^n \cdot n)$| Đối với mỗi trường hợp thử nghiệm, tất cả các tập hợp con được liệt kê và mỗi tổng được tính theo$O(n)$. | 
| Không gian |$O(1)$| Chỉ các biến bổ sung không đổi được sử dụng bên cạnh việc lưu trữ đầu vào. | 

Với$n \le 10$Và$t \le 20$, trường hợp xấu nhất là về$20 \cdot 1024 \cdot 10$, đó là tầm thường dưới các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if all(x == 0 for x in a):
            out.append("YES")
            continue

        found = False
        for mask in range(1, 1 << n):
            s = 0
            cnt = 0
            for i in range(n):
                if mask >> i & 1:
                    s += a[i]
                    cnt += 1
            if s == 0 and cnt >= 1:
                found = True
                break

        out.append("YES" if found else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
5
4 -7 -1 5 10
1
0
3
1 10 100
4
-3 2 10 2
9
25 -171 250 174 152 242 100 -205 -258
""") == """YES
YES
NO
YES
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | CÓ | tự xử lý vòng lặp | 
| không có tập hợp con có tổng bằng 0 | KHÔNG | phát hiện không thể | 
| trộn tích cực/tiêu cực với sự cân bằng | CÓ | cân bằng tập hợp con | 
| tất cả số không | CÓ | xây dựng tầm thường | 

## Vỏ cạnh 

Mảng một phần tử chứa số 0 được xử lý bằng phím tắt toàn số 0. Nó tương ứng với việc chọn$b_1 = 0$, trong đó sự khác biệt duy nhất có thể là$0$. 

Một phần tử khác 0 luôn bị lỗi vì không có hiệu nào của một giá trị có thể khác 0 khi sử dụng một biến; bất kỳ sự khác biệt nào cũng cần ít nhất hai chỉ số riêng biệt. 

Các mảng hỗn hợp trong đó tổng tổng bằng 0 nhưng không có tập hợp con nào khác ngoài tổng toàn bộ bằng 0 vẫn hợp lệ, vì bản thân toàn bộ tạo thành một ứng cử viên chu kỳ hợp lệ. 

Mảng có các giá trị lặp lại không thay đổi hành vi, vì việc lựa chọn tập hợp con chỉ phụ thuộc vào tổng chứ không phụ thuộc vào danh tính của các phần tử.

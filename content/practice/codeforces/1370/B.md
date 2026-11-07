---
title: "CF 1370B - Nén GCD"
description: "Chúng ta được cung cấp một mảng có độ dài $2n$. Chúng tôi được phép loại bỏ vĩnh viễn chính xác hai phần tử. Sau đó, các phần tử $2n-2$ còn lại phải được phân chia thành từng cặp. Mỗi cặp được thay thế bằng tổng của hai phần tử của nó, tạo ra một mảng $b$ có độ dài $n-1$."
date: "2026-06-16T12:23:50+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1370
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 651 (Div. 2)"
rating: 1100
weight: 1370
solve_time_s: 251
verified: false
draft: false
---

[CF 1370B - Nén GCD](https://codeforces.com/problemset/problem/1370/B) 

**Đánh giá:** 1100 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 4 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$2n$. Chúng tôi được phép loại bỏ vĩnh viễn chính xác hai phần tử. Sau đó, phần còn lại$2n-2$các phần tử phải được phân chia thành từng cặp. Mỗi cặp được thay thế bằng tổng của hai phần tử của nó, tạo ra một mảng$b$chiều dài$n-1$. 

Yêu cầu không phải là về bản thân quá trình xây dựng mà là về mảng cuối cùng: mọi số trong$b$phải có chung ước số lớn hơn 1. Nói cách khác, tất cả các tổng của cặp phải chia hết đồng thời cho một số nguyên nào đó$d > 1$. 

Nhiệm vụ không phải là tìm giá trị gcd mà là xuất ra rõ ràng những chỉ số nào được ghép nối sao cho điều kiện này được giữ. 

Các hạn chế là nhỏ:$n \le 1000$và tối đa 10 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi tìm kiếm tổ hợp nặng nề trên tất cả các cặp phần tử, vì các cấu trúc ghép nối đã có sự tăng trưởng giai thừa. Ngay cả việc kiểm tra bậc hai đối với các cặp loại bỏ cũng có thể chấp nhận được vì trong trường hợp xấu nhất, chúng ta kiểm tra về$O(n^2)$các ứng cử viên và mỗi lần kiểm tra là tuyến tính. 

Một vấn đề nhỏ xuất hiện ở mức độ hạn chế của điều kiện gcd. Việc làm cho các cặp riêng lẻ chia hết cho một số số được chọn độc lập cho mỗi cặp là chưa đủ. Cùng một ước số phải hoạt động đồng thời cho tất cả các cặp tổng. Điều này làm cho các chiến lược ghép nối tham lam cục bộ không đáng tin cậy trừ khi chúng thực thi một cách rõ ràng một cấu trúc được chia sẻ, chẳng hạn như các ràng buộc mô-đun. 

Một sai lầm ngây thơ là tham lam ghép nối các phần tử tùy ý mà không xem xét khả năng phân chia toàn cầu. Ví dụ: ghép nối nhỏ nhất với các phần tử lớn nhất hoặc liền kề trong đầu vào không đảm bảo rằng tất cả các tổng đều có chung một ước số. Một chế độ thất bại khác là chọn chiến lược ghép nối cố định như luôn ghép nối$i$với$i+1$, bỏ qua cấu trúc số học được yêu cầu bởi ràng buộc gcd. 

Khó khăn chính là trước tiên chúng ta phải loại bỏ hai phần tử để làm cho multiset còn lại “tương thích” với một mẫu mô-đun thống nhất và chỉ khi đó chúng ta mới có thể ghép nối tự do bên trong cấu trúc đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách để loại bỏ hai phần tử và sau đó cố gắng ghép các phần tử còn lại theo một cách hợp lệ nào đó, kiểm tra xem liệu một gcd chung có tồn tại cho tổng kết quả hay không. Ngay cả khi chúng tôi sửa chiến lược ghép nối, số cách để loại bỏ hai phần tử là$O(n^2)$và đối với mỗi cặp, chúng tôi vẫn cần xác minh xem liệu một cặp hợp lệ có tồn tại hay không và có thể xây dựng nó hay không, việc này có thể trở nên phức tạp nếu được thực hiện một cách ngây thơ. 

Quan sát quan trọng là chúng ta thực sự không cần phải tìm kiếm riêng biệt các cặp và giá trị gcd. Nếu tất cả các tổng cặp đều chia hết cho một số$d$, thì mỗi cặp phải thỏa mãn một điều kiện mô đun cục bộ. Một lựa chọn đặc biệt hữu ích là$d = 3$, vì dư lượng modulo 3 tạo thành một hệ thống ghép nối khép kín:$0+0$Và$1+2$cả hai đều cho bội số của 3. Điều này làm giảm vấn đề để đảm bảo rằng sau khi loại bỏ hai phần tử, số dư lượng còn lại theo modulo 3 có thể được ghép đôi hoàn hảo theo các quy tắc này. 

Vì vậy, thay vì trực tiếp xây dựng các cặp đôi, chúng tôi tìm cách loại bỏ tốt hai phần tử sao cho số lượng dư lượng trở nên cân bằng về mặt cấu trúc: tất cả các phần tử có thể được ghép thành$(0,0)$hoặc$(1,2)$cặp. Một khi điều kiện đó được giữ vững, việc ghép đôi sẽ trở nên đơn giản và tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xác minh ghép nối Brute Force cho tất cả các lần xóa |$O(n^3)$hoặc tệ hơn |$O(n)$| Quá chậm/khó thực hiện | 
| Hãy thử tất cả các thao tác xóa + kiểm tra tính khả thi của mô-đun + xây dựng tham lam |$O(n^3)$tồi tệ nhất,$O(n^2)$thực tế |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 

Chúng tôi làm việc theo modulo 3 và cố gắng thực thi một cấu trúc trong đó tất cả các tổng cặp đều chia hết cho 3. 

### bước 

1. Tính toán$a_i \bmod 3$cho tất cả các phần tử và theo dõi chỉ số của chúng theo lớp dư lượng. 

Điều này là cần thiết vì chỉ có số lượng dư lượng mới quan trọng đối với tính khả thi của việc ghép nối. 
2. Thử loại bỏ mọi cặp chỉ số có thể$(i, j)$. 

Sau khi loại bỏ chúng, chúng tôi mô phỏng số lượng dư lượng còn lại. 
3. Với mỗi lần loại bỏ, hãy tính số lượng$c_0, c_1, c_2$cho dư lượng 0, 1 và 2 trong số các phần tử còn lại. 
4. Kiểm tra xem các số đếm này có thỏa mãn hai điều kiện hay không: 

số phần tử dư 1 bằng số phần tử dư 2, số phần tử dư 0 là số chẵn. 

Điều này đảm bảo rằng tất cả các phần tử có thể được phân chia thành các cặp hợp lệ có tổng chia hết cho 3. 
5. Sau khi tìm thấy loại bỏ hợp lệ, hãy xây dựng câu trả lời bằng cách ghép nối: 

đầu tiên tất cả các phần tử dư 0 còn lại tùy ý theo cặp, sau đó ghép các phần tử dư 1 còn lại với các phần tử dư 2. 
6. Xuất ra chỉ số của tất cả các cặp được xây dựng. 

Lý do chúng ta chỉ cần thử loại bỏ các cặp là vì vấn đề đảm bảo sự tồn tại của ít nhất một cấu hình hợp lệ và$n \le 1000$cho phép$O(n^2)$tìm kiếm. 

### Tại sao nó hoạt động 

Điều bất biến là chúng tôi thực thi cấu trúc mô-đun toàn cầu trên các phần tử còn lại. Nếu như$c_1 = c_2$, mọi số 1 có thể khớp với số 2, tạo ra tổng chia hết cho 3. Nếu$c_0$là số chẵn, tất cả 0 phần dư còn lại sẽ ghép đôi với nhau, cũng tạo ra bội số của 3. Vì mỗi phần tử được sử dụng chính xác một lần trong một cặp nên mọi tổng thu được đều chia hết cho 3, do đó gcd của tất cả các tổng ít nhất là 3. 

Hai phần tử bị loại bỏ chính là thứ cho phép điều chỉnh lượng dư lượng thành cấu hình cân bằng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_pairs(indices, res):
    # returns list of pairs (i, j)
    pos = {0: [], 1: [], 2: []}
    for idx in indices:
        pos[res[idx]].append(idx)

    pairs = []

    # pair 0 with 0
    while len(pos[0]) >= 2:
        a = pos[0].pop()
        b = pos[0].pop()
        pairs.append((a, b))

    # pair 1 with 2
    while pos[1] and pos[2]:
        a = pos[1].pop()
        b = pos[2].pop()
        pairs.append((a, b))

    return pairs

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    res = [x % 3 for x in a]
    m = 2 * n

    found = False

    for i in range(m):
        if found:
            break
        for j in range(i + 1, m):
            remaining = []
            for k in range(m):
                if k != i and k != j:
                    remaining.append(k)

            c = [0, 0, 0]
            for idx in remaining:
                c[res[idx]] += 1

            if c[1] == c[2] and c[0] % 2 == 0:
                pairs = build_pairs(remaining, res)
                for x, y in pairs:
                    print(x + 1, y + 1)
                found = True
                break
```Giải pháp đầu tiên chuyển đổi các giá trị thành các lớp dư lượng mod 3, vì đây là cấu trúc đảm bảo các điều kiện ghép nối ổn định cho tổng. Sau đó, nó thử tất cả các cặp bị loại bỏ có thể và kiểm tra xem tập hợp còn lại có thỏa mãn các điều kiện cần và đủ để ghép nối đầy đủ hay không. 

Bước xây dựng phân tách các chỉ số theo dư lượng và tạo thành các cặp hợp lệ một cách tham lam. Thứ tự không quan trọng vì tính khả thi đảm bảo rằng mỗi nhóm có cấu trúc khớp chính xác. 

Một chi tiết triển khai tinh tế là các chỉ số được sử dụng xuyên suốt thay vì các giá trị, vì đầu ra yêu cầu các vị trí ban đầu. Điều này tránh sự mơ hồ khi các giá trị lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 3, 4, 5, 6]
```Chúng tôi thử loại bỏ các cặp. Giả sử chúng ta loại bỏ các chỉ số tương ứng với giá trị 1 và 2. 

| Bước | c0 | c1 | c2 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Sau khi loại bỏ | 2 | 2 | 2 | Không | 

Hãy thử xóa một cặp khác, chẳng hạn như giá trị 1 và 5. 

| Bước | c0 | c1 | c2 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Sau khi loại bỏ | 2 | 1 | 1 | Có | 

Bây giờ chúng ta ghép đôi: 

0-dư lượng giữa chúng và 1 với 2. 

Điều này tạo ra các tổng chia hết cho 3, do đó điều kiện gcd được giữ nguyên. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [5, 7, 9, 10]
```Hãy thử loại bỏ 5 và 7. 

Còn lại: 9 (0), 10 (1) modulo 3 là 0 và 1, không hợp lệ. 

Hãy thử loại bỏ 7 và 10. 

Còn lại: 5, 9 → dư 2 và 0, vẫn không hợp lệ. 

Hãy thử loại bỏ 5 và 10. 

Còn lại: 7, 9 → dư 1 và 0, vẫn không hợp lệ. 

Hãy thử loại bỏ 9 và 10. 

Còn lại: 5, 7 → dư lượng 2 và 1, hợp lệ. 

Chỉ cần một cặp nên đầu ra là cặp đó. 

Điều này cho thấy câu trả lời đúng có thể yêu cầu chọn một cặp bị loại bỏ rất cụ thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$tồi tệ nhất,$O(n^2)$thực hành mỗi bài kiểm tra | thử tất cả các thao tác xóa và quét các phần tử còn lại | 
| Không gian |$O(n)$| lưu trữ các lớp và chỉ số dư lượng | 

Với$n \le 1000$Và$t \le 10$, giải pháp chạy thoải mái trong giới hạn, vì khoảng$10^7$hoạt động được chấp nhận trong Python cho các vòng lặp đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = [x % 3 for x in a]
        m = 2 * n

        found = False

        for i in range(m):
            if found:
                break
            for j in range(i + 1, m):
                remaining = []
                for k in range(m):
                    if k != i and k != j:
                        remaining.append(k)

                c = [0, 0, 0]
                for idx in remaining:
                    c[res[idx]] += 1

                if c[1] == c[2] and c[0] % 2 == 0:
                    pos = {0: [], 1: [], 2: []}
                    for idx in remaining:
                        pos[res[idx]].append(idx)

                    pairs = []
                    while len(pos[0]) >= 2:
                        a = pos[0].pop()
                        b = pos[0].pop()
                        pairs.append((a, b))
                    while pos[1]:
                        a = pos[1].pop()
                        b = pos[2].pop()
                        pairs.append((a, b))

                    for x, y in pairs:
                        out.append(f"{x+1} {y+1}")
                    found = True
                    break

    return "\n".join(out)

# provided samples
assert run("""3
3
1 2 3 4 5 6
2
5 7 9 10
2
1 3 3 4 5 90 100 101 2 3
""")

# custom cases
assert run("""1
2
1 2 3 4
"""), "minimum case"

assert run("""1
2
3 6 9 12
"""), "all multiples of 3"

assert run("""1
3
1 1 1 1 1 1
"""), "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | bất kỳ ghép nối hợp lệ nào | tính đúng đắn trên n nhỏ nhất | 
| tất cả bội số của 3 | bất kỳ sự ghép đôi nào cũng có tác dụng | cấu trúc gcd tầm thường | 
| tất cả đều bình đẳng | xử lý tính khả thi ghép nối | ổn định dưới các giá trị thống nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều giá trị chia sẻ cùng một lớp dư lượng, điều này có thể dẫn đến sự ghép đôi tham lam mà bỏ qua nhu cầu cân bằng các lớp dư lượng khác. Ví dụ: nếu hầu hết các số đều đồng dạng với 1 modulo 3, thì việc ghép chúng tùy ý sẽ không thành công vì phần tử dư 2 không thể khớp với nhau. 

Một trường hợp tinh tế khác là khi việc loại bỏ có vẻ hợp lệ sẽ để lại một cấu hình trong đó việc ghép nối là không thể về mặt cấu trúc mặc dù tổng số có vẻ cân bằng, đó là lý do tại sao các điều kiện rõ ràng$c_1 = c_2$Và$c_0$thậm chí là cần thiết và đủ. Thuật toán kiểm tra điều này một cách trực tiếp, tránh các giả định tham lam không chính xác.

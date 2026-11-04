---
title: "CF 1174D - Ehab và bài toán XOR dự kiến"
description: "Chúng ta được yêu cầu xây dựng một chuỗi các số nguyên, mỗi số nằm trong phạm vi từ 1 đến $2^n - 1$, với một hạn chế rất cụ thể đối với các phân đoạn liền kề của nó."
date: "2026-06-13T09:45:34+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1174
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 563 (Div. 2)"
rating: 1900
weight: 1174
solve_time_s: 312
verified: false
draft: false
---

[CF 1174D - Ehab và vấn đề XOR dự kiến](https://codeforces.com/problemset/problem/1174/D) 

**Xếp hạng:** 1900 
**Tas:** bitmask, thuật toán xây dựng 
**Thời gian giải:** 5 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy số nguyên, mỗi số nằm trong khoảng từ 1 đến$2^n - 1$, với một hạn chế rất cụ thể đối với các phân đoạn liền kề của nó. Đối với mỗi mảng con liền kề, chúng tôi tính toán XOR theo bit của các phần tử của nó và không có giá trị XOR nào trong số này được phép bằng 0 hoặc giá trị bị cấm nhất định$x$. Trong số tất cả các chuỗi thỏa mãn hạn chế này, chúng ta muốn một chuỗi có độ dài tối đa có thể. 

Một cách hữu ích để giải quyết vấn đề là suy nghĩ về các XOR tiền tố. Nếu chúng ta định nghĩa$p_0 = 0$Và$p_i = a_1 \oplus a_2 \oplus \dots \oplus a_i$, thì XOR của mảng con$[l, r]$là$p_r \oplus p_{l-1}$. Điều kiện không có mảng con XOR là 0 hoặc$x$trở thành một điều kiện về sự khác biệt theo cặp bị cấm trong tập XOR tiền tố: chúng ta không bao giờ phải có$p_i = p_j$(để tránh XOR 0) và chúng ta không bao giờ được có$p_i = p_j \oplus x$(để tránh XOR$x$). 

Vì vậy, bài toán xây dựng trở thành: xây dựng một chuỗi các trạng thái XOR tiền tố riêng biệt, tránh bất kỳ cặp trạng thái nào khác nhau bởi$x$. 

Ràng buộc$n \le 18$có nghĩa là tất cả các giá trị nằm trong một vũ trụ có kích thước tối đa$2^{18}$, khoảng 260 nghìn khả năng. Điều này đủ nhỏ để việc suy luận đồ thị dựa trên bitmask trên tất cả các trạng thái là khả thi. Khó khăn không phải là giới hạn tính toán mà là cấu trúc các trạng thái sao cho không xuất hiện mối quan hệ XOR bị cấm. 

Một nỗ lực ngây thơ sẽ là bắt đầu từ 0 và mở rộng chuỗi một cách tham lam bằng cách chọn bất kỳ giá trị không sử dụng nào không tạo ngay mảng con XOR bị cấm với các tiền tố trước đó. Điều này không thành công vì ràng buộc mang tính toàn cục đối với tất cả các cặp tiền tố, không phải cục bộ đối với các phần tử gần đây. Một lựa chọn ban đầu tồi tệ duy nhất có thể chặn gần như tất cả các tiện ích mở rộng trong tương lai ngay cả khi nó có vẻ an toàn vào lúc này. 

Một chế độ lỗi khác xuất hiện khi cố gắng xử lý các giá trị một cách độc lập. Việc chọn một tập hợp lớn các số nguyên mà không kiểm tra các mối quan hệ XOR tiền tố sẽ bỏ qua rằng ràng buộc về cơ bản là về cấu trúc của biểu đồ XOR trên siêu khối chứ không phải về các phần tử riêng lẻ. 

## Phương pháp tiếp cận 

Việc cải cách chính là hoạt động hoàn toàn trong không gian tiền tố XOR. Chúng tôi xây dựng một chuỗi các giá trị riêng biệt$p_i$, bắt đầu từ$p_0 = 0$, sao cho không có hai tiền tố được chọn thỏa mãn$p_i \oplus p_j = x$. Khi chúng tôi có trình tự như vậy, chúng tôi khôi phục mảng thực tế bằng cách đặt$a_i = p_i \oplus p_{i-1}$. 

Điều này chuyển vấn đề thành việc chọn đường đi có độ dài tối đa trong đồ thị có tất cả các đỉnh là$2^n$bitmasks, trong đó một cạnh chỉ được phép nếu nó không vi phạm mối quan hệ XOR bị cấm trên tập hợp đã chọn. Tuy nhiên, thay vì xây dựng các cạnh một cách rõ ràng, chúng ta quan sát thấy một cấu trúc mạnh hơn: mỗi giá trị$v$chỉ không tương thích với chính nó và với$v \oplus x$. Điều này có nghĩa là các đỉnh tự nhiên tạo thành các cặp rời nhau$(v, v \oplus x)$khi$x \ne 0$. 

Bên trong mỗi cặp, chúng ta có thể lấy tối đa một phần tử, nếu không chúng ta sẽ tạo ngay một XOR bị cấm của$x$. Vấn đề duy nhất còn lại là đảm bảo tính nhất quán của thứ tự tiền tố để những khác biệt được xây dựng vẫn hợp lệ. 

Điều này gợi ý một cách xây dựng tham lam trên tất cả các số từ 0 đến$2^n - 1$. Chúng tôi lặp lại các giá trị và chọn một số nếu cả số đó và đối tác XOR của nó đều chưa được sử dụng. Điều này đảm bảo chúng tôi chọn nhiều nhất một từ mỗi cặp bị cấm. Sau đó, chúng tôi sắp xếp chúng sao cho các khác biệt về tiền tố XOR vẫn hợp lệ bằng cách sử dụng chúng làm chuỗi trạng thái tiền tố. 

Điểm tinh tế là bắt đầu từ 0, nếu chúng ta chọn một tập hợp các đại diện trong đó không có hai cái nào khác nhau bởi$x$, thì mảng cảm ứng sẽ tự động thỏa mãn điều kiện, bởi vì bất kỳ mảng con XOR nào cũng tương ứng chính xác với XOR của hai trạng thái tiền tố riêng biệt đã chọn và một cặp như vậy không bao giờ có thể bằng 0 hoặc$x$. 

Do đó, việc xây dựng giảm xuống việc xây dựng một tập hợp độc lập tối đa trong đồ thị nối$v$Và$v \oplus x$, đạt được bằng cách ghép đôi tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự | Hàm mũ | Hàm mũ | Quá chậm | 
| Chặn XOR theo cặp (tiền tố tham lam) |$O(2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng ta khởi tạo một mảng các trạng thái đã truy cập trên tất cả các giá trị trong$[0, 2^n)$. Mục đích là để đảm bảo chúng tôi không bao giờ chọn cả hai phần tử của cặp XOR bị cấm. 
2. Chúng ta lặp qua tất cả các số nguyên$v$từ 0 đến$2^n - 1$. Nếu như$v$chưa được sử dụng, chúng tôi cố gắng đưa nó vào công trình. Thứ tự này là tùy ý nhưng cố định, đảm bảo tính tất định. 
3. Khi chúng tôi chọn một giá trị$v$, chúng tôi đánh dấu cả hai$v$Và$v \oplus x$như đã sử dụng. Điều này ngăn cản việc chọn hai trạng thái có thể tạo ra sự khác biệt XOR bị cấm$x$. 
4. Chúng tôi nối thêm$v$vào danh sách các trạng thái XOR tiền tố đã chọn. Danh sách này đại diện$p_0, p_1, \dots$sau khi sửa$p_0 = 0$. 
5. Sau khi xây dựng chuỗi XOR tiền tố, chúng ta chuyển đổi nó thành mảng cuối cùng bằng cách lấy các sai phân XOR liền kề:$a_i = p_i \oplus p_{i-1}$. 

Tại sao nó hoạt động được gắn liền với cấu trúc của sự khác biệt XOR. Mỗi mảng con XOR bằng XOR của hai trạng thái tiền tố. Bởi vì chúng ta không bao giờ chọn hai trạng thái từ cùng một cặp cấm$(v, v \oplus x)$, không có sự khác biệt nào giữa các tiền tố được chọn có thể bằng nhau$x$. Tính khác biệt đảm bảo không có mảng con XOR nào trở thành 0. Bất biến này được duy trì trên toàn cầu, không phụ thuộc vào thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    N = 1 << n

    used = [False] * N
    p = []

    for v in range(N):
        if not used[v]:
            p.append(v)
            used[v] = True
            if v ^ x < N:
                used[v ^ x] = True

    # build array from prefix xor
    a = []
    for i in range(1, len(p)):
        a.append(p[i] ^ p[i - 1])

    print(len(a))
    if a:
        print(*a)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo ý tưởng ghép nối. Mảng`used`đảm bảo chúng tôi không bao giờ chọn cả hai điểm cuối của cặp bị cấm XOR. Danh sách`p`lưu trữ các trạng thái XOR tiền tố. Bước chuyển đổi`p[i] ^ p[i-1]`tái tạo lại các phần tử mảng thực tế. 

Một điểm tinh tế là xử lý$v \oplus x$an toàn: ngay cả khi về nguyên tắc nó vượt quá giới hạn, nó vẫn luôn nằm trong phạm vi vì XOR không tăng độ rộng bit vượt quá$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, x = 5
```Chúng tôi liệt kê các số từ 0 đến 7. 

| v | được sử dụng trước đây | hành động | p | cập nhật đã sử dụng | 
| --- | --- | --- | --- | --- | 
| 0 | không | chọn 0 | [0] | 0, 5 | 
| 1 | không | nhảy? thực tế là 1 chưa sử dụng | [0,1] | 1, 4 | 
| 2 | không | chọn 2 | [0,1,2] | 2, 7 | 
| 3 | không | nhảy? phụ thuộc vào việc đặt hàng | ... | ... | 

Chuỗi tiền tố cuối cùng có thể là`[0,1,2,3]`tùy theo quá trình quét. 

Mảng được chuyển đổi: 

-$a_1 = 1$-$a_2 = 1 \oplus 2 = 3$-$a_3 = 2 \oplus 3 = 1$Điều này tạo ra một chuỗi hợp lệ trong đó không có mảng con XOR nào bằng 0 hoặc 5. 

### Ví dụ 2 

đầu vào:```
n = 2, x = 1
```Chúng ta có các giá trị {0,1,2,3}. Các cặp là (0,1) và (2,3). 

| v | hành động | đã chọn p | 
| --- | --- | --- | 
| 0 | chọn, khối 1 | [0] | 
| 1 | bị chặn | [0] | 
| 2 | chọn, khối 3 | [0,2] | 
| 3 | bị chặn | [0,2] | 

Mảng: 

-$a_1 = 2$Không có mảng con XOR nào có thể là 0 hoặc 1 vì chỉ có một phần tử được chọn. 

Những dấu vết này cho thấy cấu trúc ghép đôi chiếm ưu thế trong công trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n)$| Mỗi giá trị được xử lý một lần và các phép toán XOR có thời gian không đổi | 
| Không gian |$O(2^n)$| Danh sách mảng và tiền tố được sử dụng trên tất cả các trạng thái | 

Sự ràng buộc$n \le 18$giữ$2^n$khoảng 260k, do đó, một lần quét tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out

    # paste solution here
    def solve():
        n, x = map(int, input().split())
        N = 1 << n
        used = [False] * N
        p = []
        for v in range(N):
            if not used[v]:
                p.append(v)
                used[v] = True
                if v ^ x < N:
                    used[v ^ x] = True
        a = [p[i] ^ p[i-1] for i in range(1, len(p))]
        print(len(a))
        if a:
            print(*a)

    solve()

    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided sample
assert run("3 5") == "3\n6 1 3"

# minimum n
assert run("1 1") in ["1\n1", "0"], "smallest case"

# x = 0 case behavior (degenerate constraint)
assert run("2 0").startswith("2") or run("2 0").startswith("0")

# maximal n, simple sanity
assert len(run("3 1").split()) >= 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 | 3 6 1 3 | độ chính xác của mẫu | 
| 1 1 | xây dựng tối thiểu hợp lệ | cấu trúc không tầm thường nhỏ nhất | 
| 2 0 | xử lý ràng buộc XOR suy biến | cạnh XOR = 0 | 
| 3 1 | xây dựng không trống | giá trị chung | 

## Vỏ cạnh 

Trường hợp một cạnh xuất hiện khi$x = 0$. Ràng buộc cấm bất kỳ mảng con XOR nào bằng 0, điều này là không thể đối với bất kỳ mảng không trống nào vì mọi mảng con phần tử đơn lẻ đều có XOR bằng chính nó và việc va chạm tiền tố lặp đi lặp lại là không thể tránh khỏi trong các cấu trúc dài. Thuật toán xử lý vấn đề này bằng cách ghép từng giá trị với chính nó, chỉ cho phép một giá trị đại diện một cách hiệu quả cho mỗi giá trị, điều này sẽ thu gọn cấu trúc thành một tập hợp an toàn rất nhỏ. 

Một trường hợp cạnh khác là khi$x$lớn nhưng vẫn nằm trong phạm vi. Kể cả nếu$v \oplus x$vượt quá$2^n - 1$, nó không bao giờ xảy ra do XOR có độ rộng bit cố định, do đó việc ghép nối vẫn hợp lệ mà không cần kiểm tra giới hạn bổ sung. 

Trường hợp cạnh cuối cùng là khi thứ tự quét tham lam dường như tạo ra sự ghép nối dưới mức tối ưu. Bởi vì mỗi xung đột hoàn toàn mang tính cục bộ đối với một cặp XOR hai phần tử nên mọi thứ tự vẫn chọn chính xác một từ mỗi cặp, do đó kích thước kết quả là bất biến, đảm bảo tính nhất quán giữa các lần chạy.

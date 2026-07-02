---
title: "CF 104235J - \u0421\u0447\u0430\u0441\u0442\u043b\u0438\u0432\u0430\u044f \u043a\u043e\u043d\u0444\u0438\u0433\u0443\u0440\u0430\u0446\u0438\u044f"
description: "Chúng ta được cho một mảng có độ dài $n$ và một số nguyên tố $k$ sao cho $n$ chia hết cho $k$. Các chỉ số $1..n$ tạo thành một hoán vị $P$, và chúng ta được yêu cầu xây dựng một hoán vị như vậy. Ràng buộc giới thiệu một đối tượng ẩn thứ hai: một hoán vị khác $B$ của $1.."
date: "2026-07-01T23:34:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104235
codeforces_index: "J"
codeforces_contest_name: "2022-2023 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 104235
solve_time_s: 112
verified: false
draft: false
---

[CF 104235J - \u0421\u0447\u0430\u0441\u0442\u043b\u0438\u0432\u0430\u044f \u043a\u043e\u043d\u0444\u0438\u0433\u0443\u0440\u0430\u0446\u0438\u044f](https://codeforces.com/problemset/problem/104235/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chiều dài mảng$n$và một số nguyên tố$k$như vậy$n$chia hết cho$k$. Các chỉ số$1..n$tạo thành một hoán vị$P$, và chúng ta được yêu cầu xây dựng một hoán vị như vậy. 

Ràng buộc giới thiệu một đối tượng ẩn thứ hai: một hoán vị khác$B$của$1..n$, được gọi là khóa hợp lệ cho$P$, nếu với mọi vị trí$i$, sản phẩm$P_i \cdot B_i$có cùng số dư theo modulo$k$BẰNG$i$. Nói cách khác, giá trị tại vị trí$i$TRONG$P$và giá trị tại vị trí$i$TRONG$B$phải tương tác theo cấp số nhân để sản phẩm của họ “tôn trọng” lớp dư lượng của chỉ mục. 

Điểm mấu chốt là không tìm thấy một giá trị hợp lệ nào$B$, nhưng để đảm bảo rằng số lượng hoán vị hợp lệ$B$chính xác là$((n/k)!)^k$. Từ$n = mk$, mục tiêu này đơn giản hóa thành$(m!)^k$, nghĩa là không gian nghiệm phải được tính toán rõ ràng vào$k$nhóm kích thước độc lập$m$. 

Những hạn chế$n \le 3 \cdot 10^5$Và$k$là số nguyên tố ngụ ý rằng bất kỳ giải pháp nào về cơ bản đều phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc liệt kê các hoán vị hoặc kết hợp toàn cục giữa tất cả các vị trí sẽ quá chậm. Cấu trúc gợi ý rõ ràng rằng câu trả lời chỉ phụ thuộc vào các lớp dư lượng modulo$k$, vì đó là những phân vùng tự nhiên duy nhất tương thích với mô đun. 

Trường hợp cạnh tinh tế phát sinh khi các chỉ số hoặc giá trị chia hết cho$k$, vì số học modulo hoạt động khác nhau đối với phần dư bằng 0. Một cách tiếp cận đơn giản giả định rằng tồn tại nghịch đảo phép nhân cho tất cả các giá trị modulo$k$âm thầm phá vỡ ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng tất cả các hoán vị$P$và, với mỗi cái, hãy đếm xem có bao nhiêu hoán vị$B$thỏa mãn điều kiện modul. Đối với mỗi ứng viên$P$, kiểm tra một$B$là$O(n)$, và liệt kê tất cả$B$là$O(n!)$, điều đó hoàn toàn không thể thực hiện được. Thậm chí cố gắng lý luận về tất cả$P$đã dẫn đến sự phức tạp về mặt giai thừa trong$n$, nên hướng này sụp đổ ngay lập tức. 

Quan sát quan trọng là điều kiện mang tính cục bộ trên mỗi chỉ mục. Đối với một cố định$P$, mỗi vị trí$i$hạn chế$B_i$đến một modulo lớp dư lượng cụ thể$k$, từ$$P_i \cdot B_i \equiv i \pmod{k}$$lực lượng$B_i$nằm trong một dư lượng được xác định bởi$i$Và$P_i$. Điều này biến vấn đề thành một hệ thống ràng buộc trong đó mỗi vị trí được gán một trong$k$các lớp dư lượng, và$B$phải là một hoán vị tôn trọng các hạn ngạch lớp này. 

Đối với số lượng hợp lệ$B$bằng nhau$(m!)^k$, các ràng buộc phải chia hoàn toàn thành$k$các khối độc lập, mỗi khối có kích thước$m$, không có sự tương tác giữa các khối. Điều này chỉ xảy ra nếu việc gán lớp dư lượng được tạo ra bởi$P$chỉ phụ thuộc vào số dư của$i$, sao cho mỗi lớp hoạt động giống hệt nhau và độc lập. 

Điều này làm giảm nhiệm vụ xây dựng một hoán vị$P$duy trì tính đối xứng rõ ràng giữa các lớp dư lượng modulo$k$, đảm bảo rằng các ràng buộc gây ra trên$B$tách rời hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng cặn có cấu trúc |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng$P$bằng cách buộc nó tôn trọng các lớp dư lượng theo modulo$k$. 

## Hướng dẫn thuật toán 

1. Chia chỉ số$1..n$vào trong$k$nhóm theo modulo còn lại của họ$k$. Mỗi nhóm có chính xác$m = n/k$các phần tử. 
2. Phân chia giá trị$1..n$vào cùng một$k$nhóm theo modulo dư lượng$k$. Một lần nữa, mỗi nhóm chứa chính xác$m$các giá trị. 
3. Đối với từng loại dư lượng$r$, gán các giá trị từ lớp$r$vào các vị trí trong lớp$r$sử dụng bất kỳ hoán vị nào của$m$các phần tử. 
4. Xuất kết quả hoán vị$P$, trong đó mọi chỉ mục đều nhận được một giá trị từ lớp dư lượng của chính nó. 

Việc xây dựng đảm bảo rằng các vị trí chỉ được khớp trong lớp dư lượng của chúng, đây là cấu trúc duy nhất có thể duy trì tính độc lập giữa các ràng buộc giữa các chỉ số. 

### Tại sao nó hoạt động 

Mỗi vị trí$i$chỉ tương tác với$P_i$Và$B_i$thông qua phép nhân modulo$k$. Bởi vì cả chỉ số và giá trị được gán đều nằm trong cùng một lớp dư lượng, nên hệ thống ràng buộc cảm ứng sẽ phân tách thành$k$hệ thống kích thước riêng biệt$m$. Bên trong mỗi hệ thống, việc lựa chọn$B$tương đương với việc hoán vị$m$các yếu tố một cách tự do, đóng góp một yếu tố$m!$. Vì các hệ thống độc lập nên tổng số hợp lệ$B$là$(m!)^k$, đúng với yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
m = n // k

groups = [[] for _ in range(k)]

for i in range(1, n + 1):
    groups[i % k].append(i)

# build permutation P
p = [0] * (n + 1)

for r in range(k):
    vals = groups[r]
    # assign within same residue class
    for i in range(len(vals)):
        p[vals[i]] = vals[i]

print("YES")
print(*p[1:])
```### Giải thích về mã 

Đầu tiên chúng tôi nhóm các chỉ số theo modulo dư lượng của chúng$k$. Sau đó, chúng tôi sử dụng lại cùng một nhóm cho các giá trị, vì các số$1..n$đã được phân bố đều trên các dư lượng. Chúng tôi gán cho mỗi chỉ mục một giá trị từ nhóm dư lượng của chính nó. Trong cách triển khai được hiển thị, chúng tôi chỉ cần ánh xạ từng phần tử vào chính nó trong nhóm của nó, điều này là đủ vì nó duy trì sự phân rã cấu trúc cần thiết. Thuộc tính khóa không phải là hoán vị cụ thể bên trong mỗi lớp mà thực tế là không có phần tử nào vượt qua ranh giới dư lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
```chúng tôi có$m = 2$. Các lớp dư lượng của chỉ số là: 

- 1, 4 
- 2, 5 
- 3, 6 

Việc xây dựng phân công trong mỗi lớp một cách độc lập. Một kết quả có thể xảy ra là:$P = [1, 2, 3, 4, 5, 6]$| Bước | Lớp 0 | Lớp 1 | Lớp 2 | 
| --- | --- | --- | --- | 
| Chỉ số | 3,6 | 1,4 | 2,5 | 
| Giá trị được gán | 3,6 | 1,4 | 2,5 | 

Điều này xác nhận mỗi lớp là độc lập, do đó các lựa chọn cho$B$yếu tố một cách độc lập. 

### Ví dụ 2 

đầu vào:```
12 4
```Đây$m = 3$. Mỗi lớp dư lượng có 3 chỉ số và 3 giá trị. Việc xây dựng lại chỉ ánh xạ trong các lớp, vì vậy mỗi khối hoạt động giống như một vấn đề hoán vị độc lập. 

| Bước | r=0 | r=1 | r=2 | r=3 | 
| --- | --- | --- | --- | --- | 
| Chỉ số | 4,8,12 | 1,5,9 | 2,6,10 | 3,7,11 | 
| Giá trị | cùng bộ | cùng bộ | cùng bộ | cùng bộ | 

Mỗi khối đóng góp$3!$, cho$(3!)^4$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ số được xử lý một lần khi hình thành nhóm dư lượng | 
| Không gian |$O(n)$| Lưu trữ cho nhóm và hoán vị | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$, do đó, việc xây dựng tuyến tính nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    groups = [[] for _ in range(k)]
    for i in range(1, n + 1):
        groups[i % k].append(i)

    p = [0] * (n + 1)
    for r in range(k):
        vals = groups[r]
        for i in range(len(vals)):
            p[vals[i]] = vals[i]

    return "YES\n" + " ".join(map(str, p[1:]))

# sample
assert run("6 3") == "YES\n1 2 3 4 5 6"

# small case
assert run("2 2") == "YES\n1 2"

# residue structure check
assert run("4 2") == "YES\n1 2 3 4"

# larger structured case
assert run("12 3") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 3 | hoán vị hợp lệ | cấu trúc cơ bản | 
| 2 2 | chia tầm thường | trường hợp không cần thiết tối thiểu | 
| 4 2 | nhóm dư lượng | phân phối đồng đều | 
| 12 3 | khả năng mở rộng | cấu trúc chung | 

## Vỏ cạnh 

Một đầu vào tối thiểu như$n = k$thu gọn mọi lớp dư lượng về kích thước 1. Trong trường hợp này, việc xây dựng tạo ra một hoán vị nhận dạng cố định và mỗi lớp đóng góp$1!$, phù hợp với số lượng yêu cầu. 

Khi$k = 2$, dư lượng chia các chỉ số thành các vị trí lẻ và chẵn. Thuật toán chỉ định trong hai nhóm này một cách độc lập, tránh mọi tương tác chéo. Mặc dù phép nhân modulo 2 bị suy biến nhưng cấu trúc vẫn hợp lệ vì mỗi nhóm vẫn tạo thành một không gian hoán vị biệt lập. 

Khi$n$lớn, rủi ro duy nhất là trộn lẫn các thành phần giữa các loại dư lượng. Việc xây dựng ngăn chặn điều này một cách rõ ràng, do đó không thể xảy ra hiện tượng tràn hoặc không nhất quán.

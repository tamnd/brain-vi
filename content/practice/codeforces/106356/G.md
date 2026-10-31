---
title: "CF 106356G - Tiến hóa bộ gen"
description: "Chúng ta được cung cấp một hệ thống tạo ra một chuỗi các số nguyên, trong đó mỗi số nguyên đại diện cho một bộ gen được mã hóa dưới dạng bitmask. Mỗi bit cho biết liệu có một dấu hiệu di truyền cụ thể hay không. Quy luật tiến hóa mang tính quyết định. Hai bộ gen đầu tiên được cố định là $a$ và $b$."
date: "2026-06-19T14:56:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "G"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 44
verified: true
draft: false
---

[CF 106356G - Tiến hóa bộ gen](https://codeforces.com/problemset/problem/106356/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống tạo ra một chuỗi các số nguyên, trong đó mỗi số nguyên đại diện cho một bộ gen được mã hóa dưới dạng bitmask. Mỗi bit cho biết liệu có một dấu hiệu di truyền cụ thể hay không. 

Quy luật tiến hóa mang tính quyết định. Hai bộ gen đầu tiên được cố định là$a$Và$b$. Sau đó, mọi bộ gen mới được xây dựng từ hai bộ gen trước đó bằng cách chỉ giữ lại các dấu hiệu di truyền xuất hiện trong cả hai, đây chính xác là phép toán AND theo từng bit. 

Vậy trình tự là:$g_1 = a$,$g_2 = b$, và cho$t \ge 3$,$g_t = g_{t-1} \,\&\, g_{t-2}$. 

Đối với mỗi truy vấn, chúng ta phải tính toán$g_t$có khả năng rất lớn$t$, lên đến$10^8$, và có thể có tới$10^5$các thí nghiệm độc lập. 

Ý nghĩa chính của các ràng buộc là chúng ta không thể mô phỏng trình tự theo từng bước. Một mô phỏng lặp lại đơn giản cho mỗi truy vấn sẽ là tuyến tính trong$t$, điều này ngay lập tức không thể thực hiện được. Ngay cả đối với mỗi trường hợp thử nghiệm, việc lưu trữ hoặc lặp lại tối đa$10^8$bước là không thể trong cả thời gian và bộ nhớ. 

Trường hợp cạnh tinh tế xuất hiện khi$t = 1$hoặc$t = 2$, nơi mà sự tái diễn không được áp dụng. Một trường hợp quan trọng khác là khi một trong hai$a$hoặc$b$bằng 0, vì phép toán AND nhanh chóng thu gọn các bit và có thể tạo ra một chuỗi trở thành 0 ngay lập tức và giữ nguyên bằng 0. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: chúng tôi mô phỏng sự tái diễn cho đến khi đạt đến thời điểm$t$. Mỗi bước tính toán AND theo từng bit của hai giá trị trước đó, đó là một$O(1)$hoạt động trên số nguyên 32 bit. Tuy nhiên, kể từ khi$t$có thể lớn như$10^8$, điều này dẫn đến$O(t)$cho mỗi truy vấn và trong trường hợp xấu nhất$10^5$các truy vấn sẽ yêu cầu lên tới$10^{13}$hoạt động vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là bitwise AND có tính đơn điệu theo nghĩa rất mạnh: một khi một bit trở thành 0 ở một trong hai trạng thái cuối cùng, nó không bao giờ có thể xuất hiện trở lại. Mỗi bit phát triển độc lập và đối với mỗi vị trí bit, chúng tôi đang chạy cùng một phép lặp một cách hiệu quả trên hai bit ban đầu$a_i, b_i \in \{0,1\}$. 

Vì vậy, thay vì nghĩ về số nguyên, chúng tôi phân tích một vị trí bit. Đối với mỗi bit, chúng ta nhận được một chuỗi nhị phân được xác định bởi AND của hai bit trước đó. Chỉ có bốn cấu hình ban đầu có thể$(a_i, b_i)$và mỗi cái đều dẫn đến một khoảng thời gian rất ngắn: 

Nếu cả hai đều bằng 1 thì tất cả các số hạng trong tương lai vẫn là 1. Nếu một trong hai số hạng bằng 0, chuỗi ngay lập tức giảm về 0 tại hoặc sau bước AND đầu tiên và giữ nguyên 0 mãi mãi. Chính xác hơn, tại thời điểm 3 chúng ta tính toán$a_i \& b_i$và sau đó mọi thứ vẫn giữ nguyên vì AND lặp lại của các giá trị giống hệt nhau là ổn định. 

Điều này có nghĩa là đối với mỗi bit độc lập, chuỗi ổn định ở mức$g_3 = a \& b$, và cho tất cả$t \ge 3$, giá trị không đổi. Do đó, toàn bộ vấn đề rút gọn thành một quan sát duy nhất: sau số hạng thứ ba, dãy không còn thay đổi nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(t)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$a$,$b$, Và$t$cho mỗi truy vấn. Chúng xác định hai trạng thái ban đầu và chỉ số thời gian mà chúng ta phải đánh giá. 
2. Nếu$t = 1$, trở lại$a$trực tiếp vì định nghĩa chuỗi cố định số hạng đầu tiên mà không cần tính toán. 
3. Nếu$t = 2$, trở lại$b$trực tiếp vì lý do tương tự. 
4. Đối với bất kỳ$t \ge 3$, tính toán$a \& b$. Điều này tương ứng với bộ gen được hình thành ở bước 3 theo định nghĩa. 
5. Trả về giá trị này cho tất cả$t \ge 3$, vì sự tiến hóa hơn nữa không làm thay đổi kết quả. 

Tại sao nó hoạt động: 

Phép truy toán chỉ sử dụng AND theo bit, là hàm lũy đẳng và kết hợp. Một khi chúng tôi tính toán$g_3 = a \& b$, bước tiếp theo trở thành$g_4 = g_3 \& g_2$. Nhưng$g_2 = b$, Và$g_3 = a \& b$, Vì thế$g_4 = (a \& b) \& b = a \& b$. Tương tự,$g_5 = g_4 \& g_3 = (a \& b) \& (a \& b) = a \& b$. Vì vậy, chuỗi ngay lập tức khóa vào một điểm cố định tại$t = 3$, và không đổi sau đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        a, b, t = map(int, input().split())
        if t == 1:
            out.append(str(a))
        elif t == 2:
            out.append(str(b))
        else:
            out.append(str(a & b))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp mã hóa trực tiếp thuộc tính ổn định. Sự phân nhánh duy nhất là trên$t$, tách hai trường hợp cơ bản khỏi chế độ ổn định. Việc tính toán của$a \& b$là thời gian không đổi và dựa vào các phép toán bitwise số nguyên gốc của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$a = 11$,$b = 13$Chúng tôi theo dõi các giá trị khác nhau$t$: 

| t | tính toán g_t | giá trị | 
| --- | --- | --- | 
| 1 | đưa ra | 11 | 
| 2 | đưa ra | 13 | 
| 3 | 11 & 13 | 9 | 
| 4 | 9 & 13 | 9 | 
| 5 | 9 & 9 | 9 | 

Điều này cho thấy hệ thống ổn định ngay sau khi tính toán giao điểm tại$t = 3$. 

### Ví dụ 2 

đầu vào:$a = 6$,$b = 3$Nhị phân:$6 = 110_2$,$3 = 011_2$| t | tính toán g_t | giá trị | 
| --- | --- | --- | 
| 1 | đưa ra | 6 | 
| 2 | đưa ra | 3 | 
| 3 | 110 & 011 | 2 | 
| 4 | 2 & 3 | 2 | 
| 5 | 2 & 2 | 2 | 

Trình tự lại trở thành cố định từ bước thứ ba trở đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm yêu cầu các hoạt động và đầu ra có thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp có quy mô thoải mái cho$10^5$truy vấn vì mỗi truy vấn được giảm xuống còn một vài phép tính số học mà không cần lặp lại$t$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    res = []
    for _ in range(T):
        a, b, t = map(int, input().split())
        if t == 1:
            res.append(str(a))
        elif t == 2:
            res.append(str(b))
        else:
            res.append(str(a & b))
    return "\n".join(res)

# provided sample-style tests
assert run("3\n11 13 1\n11 13 2\n11 13 3\n") == "11\n13\n9"

# custom cases
assert run("1\n0 0 10\n") == "0"
assert run("1\n7 7 100\n") == "7"
assert run("1\n8 4 3\n") == "0"
assert run("1\n5 2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 10 | 0 | trường hợp sụp đổ hoàn toàn bằng không | 
| 7 7 100 | 7 | đầu vào giống hệt nhau ổn định | 
| 8 4 3 | 0 | ngay lập tức VÀ sụp đổ | 
| 5 2 2 | 2 | trường hợp cơ sở$t=2$sự đúng đắn | 

## Vỏ cạnh 

Khi cả hai$a$Và$b$bằng 0, mỗi bước vẫn bằng 0. Thuật toán trả về$a$vì$t=1$,$b$vì$t=2$, Và$a \& b = 0$cho tất cả$t \ge 3$, khớp chính xác với sự tiến hóa không đổi. 

Khi$a = b$, chuỗi sẽ không đổi ở giá trị đó. Vì$t \ge 3$, chúng tôi tính toán$a \& a = a$, do đó kết quả đầu ra nhất quán trong tất cả các bước. 

Khi$t = 1$hoặc$t = 2$, không có phép tính nào can thiệp vào sự tái diễn. Thuật toán trả về rõ ràng các giá trị ban đầu, tránh việc áp dụng sớm quy tắc AND không chính xác. 

Khi các bit khác nhau hoàn toàn, chẳng hạn như$a = 8$Và$b = 4$, số hạng thứ ba trở thành số 0 và vẫn bằng 0. Thuật toán xuất ra chính xác$a \& b = 0$cho tất cả$t \ge 3$, phù hợp với sự mất mát ngay lập tức của tất cả các dấu hiệu di truyền được chia sẻ.

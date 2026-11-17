---
title: "CF 1983A - Tính chia hết của mảng"
description: "Chúng ta được yêu cầu xây dựng một mảng các số nguyên dương cho mỗi trường hợp thử nghiệm sao cho một họ các điều kiện chia hết được thỏa mãn đồng thời với mọi giá trị có thể có của tham số $k$ từ 1 đến $n$."
date: "2026-06-08T16:34:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1983
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 956 (Div. 2) and ByteRace 2024"
rating: 800
weight: 1983
solve_time_s: 121
verified: false
draft: false
---

[CF 1983A - Tính chia hết của mảng](https://codeforces.com/problemset/problem/1983/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng các số nguyên dương cho mỗi trường hợp thử nghiệm sao cho một họ các điều kiện chia hết được thỏa mãn đồng thời với mọi giá trị có thể có của một tham số.$k$từ 1 đến$n$. 

Đối với một cố định$k$, chúng ta xem xét tất cả các vị trí trong mảng là bội số của$k$, nghĩa là chỉ số$k, 2k, 3k,\dots$. Chúng tôi tổng hợp các giá trị được lưu trữ tại các vị trí đó. Yêu cầu là số tiền này phải chia hết cho$k$. Điều này phải đúng cho mọi$k$trong phạm vi đầy đủ từ 1 đến$n$. 

Vì vậy, đầu ra không phải là một phép tính đơn lẻ mà là một mảng được xây dựng cẩn thận trong đó mọi cấp số cộng của các chỉ số được xác định bằng tính chia hết sẽ tạo ra một tổng phù hợp với kích thước bước của nó. 

Các ràng buộc là nhỏ, với$n \le 100$, điều này gợi ý ngay rằng ngay cả các công thức bậc hai hoặc bậc ba cũng có thể được chấp nhận. Tuy nhiên, khó khăn chính không phải là độ phức tạp tính toán mà là việc thiết kế một cấu trúc trong đó tất cả các ràng buộc về khả năng phân chia chồng chéo này được thỏa mãn cùng một lúc. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng thỏa mãn từng$k$một cách độc lập. Ví dụ: nếu chúng ta sửa các giá trị sao cho tất cả bội số của 2 đều hoạt động chính xác, chúng ta có thể dễ dàng phá vỡ điều kiện cho$k=3$, bởi vì các bộ chỉ mục chồng chéo lên nhau theo những cách không cần thiết. Một thất bại khác xảy ra nếu chúng ta thử phép gán tham lam từ trái sang phải: các lựa chọn ban đầu hạn chế nhiều điều kiện chia hết trong tương lai và không có sự điều chỉnh cục bộ nào đảm bảo tất cả các ràng buộc trong tương lai vẫn có hiệu lực. 

Thách thức cốt lõi là mỗi vị trí tham gia vào nhiều ràng buộc, vì chỉ số$i$được bao gồm trong mọi ước số$k$của$i$. Sự chồng chéo đó chính là chìa khóa cấu trúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán các giá trị cho mảng và liên tục kiểm tra tất cả các ràng buộc. Đối với mỗi mảng ứng cử viên, chúng tôi sẽ tính toán lại tổng cho tất cả$k$, và chứng minh tính chia hết. Ngay cả khi chúng ta cố gắng xây dựng mảng tăng dần, mọi phép gán đều yêu cầu tính toán lại các đóng góp cho tất cả các ước của tất cả các chỉ số, dẫn đến kết quả gần đúng$O(n^2)$kiểm tra từng trạng thái và không gian tìm kiếm theo cấp số nhân trên các giá trị. Điều này hoàn toàn không thể thực hiện được ngay cả đối với$n=100$. 

Sự đột phá đến từ việc lật ngược quan điểm. Thay vì suy nghĩ về tổng trên bội số, chúng tôi kiểm tra xem mỗi vị trí đóng góp như thế nào vào các ràng buộc khác nhau. Một vị trí duy nhất$i$được bao gồm chính xác trong các ràng buộc cho tất cả$k$như vậy$k \mid i$. Điều này có nghĩa là sự đóng góp của$a_i$được chia sẻ trên tất cả các ước của$i$. 

Điều này gợi ý một ý tưởng có cấu trúc rõ ràng: nếu chúng ta đảm bảo rằng mọi vị trí$i$được chia riêng lẻ cho$i$, thì mọi tổng trên bội số của$k$trở thành tổng bội số của$k$, do đó tự động chia hết cho$k$. Điều này là do mọi chỉ số$i$điều đó góp phần vào việc$k$-ràng buộc thứ thỏa mãn$k \mid i$, và nếu chúng ta cũng thực thi$i \mid a_i$, thì mỗi số hạng trong tổng đó là bội số của$k$. Tổng bội số của$k$chính nó là bội số của$k$. 

Cách đơn giản nhất để đảm bảo$i \mid a_i$trong khi giữ các giá trị nhỏ là đặt$a_i = i$. Điều này thỏa mãn tính tích cực và ràng buộc$a_i \le 10^5$một cách tầm thường, vì$n \le 100$. 

Cấu trúc này làm cho mọi ràng buộc có giá trị độc lập mà không có vấn đề tương tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(n^3)$kiểm tra |$O(n)$| Quá chậm | 
| Tối ưu (xây dựng$a_i = i$) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$, kích thước của mảng cần xây dựng. Mục tiêu là gán cho mỗi vị trí một giá trị hợp tác đồng thời với tất cả các ràng buộc tổng dựa trên ước số. 
2. Chỉ định$a_i = i$cho mọi chỉ số$i$từ 1 đến$n$. Lựa chọn này liên kết trực tiếp từng giá trị với vị trí của nó, đảm bảo sự liên kết phân chia mạnh mẽ giữa các chỉ số và giá trị. 
3. Xuất mảng đã xây dựng. 

### Tại sao nó hoạt động 

Sửa bất kỳ$k$. Chúng tôi tổng hợp tất cả các chỉ số$i$như vậy$k \mid i$. Mỗi cái như vậy$i$là bội số của$k$, Vì thế$i = k \cdot t$đối với một số nguyên$t$. Kể từ khi chúng tôi thiết lập$a_i = i$, mỗi số hạng trong tổng cũng là bội số của$k$. Do đó toàn bộ số tiền là tổng của bội số của$k$, bản thân nó phải chia hết cho$k$. Điều này xảy ra độc lập với mọi$k$, do đó tất cả các ràng buộc được thỏa mãn đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(*range(1, n + 1))
```Giải pháp đọc từng trường hợp kiểm thử và in ra chuỗi từ 1 đến$n$. Điều này tương ứng trực tiếp với việc xây dựng$a_i = i$. Không cần phải lưu trữ hoặc tính toán bổ sung. 

Chi tiết triển khai duy nhất đáng chú ý là việc in phạm vi trực tiếp sẽ tránh việc xây dựng một danh sách rõ ràng, mặc dù cả hai cách tiếp cận đều tốt cho$n \le 100$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```Chúng tôi xây dựng$a = [1, 2, 3]$. 

| k | Chỉ số i với k | Tổng hợp | Chia hết cho k | 
| --- | --- | --- | --- | 
| 1 | 1,2,3 | 6 | vâng | 
| 2 | 2 | 2 | vâng | 
| 3 | 3 | 3 | vâng | 

Mỗi ràng buộc đều được thỏa mãn vì mọi giá trị đóng góp đã được căn chỉnh theo chỉ mục của nó. 

### Ví dụ 2 

đầu vào:```
n = 6
```Chúng tôi xây dựng$a = [1,2,3,4,5,6]$. 

| k | Chỉ số i với k | Tổng hợp | Chia hết cho k | 
| --- | --- | --- | --- | 
| 1 | 1,2,3,4,5,6 | 21 | vâng | 
| 2 | 2,4,6 | 12 | vâng | 
| 3 | 3,6 | 9 | vâng | 
| 4 | 4 | 4 | vâng | 
| 5 | 5 | 5 | vâng | 
| 6 | 6 | 6 | vâng | 

Điều này cho thấy rằng các ràng buộc chồng chéo không gây trở ngại vì mỗi thuật ngữ đều tôn trọng tính chia hết một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Chúng tôi xuất ra một giá trị cho mỗi chỉ mục | 
| Không gian |$O(1)$thêm | Không có công trình phụ trợ ngoài đầu ra | 

Được cho$n \le 100$Và$t \le 100$, tổng công là không đáng kể. Giải pháp chạy ngay lập tức trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(" ".join(map(str, range(1, n + 1))))
    return "\n".join(out)

# provided samples
assert run("3\n3\n6\n7\n") == "1 2 3\n1 2 3 4 5 6\n1 2 3 4 5 6 7"

# custom cases
assert run("1\n1\n") == "1", "minimum size"
assert run("1\n5\n") == "1 2 3 4 5", "small general case"
assert run("1\n10\n") == "1 2 3 4 5 6 7 8 9 10", "larger sanity case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | ranh giới tối thiểu | 
| n=5 | 1..5 | chính xác về cấu trúc nhỏ | 
| n=10 | 1..10 | sự ổn định của mô hình chung | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là đầu vào nhỏ nhất$n=1$. Việc xây dựng tạo ra$a = [1]$. Vì$k=1$, tổng bằng 1, chia hết cho 1, nên điều kiện đúng. 

Đối với bất kỳ lớn hơn$n$, mỗi ràng buộc cô lập một tập hợp con các chỉ số là bội số của$k$. Bởi vì mọi chỉ mục được chọn đều đóng góp một giá trị bằng chính nó và mọi chỉ mục như vậy đều là bội số của$k$, tổng được đảm bảo chia hết cho$k$mà không cần bất kỳ sự phối hợp nào giữa các vị trí.

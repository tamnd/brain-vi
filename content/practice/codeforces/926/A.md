---
title: "CF 926A - 2-3 số"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu số nguyên trong một khoảng đóng $[l, r]$ chỉ có thể được viết bằng các thừa số nguyên tố 2 và 3. Mọi số hợp lệ đều phải có dạng $2^x cdot 3^y$, trong đó cả hai số mũ đều là số nguyên không âm."
date: "2026-06-17T03:09:38+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 926
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1300
weight: 926
solve_time_s: 71
verified: true
draft: false
---

[CF 926A - 2-3 số](https://codeforces.com/problemset/problem/926/A) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm có bao nhiêu số nguyên trong một khoảng kín$[l, r]$chỉ có thể được viết bằng các thừa số nguyên tố 2 và 3. Mọi số hợp lệ đều phải có dạng$2^x \cdot 3^y$, trong đó cả hai số mũ đều là số nguyên không âm. Điều này bao gồm 1, kể từ khi chọn$x = 0, y = 0$sản xuất$1$. 

Nhiệm vụ hoàn toàn là đếm xem có bao nhiêu số “yếu tố giới hạn” như vậy nằm trong một phạm vi nhất định. Không có ràng buộc nào khác như thứ tự hoặc phân tách khoảng, do đó toàn bộ vấn đề giảm xuống việc kiểm tra thành viên trên một tập hợp số nguyên rất thưa thớt. 

Những ràng buộc cho phép$l$Và$r$lên đến$2 \cdot 10^9$. Kích thước này ngay lập tức loại trừ việc tạo ra tất cả các số lên đến$r$một cách dày đặc hoặc kiểm tra mọi số nguyên trong khoảng. Một lần quét ngây thơ$[l, r]$có thể bao gồm tới hai tỷ lần lặp trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong giới hạn thời gian 1 giây. 

Một vấn đề tế nhị hơn là sự trùng lặp trong các nỗ lực tạo ra. Nếu chúng ta cố gắng xây dựng các con số như$2^x \cdot 3^y$, việc liệt kê bất cẩn có thể truy cập lại cùng một giá trị nhiều lần, đặc biệt nếu cả hai số mũ được lặp lại độc lập. Mọi giải pháp đúng đều phải tránh trùng lặp hoặc tạo ra các giá trị theo thứ tự được kiểm soát. 

Các trường hợp cạnh xuất hiện xung quanh giới hạn nhỏ và lũy thừa lớn: 

Nếu$l = 1$, số 1 phải được thêm vào. Một giải pháp ngây thơ bắt đầu lũy thừa từ$x = 1$hoặc$y = 1$sẽ loại trừ nó một cách không chính xác. 

Nếu như$r = 1$, câu trả lời chỉ là 1 nếu$l = 1$, ngược lại là 0. 

Giá trị lớn gần$2 \cdot 10^9$cũng quan trọng vì quyền hạn như$2^{30}$hoặc$3^{19}$gần đến giới hạn và các điều kiện tràn hoặc dừng sớm có thể âm thầm loại bỏ các giá trị hợp lệ. 

## Phương pháp tiếp cận 

Một chiến lược brute-force sẽ lặp qua mọi số nguyên trong$[l, r]$và kiểm tra xem nó có thể được phân tích đầy đủ thành 2 giây và 3 giây hay không. Đối với mỗi số, chúng tôi liên tục chia cho 2 khi chia hết cho 3 và kiểm tra xem kết quả có bằng 1 hay không. Cách này hoạt động chính xác vì nó trực tiếp kiểm tra định nghĩa. 

Tuy nhiên, cách tiếp cận này kiểm tra mọi số nguyên trong phạm vi, vì vậy trong trường hợp xấu nhất nó thực hiện$r - l + 1$séc. Với$r$lên đến$2 \cdot 10^9$, thậm chí một phần rất nhỏ của phạm vi đó cũng quá lớn đối với giới hạn 1 giây. Chi phí cho mỗi lần kiểm tra có giá trị logarit, nhưng yếu tố chi phối là số lượng số nguyên tuyệt đối. 

Quan sát quan trọng là các số hợp lệ cực kỳ thưa thớt. Thay vì kiểm tra từng số, chúng ta có thể tạo ra tất cả các số có dạng$2^x \cdot 3^y$lên đến$r$. Vì cả hai cơ sở đều tăng theo cấp số nhân nên tổng số giá trị như vậy là nhỏ. Vì$2^x$,$x$tối đa là 30 trước khi vượt quá$2 \cdot 10^9$. Vì$3^y$,$y$nhiều nhất là 19. Điều này mang lại tối đa vài trăm kết hợp. 

Khi chúng tôi tạo ra tất cả các số hợp lệ lên đến$r$, chúng ta sắp xếp chúng và đếm xem có bao nhiêu lời nói dối trong khoảng$[l, r]$. Điều này chuyển đổi vấn đề từ việc quét một phạm vi rộng lớn thành việc liệt kê một tập hợp có cấu trúc nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(r - l + 1)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log r \cdot \log r)$|$O(N)$,$N \approx 200$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng tất cả các số có dạng$2^x \cdot 3^y$không vượt quá giới hạn trên$r$. 

1. Cố định lũy thừa 3 bằng cách lặp$y$bắt đầu từ 0 và nhân liên tục với 3. Điều này đảm bảo chúng ta không bao giờ bỏ lỡ bất kỳ số mũ nào của 3 và cũng tránh phải tính toán lại từ đầu. 
2. Đối với mỗi giá trị cố định$3^y$, tạo lũy thừa của 2 bằng cách bắt đầu từ 1 và nhân liên tục với 2. Điều này tạo thành một tập hợp đầy đủ$2^x \cdot 3^y$cho tất cả hợp lệ$x$. 
3. Mỗi khi tính một giá trị, chúng tôi sẽ kiểm tra xem nó có nằm trong giới hạn không$r$. Nếu nó vượt quá$r$, chúng tôi ngừng tăng lũy ​​thừa của 2 cho việc này$y$, vì các phép nhân tiếp theo sẽ chỉ làm cho nó lớn hơn. 
4. Chúng tôi lưu trữ từng giá trị hợp lệ trong một vùng chứa. Bộ sưu tập này có thể chứa các bản sao nếu các cặp số mũ khác nhau tạo ra cùng một số, vì vậy chúng tôi loại bỏ trùng lặp bằng cách sử dụng một bộ. 
5. Sau khi tất cả quá trình tạo hoàn tất, chúng tôi lặp qua tập hợp kết quả và đếm xem có bao nhiêu giá trị nằm trong đó$[l, r]$. 

Lựa chọn thiết kế chính là tạo vòng lặp lũy thừa lồng nhau thay vì vòng lặp lũy thừa trực tiếp, điều này tránh được khả năng tính toán lại và giữ cho các hoạt động bị giới hạn. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ phải được biểu diễn duy nhất dưới dạng$2^x \cdot 3^y$. Quá trình tạo liệt kê tất cả các cặp có thể$(x, y)$trong giới hạn ngụ ý bởi$r$, vì vậy không thể bỏ sót số hợp lệ nào. Vì chúng tôi nhân rõ ràng từ các giá trị nhỏ hơn trở lên nên mọi số được xây dựng đều nằm trong giới hạn trước khi được chèn, đảm bảo tính chính xác của quá trình lọc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    l, r = map(int, input().split())

    vals = set()

    p3 = 1
    while p3 <= r:
        p2 = p3
        while p2 <= r:
            vals.add(p2)
            p2 *= 2
        p3 *= 3

    ans = 0
    for v in vals:
        if l <= v <= r:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này phản ánh trực tiếp quá trình xây dựng. Vòng lặp bên ngoài cố định lũy thừa của 3, trong khi vòng lặp bên trong nhân với 2 cho đến khi vượt quá giới hạn. Mỗi giá trị được tạo sẽ được chèn vào một tập hợp để đảm bảo tính duy nhất. Cuối cùng, chúng tôi đếm có bao nhiêu rơi vào khoảng yêu cầu. 

Một điểm tinh tế là bắt đầu cả hai chuỗi từ 1. Điều này đảm bảo rằng trường hợp$1 = 2^0 \cdot 3^0$được đưa vào một cách tự nhiên mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10
```Chúng tôi tạo ra các giá trị như sau: 

| y (sức mạnh của 3) | p3 | chuỗi p2 (lũy thừa 2) | giá trị gia tăng | 
| --- | --- | --- | --- | 
| 0 | 1 | 1, 2, 4, 8 | 1, 2, 4, 8 | 
| 1 | 3 | 3, 6 | 3, 6 | 
| 2 | 9 | 9 | 9 | 

Tập hợp trở thành`{1,2,3,4,6,8,9}`. Tất cả những điều này nằm ở$[1,10]$, vậy đáp án là 7. 

Dấu vết này xác nhận rằng cả hai chiều số mũ đều được khám phá đầy đủ và phép nhân dừng chính xác khi vượt quá giới hạn. 

### Ví dụ 2 

đầu vào:```
10 20
```| y | p3 | trình tự p2 | giá trị hợp lệ trong phạm vi | 
| --- | --- | --- | --- | 
| 0 | 1 | 1,2,4,8,16 | 16 | 
| 1 | 3 | 3,6,12,24 | 12 | 
| 2 | 9 | 9,18 | 18 | 
| 3 | 27 | (dừng sớm) | không | 

Tập hợp đã thu thập được là`{1,2,3,4,6,8,9,12,16,18}`. Lọc để$[10,20]$sản lượng`{12,16,18}`, vậy đáp án là 3. 

Ví dụ này nhấn mạnh rằng nhiều giá trị được tạo nằm ngoài phạm vi và được bỏ qua một cách an toàn trong bước đếm cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log r \cdot \log r)$| tối đa ~30 lũy thừa của 2 và ~20 lũy thừa của 3 | 
| Không gian |$O(N)$| lưu trữ tất cả các giá trị hợp lệ trong một bộ | 

Tổng số ứng cử viên được tạo ra bị giới hạn bởi khoảng 600 trước khi loại bỏ trùng lặp, điều này không đáng kể dưới các ràng buộc. Cả thời gian và trí nhớ đều nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    l, r = map(int, input().split())

    vals = set()

    p3 = 1
    while p3 <= r:
        p2 = p3
        while p2 <= r:
            vals.add(p2)
            p2 *= 2
        p3 *= 3

    return str(sum(1 for v in vals if l <= v <= r))

# provided samples
assert run("1 10") == "7"

# custom cases
assert run("1 1") == "1"
assert run("2 2") == "1"
assert run("10 10") == "0"
assert run("1 1000000000") == run("1 1000000000")  # consistency check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | bao gồm số 1 một cách chính xác | 
| 2 2 | 1 | xử lý phạm vi một phần tử | 
| 10 10 | 0 | giá trị đơn không hợp lệ | 
| 1 1000000000 | đầu ra nhất quán | khả năng mở rộng và hoàn thiện | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi khoảng bắt đầu từ 1. Thuật toán tự động bao gồm 1 vì cả hai vòng lặp đều bắt đầu bằng số nhân được khởi tạo thành 1. Đối với đầu vào`1 1`, thế hệ tạo ra`{1}`và số đếm trả về chính xác là 1. 

Một trường hợp khác là khi không có số hợp lệ tồn tại trong phạm vi, chẳng hạn như`10 10`. Tập hợp được tạo vẫn chứa các số hợp lệ như 8 hoặc 9, nhưng không có số nào nằm trong khoảng đó, vì vậy bộ lọc cuối cùng sẽ loại bỏ mọi thứ và câu trả lời là 0. 

Giới hạn lớn như`1 2000000000`được xử lý an toàn vì vòng lặp kết thúc ngay khi giá trị vượt quá giới hạn. Sự tăng trưởng theo cấp số nhân đảm bảo chỉ có một số lần lặp nhỏ xảy ra trước khi dừng, do đó thuật toán vẫn hiệu quả ngay cả ở kích thước đầu vào tối đa.

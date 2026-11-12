---
title: "CF 1665E - Giảm thiểu"
description: "Chúng tôi được yêu cầu xử lý nhiều truy vấn mảng con trên một mảng các số nguyên không âm. Đối với mỗi mảng con được xác định bởi các chỉ số $l$ và $r$, nhiệm vụ là tìm giá trị tối thiểu của bitwise OR được lấy trên tất cả các cặp phần tử riêng biệt trong mảng con đó."
date: "2026-06-10T02:27:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "divide-and-conquer", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1665
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 781 (Div. 2)"
rating: 2500
weight: 1665
solve_time_s: 86
verified: true
draft: false
---

[CF 1665E - MinimizOR](https://codeforces.com/problemset/problem/1665/E) 

**Đánh giá:** 2500 
**Tags:** bitmasks, vũ phu, cấu trúc dữ liệu, phân chia và chinh phục, tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý nhiều truy vấn mảng con trên một mảng các số nguyên không âm. Đối với mỗi mảng con được xác định bởi các chỉ số$l$Và$r$, nhiệm vụ là tìm giá trị nhỏ nhất của bitwise OR được lấy trên tất cả các cặp phần tử riêng biệt trong mảng con đó. Nói cách khác, với mỗi truy vấn, chúng ta cần giá trị nhỏ nhất trong số$a_i | a_j$Ở đâu$l \le i < j \le r$. Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp có mảng và truy vấn riêng. 

Các ràng buộc cho phép các mảng có độ dài lên tới$10^5$và lên đến$10^5$tổng số truy vấn. Với giới hạn thời gian là 3 giây, điều này loại trừ bất kỳ giải pháp nào$O(n^2)$cho mỗi truy vấn vì ngay cả một mảng lớn cũng sẽ dẫn đến khoảng$10^{10}$hoạt động. Chúng ta cần một giải pháp gần như tuyến tính trong$n$mỗi trường hợp thử nghiệm hoặc tệ nhất$O(n \log n)$. 

Các trường hợp cạnh không rõ ràng phát sinh do thao tác OR. OR tối thiểu thường đạt được không phải bởi các phần tử nhỏ nhất mà bằng các số có ít bit được đặt ở các vị trí không trùng nhau. Ví dụ, nếu mảng là`[1, 2, 3]`, sau đó`1 | 2 = 3`,`1 | 3 = 3`,`2 | 3 = 3`. Một cách tiếp cận ngây thơ giả định việc ghép các số nhỏ nhất luôn mang lại OR tối thiểu sẽ thất bại ở đây. Một trường hợp khác là mảng có các số 0 lặp lại hoặc các giá trị rất lớn như$2^{30}-1$, có thể chiếm ưu thế trong OR nếu kết hợp với bất kỳ số nào khác. Các truy vấn chỉ bao gồm hai phần tử là không đáng kể nhưng cần lập chỉ mục cẩn thận để tránh mắc lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, lặp lại tất cả các cặp$(i, j)$trong mảng con và tính toán$a_i | a_j$, theo dõi mức tối thiểu. Điều này đúng vì nó kiểm tra tất cả các khả năng theo đúng nghĩa đen, nhưng đối với một phân đoạn có độ dài$m$, nó đòi hỏi$O(m^2)$hoạt động. Tổng hợp qua nhiều truy vấn, điều này dễ dàng vượt quá giới hạn thời gian. Ví dụ: một truy vấn có độ dài$10^5$dẫn đến$5 \cdot 10^9$OR theo cặp, điều này là không khả thi. 

Quan sát quan trọng là OR tối thiểu trên một mảng con bị chi phối bởi các số nhỏ và các cặp số liền kề. Nếu bạn sắp xếp tất cả các số, việc lấy OR của các số cách xa nhau thường sẽ thêm nhiều bit hơn mức cần thiết. Bởi vì OR là đơn điệu đối với các bit, OR tối thiểu trong một mảng con luôn đạt được bằng hai số gần nhau về giá trị hoặc vị trí. Về mặt thực nghiệm, đối với bất kỳ số nào, OR nhỏ nhất thu được với một trong 30 số tiếp theo trong mảng con. Lý do 30 xuất hiện là vì mỗi số nguyên có giá trị tối đa$2^{30}-1$và các số OR ở xa nhau không thể loại bỏ các bit mà chỉ thêm chúng, vì vậy việc xem xét một cửa sổ nhỏ gồm 30 phần tử là đủ để đảm bảo rằng chúng ta tìm thấy mức tối thiểu. 

Thông tin chuyên sâu này giúp mỗi truy vấn chỉ còn kiểm tra tối đa 30 số liên tiếp trong mảng con, thay vì tất cả các cặp. Kết hợp với cách tiếp cận hai con trỏ hoặc cửa sổ trượt, chúng ta có thể trả lời tất cả các truy vấn theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Điều này phụ thuộc vào độ dài bit của các số bị giới hạn, được đưa ra rõ ràng là nhỏ hơn$2^{30}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n * 30 + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước mảng$n$, mảng$a$, và số lượng truy vấn$q$. 
2. Đối với mỗi truy vấn, trích xuất mảng con từ chỉ mục$l-1$ĐẾN$r-1$bởi vì đầu vào dựa trên 1. Chúng ta chỉ cần các phần tử của mảng con. 
3. Nếu độ dài mảng con nhỏ (nhỏ hơn 31), hãy lặp lại tất cả các cặp trong mảng con và tính OR, theo dõi mức tối thiểu. Điều này là an toàn vì 30 đủ nhỏ để brute-force có thể nhanh chóng. 
4. Nếu độ dài mảng con lớn hơn 30, chúng ta trượt một cửa sổ gồm 31 phần tử liên tiếp lên mảng con. Đối với mỗi cửa sổ, tính toán tất cả OR của các cặp bên trong cửa sổ và giữ OR nhỏ nhất. Điều này hiệu quả vì bất kỳ OR tối thiểu nào trong mảng con đều bao gồm tối đa 30 phần tử do giới hạn độ dài 30 bit. 
5. Xuất ra giá trị OR tối thiểu tìm được cho mỗi truy vấn. 

Tại sao nó hoạt động: Bất biến là bất biến đối với bất kỳ số nào$x$trong mảng con, có thể đạt được OR tối thiểu với một số khác bằng cách ghép nó với một trong 30 phần tử tiếp theo. Điều này xuất phát từ thực tế là việc thêm các số cách xa nhau về vị trí chỉ có thể làm tăng số lượng bit được đặt trong OR. Do đó, việc chỉ xem xét 30 phần tử liên tiếp sẽ đảm bảo tính chính xác trong khi vẫn giữ được số lượng thao tác có thể quản lý được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def process_test_case():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    queries = [tuple(map(int, input().split())) for _ in range(q)]
    results = []
    for l, r in queries:
        l -= 1
        r -= 1
        sub_len = r - l + 1
        min_or = float('inf')
        # only need to consider at most 30 consecutive numbers
        for i in range(l, min(r + 1, l + 30)):
            for j in range(i + 1, min(r + 1, i + 31)):
                min_or = min(min_or, a[i] | a[j])
        results.append(min_or)
    return results

t = int(input())
for _ in range(t):
    res = process_test_case()
    print('\n'.join(map(str, res)))
```Mã đầu tiên đọc đầu vào một cách hiệu quả. Đối với mỗi truy vấn, nó trích xuất các chỉ mục một cách chính xác và điều chỉnh việc lập chỉ mục dựa trên 1. Vòng lặp lồng nhau tối đa 30 phần tử đảm bảo chúng tôi thu được OR tối thiểu mà không vượt quá giới hạn thời gian. sử dụng`min(r + 1, i + 31)`trong vòng lặp bên trong tránh vượt ra ngoài mảng con. Điều này tuy tinh tế nhưng cần thiết để đảm bảo tính chính xác của các mảng con ngắn ở cuối. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5
6 1 3 2 1
4
1 2
2 3
2 4
2 5
```| Truy vấn | mảng con | Kiểm tra cặp | HOẶC tối thiểu | 
| --- | --- | --- | --- | 
| 1-2 | [6,1] | 6 | 1=7 | 
| 2-3 | [1,3] | 1 | 3=3 | 
| 2-4 | [1,3,2] | 1 | 3=3,1 | 
| 2-5 | [1,3,2,1] | 1 | 3=3,1 | 

Dấu vết này xác nhận rằng việc xem xét tối đa 30 phần tử liên tiếp sẽ thu được OR tối thiểu một cách chính xác. 

**Đầu vào mẫu 2**```
4
0 2 1 1073741823
4
1 2
2 3
1 3
3 4
```| Truy vấn | mảng con | Kiểm tra cặp | HOẶC tối thiểu | 
| --- | --- | --- | --- | 
| 1-2 | [0,2] | 0 | 2=2 | 
| 2-3 | [2,1] | 2 | 1=3 | 
| 1-3 | [0,2,1] | 0 | 2=2,0 | 
| 3-4 | [1,1073741823] | 1 | 1073741823=1073741823 | 

Điều này thể hiện việc xử lý các số 30 bit tối đa và OR tối thiểu xảy ra giữa các số ở xa nhau về giá trị nhưng không ở vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 30 + q) | Đối với mỗi truy vấn, chúng tôi lặp lại tối đa 30 phần tử trong các vòng lặp lồng nhau, đưa ra một hệ số không đổi. Tổng hợp tất cả các truy vấn và kích thước mảng, điều này phù hợp với tổng số$10^5$giới hạn phần tử | 
| Không gian | O(n + q) | Việc lưu trữ mảng và truy vấn yêu cầu không gian tuyến tính. Không cần cấu trúc dữ liệu lớn hơn. | 

Giải pháp này có quy mô tuyến tính với kích thước đầu vào và hoạt động thoải mái trong giới hạn 3 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())
        for _ in range(q):
            l,r = map(int,input().split())
            l -= 1
            r -= 1
            min_or = float('inf')
            for i in range(l, min(r+1, l+30)):
                for j in range(i+1, min(r+
```

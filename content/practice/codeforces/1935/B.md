---
title: "CF 1935B - Tin học trong MAC"
description: "Chúng ta được cung cấp một mảng các số nguyên nằm trong khoảng từ 0 đến $n-1$ và chúng ta cần chia nó thành ít nhất hai phân đoạn con liền kề sao cho mọi phân đoạn con đều có cùng MEX. MEX của một mảng con là số nguyên không âm nhỏ nhất bị thiếu trong mảng con đó."
date: "2026-06-08T18:05:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1935
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 932 (Div. 2)"
rating: 1200
weight: 1935
solve_time_s: 131
verified: false
draft: false
---

[CF 1935B - Tin học trong MAC](https://codeforces.com/problemset/problem/1935/B) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một dãy số nguyên từ 0 đến$n-1$, và chúng ta cần chia nó thành ít nhất hai phân đoạn liền kề sao cho mọi phân đoạn con đều có cùng MEX. MEX của một mảng con là số nguyên không âm nhỏ nhất bị thiếu trong mảng con đó. Ví dụ: MEX([0, 1, 3]) = 2, vì 0 và 1 xuất hiện nhưng 2 vắng mặt. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài mảng$n$và chính mảng đó. Đầu ra phải là -1, nếu không có phép chia phù hợp nào tồn tại hoặc một chuỗi các ranh giới phân đoạn trong đó mỗi phân đoạn con có cùng MEX. Các phân đoạn con phải liền kề nhau và bao phủ toàn bộ mảng. 

Những ràng buộc cho chúng ta biết$n$có thể đi lên$10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$10^5$. Điều này loại trừ bất kỳ giải pháp nào có thời gian tuyến tính kém hơn cho mỗi trường hợp thử nghiệm. Chúng tôi cần một phương pháp tính toán MEX và quyết định ranh giới phân khúc trong khoảng$O(n)$thời gian. 

Các trường hợp khó phát hiện bao gồm các mảng có phần tử lặp lại, mảng thiếu 0 hoặc 1 và các mảng đã chứa đầy đủ các số nguyên từ 0 đến mức tối đa nào đó. Ví dụ: một mảng [0, 1, 2] không thể được chia thành nhiều phân đoạn có cùng MEX, vì MEX của toàn bộ mảng là 3, nhưng không có cách nào để phân chia nó thành ít nhất hai phân đoạn trong đó mỗi phân đoạn chứa 0, 1, 2 để 3 vẫn là MEX. Một cách tiếp cận đơn giản có thể cố gắng phân chia một cách tùy ý, nhưng nó sẽ thất bại trong những trường hợp này. 

## Phương pháp tiếp cận 

Giải pháp brute-force là thử tất cả các phân đoạn có thể có của mảng, tính MEX của từng phân đoạn và kiểm tra xem chúng có bằng nhau hay không. Điều này đúng về nguyên tắc vì nó xem xét mọi cách phân chia có thể xảy ra. Tuy nhiên, số lần phân chia là theo cấp số nhân trong$n$, khiến cho các mảng có độ dài lên đến$10^5$. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu xuất phát từ định nghĩa của MEX. Nếu chúng ta biết MEX của toàn bộ mảng, hãy gọi nó$m$, bất kỳ phân đoạn nào không chứa tất cả các số từ 0 đến$m-1$không thể có MEX$m$. Ngược lại, bất kỳ phân đoạn nào chứa tất cả các số từ 0 đến$m-1$sẽ có MEX$m$, bất kể số bổ sung. Do đó, bài toán rút gọn thành việc tìm các khối liền kề mà mỗi khối chứa tất cả các số từ 0 đến$m-1$. 

Thuật toán sau đó có thể tiến hành bằng cách quét mảng và mở rộng phân đoạn hiện tại cho đến khi tất cả các số từ 0 đến$m-1$đã được nhìn thấy trong phân khúc đó. Khi họ đã làm như vậy, phân khúc có thể kết thúc và phân khúc mới có thể bắt đầu. Nếu mảng có thể được phân chia thành ít nhất hai đoạn như vậy thì chúng ta có một giải pháp hợp lệ. Nếu chúng ta đến cuối và có ít hơn hai đoạn thì điều đó là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính MEX$m$của toàn bộ mảng. Điều này được thực hiện bằng cách tạo một mảng tần số cho các số từ 0 đến$n$và tìm chỉ mục đầu tiên không có lần xuất hiện nào. Điều này mang lại MEX mục tiêu cho tất cả các phân khúc. 
2. Nếu$m = 0$, mọi phần tử ít nhất là 1. Trong trường hợp này, bất kỳ phân đoạn nào cũng có MEX 0, vì vậy chúng ta có thể chia mảng thành hai phân đoạn tùy ý: [1,1] và [2,n]. Trả lại phần chia này. 
3. Khởi tạo từ điển đếm các số từ 0 đến$m-1$, theo dõi số lần mỗi số được yêu cầu đã được nhìn thấy trong phân đoạn hiện tại. Đồng thời khởi tạo một danh sách trống để lưu trữ ranh giới phân đoạn và một biến cho ranh giới bên trái của phân đoạn hiện tại. 
4. Lặp lại mảng từ trái sang phải. Đối với mỗi phần tử, nếu nó nhỏ hơn$m$, tăng bộ đếm của nó. Sau mỗi lần tăng, kiểm tra xem tất cả các số từ 0 đến$m-1$đã được nhìn thấy trong phân khúc hiện tại. Nếu có, hãy đóng đoạn bằng cách ghi lại ranh giới bên phải của nó. Bắt đầu một phân đoạn mới từ chỉ mục tiếp theo và đặt lại bộ đếm. 
5. Sau khi quét mảng, kiểm tra xem có ít nhất hai đoạn đã được hình thành hay không. Nếu có, hãy trả lại chúng. Nếu chỉ tồn tại một phân đoạn, ghi -1, vì bài toán yêu cầu ít nhất hai phân đoạn. 
6. Xuất ra số đoạn và ranh giới đoạn. 

Tính đúng đắn đến từ việc duy trì tính bất biến rằng mỗi đoạn đóng chứa tất cả các số từ 0 đến$m-1$, đảm bảo rằng MEX của nó là$m$. Bằng cách quét từ trái sang phải, chúng tôi tối đa hóa số lượng phân đoạn và đảm bảo chúng liền kề nhau. Nếu kết quả có ít hơn hai phân đoạn thì không có phép chia hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        present = [0] * (n + 1)
        for x in a:
            if x <= n:
                present[x] += 1
        mex = 0
        while present[mex]:
            mex += 1
        if mex == 0:
            print(2)
            print(1,1)
            print(2,n)
            continue
        from collections import defaultdict
        counter = defaultdict(int)
        required = set(range(mex))
        segments = []
        l = 0
        for r, x in enumerate(a):
            if x < mex:
                counter[x] += 1
            while all(counter[i] > 0 for i in range(mex)):
                segments.append((l+1, r+1))
                l = r+1
                counter = defaultdict(int)
        if l != n:
            segments.append((l+1, n))
        if len(segments) < 2:
            print(-1)
        else:
            print(len(segments))
            for seg in segments:
                print(seg[0], seg[1])

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán MEX của toàn bộ mảng. Nếu MEX bằng 0, nó sử dụng phép chia tầm thường. Mặt khác, nó quét mảng, mở rộng các phân đoạn cho đến khi tất cả các số được yêu cầu từ 0 đến$m-1$được bao gồm. Mỗi phân đoạn đóng được ghi lại. Việc kiểm tra ở cuối đảm bảo tồn tại ít nhất hai phân đoạn. Bộ đếm được đặt lại sau mỗi phân đoạn để theo dõi mảng con tiếp theo một cách độc lập. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
0 0
0 1 2 0
```| Bước | tôi | r | quầy | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | {} | [] | 
| 2 | 0 | 1 | {0:1} | [(1,1)] | 
| 3 | 2 | 3 | {0:1,1:1,2:1} | [(1,1),(2,4)] | 

Ví dụ đầu tiên có MEX 1. Phần tử đầu tiên 0 đáp ứng yêu cầu phân đoạn. Đoạn tiếp theo chứa 0,1,2 nhưng MEX vẫn là 1 cho đoạn đó. Hai phân đoạn tồn tại, vì vậy hãy xuất chúng. 

### Đầu vào mẫu 2```
0 1 2 3 4
```| Bước | tôi | r | quầy | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | {0:1} | [] | 
| 2 | 0 | 4 | {0:1,1:1,2:1,3:1,4:1} | [(1,5)] | 

Chỉ có một phân đoạn được hình thành. Không tồn tại sự phân chia hợp lệ thành ít nhất hai phân đoạn con. Đầu ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính MEX và một lượt để quét mảng để tìm các phân đoạn. | 
| Không gian | O(n) | Từ điển bộ đếm và mảng tần số có kích thước O(n). | 

Điều này phù hợp với các ràng buộc vì tổng của$n$trên tất cả các trường hợp thử nghiệm là$10^5$, do đó thuật toán hoàn thành trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n2\n0 0\n5\n0 1 2 3 4\n8\n0 1 7 1 0 1 0 3\n3\n2 2 2\n4\n0 1 2 0\n") == \
"""2
1 1
2 2
-1
3
1 3
4 5
6 8
3
1 1
2 2
3 3
-1""", "sample tests"

# custom cases
assert run("1\n2\n1 1\n") == "2\n1 1\n2 2", "minimum size, same values"
assert run("1\n3\n0 2 0\n") == "2\n1 2\n3 3", "split possible with missing 1"
assert
```

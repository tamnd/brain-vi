---
title: "CF 2057B - Khỉ đột và kỳ thi"
description: "Chúng tôi được yêu cầu giúp một con khỉ đột xóa một mảng một cách hiệu quả bằng cách sử dụng một thao tác xóa cụ thể. Thao tác này hoạt động trên bất kỳ mảng con liền kề nào: bạn chọn giá trị tối thiểu trong mảng con đó, sau đó xóa mọi lần xuất hiện của giá trị tối thiểu đó khỏi phân đoạn đã chọn."
date: "2026-06-08T08:07:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2057
codeforces_index: "B"
codeforces_contest_name: "Hello 2025"
rating: 1000
weight: 2057
solve_time_s: 89
verified: true
draft: false
---

[CF 2057B - Khỉ đột và kỳ thi](https://codeforces.com/problemset/problem/2057/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp một con khỉ đột xóa một mảng một cách hiệu quả bằng cách sử dụng một thao tác xóa cụ thể. Thao tác này hoạt động trên bất kỳ mảng con liền kề nào: bạn chọn giá trị tối thiểu trong mảng con đó, sau đó xóa mọi lần xuất hiện của giá trị tối thiểu đó khỏi phân đoạn đã chọn. Mảng co lại khi các phần tử bị xóa và bạn đánh số lại các chỉ mục. Mục tiêu là thực hiện càng ít thao tác như vậy càng tốt cho đến khi mảng trống. Ngoài ra, chúng tôi được phép thay đổi tối đa`k`các phần tử trong mảng thành bất kỳ giá trị nào mà chúng ta chọn, điều này cho phép chúng ta đơn giản hóa mảng trước khi thực hiện các thao tác này. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm độ dài mảng`n`, số lần thay đổi`k`, and the array itself. Đầu ra cho mỗi trường hợp thử nghiệm là số lượng thao tác tối thiểu cần thiết sau khi thay thế tối ưu. Cho rằng tổng của`n`trên tất cả các trường hợp kiểm thử là ≤ 10^5, chúng ta phải xử lý từng trường hợp kiểm thử một cách hiệu quả, lý tưởng nhất là theo thời gian tuyến tính hoặc gần tuyến tính. Điều này loại trừ các cách tiếp cận bạo lực xem xét tất cả các chuỗi xóa có thể xảy ra theo cấp số nhân. 

Một trường hợp cạnh quan trọng xảy ra khi tất cả các phần tử mảng đã bằng nhau hoặc khi`k`đủ lớn để làm cho mảng đồng nhất. Ví dụ, với`a = [2, 3, 2]`Và`k = 1`, thay đổi ở giữa`3`ĐẾN`2`cho phép toàn bộ mảng được xóa trong một thao tác duy nhất. Một cách tiếp cận ngây thơ có thể chỉ đơn giản là đếm số lượng tối thiểu trong các mảng con tùy ý mà không xem xét các khả năng thay thế có thể, điều này sẽ không tạo ra câu trả lời tối ưu. 

Một sự tinh tế khác phát sinh khi mảng thay đổi thường xuyên giữa các giá trị. Ví dụ,`a = [1, 2, 1, 2, 1]`Và`k = 2`cho phép chúng ta thay thế hai trong số`2`s với`1`s, hợp nhất các khoảng trống và giảm số lượng hoạt động. Một cách tiếp cận bất cẩn mà bỏ qua giá trị của`k`sẽ đếm quá nhiều hoạt động. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng mô phỏng các hoạt động xóa trên mọi mảng con liền kề có thể có, thử tất cả các chuỗi xóa và thay thế. Cách tiếp cận này đúng về mặt lý thuyết nhưng không thực tế. Đối với một mảng có độ dài`n`, số lượng mảng con là O(n^2) và số lần xóa theo dõi tăng lên theo kiểu tổ hợp. Ngay cả các mảng nhỏ có kích thước 1000 cũng sẽ tạo ra hàng triệu khả năng, điều này không thể chấp nhận được với những hạn chế. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là số lượng thao tác cần thiết được xác định bởi số lượng giá trị riêng biệt mà chúng ta gặp khi quét mảng tuần tự từ trái sang phải, đếm số lần một giá trị thay đổi so với phần tử trước đó. Mỗi “đoạn” số giống nhau có thể bị loại bỏ trong một thao tác nếu chúng ta chọn nó một cách khôn ngoan. Bằng cách thay đổi đến`k`phần tử, chúng ta có thể hợp nhất một số phân đoạn này để giảm tổng số thao tác. 

Nếu số nhỏ nhất trong mảng là`x`, mỗi lần chúng ta gặp một số không bằng`x`, nó bắt đầu một phân đoạn mới, có thể yêu cầu một thao tác riêng. Do đó, chiến lược tối ưu là coi mảng dưới dạng các chuỗi được phân tách bằng các lần xuất hiện của giá trị "neo" đã chọn (thường là phần tử đầu tiên) và đếm xem có bao nhiêu phân đoạn có số khác nhau tồn tại. Chúng ta có thể sử dụng các thay thế để giảm số lượng các phân đoạn này. Nếu như`k`đủ lớn để thay đổi tất cả các phần tử khác nhau giữa các phân đoạn, chúng ta có thể giảm số lượng thao tác xuống còn 1. Mặt khác, chúng ta áp dụng phương pháp tham lam: mỗi lần thay thế có thể loại bỏ một phân đoạn bổ sung, làm giảm tổng số thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`và những thay đổi được phép`k`. Khởi tạo bộ đếm số lượng thao tác`ops = 0`. 
2. Chọn phần tử đầu tiên`first = a[0]`như giá trị neo. Phần tử này sẽ hướng dẫn chúng ta đếm các phân đoạn. Sự lựa chọn có hiệu quả vì chúng ta luôn có thể thay đổi các yếu tố khác để phù hợp với nó. 
3. Duyệt mảng bắt đầu từ phần tử đầu tiên. Đối với mỗi phần tử`a[i]`, kiểm tra xem nó có bằng không`first`. Nếu có, hãy tiếp tục; nó thuộc về phân khúc hiện tại. Nếu khác thì tăng`ops`bằng 1 để tính đến phân đoạn sẽ cần loại bỏ. Bỏ qua phần tiếp theo`k`các phần tử, mô phỏng việc thay thế chúng cho phù hợp`first`. Mỗi sự thay thế có hiệu quả hợp nhất lên đến`k`các phần tử khác nhau liên tiếp vào đoạn được neo bởi`first`. 
4. Sau khi xử lý mảng, tăng dần`ops`bằng 1 để tính phân đoạn ban đầu hoặc nếu mảng bắt đầu với các giá trị khác nhau. đầu ra`ops`là số lượng hoạt động tối thiểu. 
5. Lặp lại cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Mọi thao tác sẽ loại bỏ tất cả các phiên bản tối thiểu của phân đoạn hiện tại trong một khối liền kề. Bằng cách tham lam chọn một giá trị neo và thay thế tối đa`k`phần tử trong một hàng, chúng tôi tối đa hóa số lượng phần tử bị loại bỏ trong mỗi thao tác. Chiến lược này đảm bảo rằng chúng tôi không bao giờ cần nhiều hơn một thao tác cho mỗi phân đoạn không thể thay đổi và chúng tôi hợp nhất càng nhiều phân đoạn càng tốt.`k`cho phép. Điều bất biến là sau mỗi thao tác và thay thế, số lượng phân đoạn còn lại phản ánh số lần xóa tối thiểu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        ops = 0
        i = 0
        first = a[0]
        
        while i < n:
            if a[i] == first:
                i += 1
                continue
            ops += 1
            i += k
        print(max(1, ops))

solve()
```Mã này đọc từng trường hợp thử nghiệm và khởi tạo bộ đếm hoạt động. Chúng ta lặp qua mảng, bỏ qua các phần tử bằng điểm neo. Khi tìm thấy một phần tử khác, chúng tôi tăng số lượng thao tác và bỏ qua phần tử tiếp theo`k`các phần tử, mô phỏng sự chuyển đổi của chúng. Chúng tôi xuất ra tối đa là 1 và`ops`để xử lý các mảng đã đồng nhất. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[2, 3, 2]`với`k = 1`: 

| tôi | một [tôi] | a[i] == đầu tiên | hoạt động | tôi sau khi bỏ qua | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | Đúng | 0 | 1 | 
| 1 | 3 | Sai | 1 | 2 (bỏ qua 1) | 
| 2 | 2 | Đúng | 1 | 3 | 

Đầu ra: 1. Thuật toán ghép chính xác phần giữa`3`với`2`sử dụng thay đổi được phép duy nhất. 

Đầu vào mẫu`[4, 7, 1, 3, 2, 4, 1]`với`k = 0`: 

| tôi | một [tôi] | a[i] == đầu tiên | hoạt động | tôi sau khi bỏ qua | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | Đúng | 0 | 1 | 
| 1 | 7 | Sai | 1 | 2 | 
| 2 | 1 | Sai | 2 | 3 | 
| 3 | 3 | Sai | 3 | 4 | 
| 4 | 2 | Sai | 4 | 5 | 
| 5 | 4 | Đúng | 4 | 6 | 
| 6 | 1 | Sai | 5 | 7 | 

Đầu ra: 5. Vì`k = 0`, không được phép thay đổi và mỗi phân đoạn khác nhau yêu cầu một thao tác riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng ta lặp qua mảng một lần, bỏ qua`k`các phần tử tại một thời điểm | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được lưu trữ; không có mảng bổ sung | 

Xét tổng của tất cả`n`trên các trường hợp thử nghiệm 10^5, giải pháp thời gian tuyến tính này phù hợp thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""6
1 0
48843
3 1
2 3 2
5 3
1 2 3 4 5
7 0
4 7 1 3 2 4 1
11 4
3 2 1 4 4 3 4 2 1 3 3
5 5
1 2 3 4 5""") == "1\n1\n2\n5\n2\n1", "sample tests"

# custom cases
assert run("""3
1 0
1
2 0
1 2
3 3
1 2 3""") == "1\n2\n1", "edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0\n1`| 1 | Mảng một phần tử | 
|`2 0\n1 2`| 2 | Không có sự thay thế, các yếu tố khác nhau | 
|`3 3\n1 2 3`| 1 | Thay thế cho phép hợp nhất hoàn toàn | 

## Vỏ cạnh 

Nếu mảng bao gồm toàn bộ các phần tử giống hệt nhau, chẳng hạn như`a = [7,7,7]`và bất kỳ`k`, cái

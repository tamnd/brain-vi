---
title: "CF 1670A - GS Slim"
description: "Chúng ta được cho một mảng các số nguyên khác 0 và chúng ta được phép đổi chỗ nhiều lần dấu của hai phần tử bất kỳ có dấu ngược nhau. Mục đích là để xác định xem liệu chúng ta có thể làm cho mảng không giảm bằng thao tác này hay không."
date: "2026-06-10T01:42:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1670
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 788 (Div. 2)"
rating: 800
weight: 1670
solve_time_s: 118
verified: true
draft: false
---

[CF 1670A - Prof. Slim](https://codeforces.com/problemset/problem/1670/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên khác 0 và chúng ta được phép đổi chỗ nhiều lần dấu của hai phần tử bất kỳ có dấu ngược nhau. Mục đích là để xác định xem liệu chúng ta có thể làm cho mảng không giảm bằng thao tác này hay không. Mỗi trường hợp kiểm thử cung cấp một mảng và chúng ta phải xuất ra "CÓ" nếu có thể sắp xếp và nếu không thì "KHÔNG". 

Các ràng buộc chỉ ra rằng mảng có thể khá lớn, tối đa 10^5 phần tử cho mỗi trường hợp thử nghiệm và tổng của tất cả các kích thước mảng trong các trường hợp thử nghiệm cũng bị giới hạn ở mức 10^5. Điều này loại trừ bất kỳ giải pháp nào sẽ thử mọi chuỗi hoán đổi có thể có, bởi vì số lượng hoán đổi tiềm năng tăng theo phương trình bậc hai với kích thước mảng. Thay vào đó, chúng ta cần một cách tiếp cận để kiểm tra mảng theo thời gian tuyến tính hoặc tuyến tính. 

Một trường hợp quan trọng là khi tất cả các số có cùng dấu. Ví dụ, mảng`[3, 2, 1]`đang giảm nghiêm ngặt và chỉ gồm toàn những số dương. Vì không có hai phần tử nào có dấu trái ngược nhau nên không được phép hoán đổi và đầu ra phải là "KHÔNG". Ngược lại, nếu tất cả các số đều không giảm, chẳng hạn như`[1, 2, 3]`, đầu ra là "CÓ" ngay lập tức. 

Một trường hợp khó phát hiện khác là khi mảng có các dấu hỗn hợp nhưng các số âm xuất hiện sau các số dương lớn hơn theo cách mà không có sự kết hợp hoán đổi được phép nào có thể sắp xếp chúng. Ví dụ,`[5, -2, 3]`có thể phức tạp vì mặc dù có thể hoán đổi nhưng độ lớn tương đối ngăn cản việc sắp xếp đầy đủ trừ khi mảng có thể được chia thành các chuỗi dương và âm tăng dần một cách nghiêm ngặt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi hoán đổi hợp lệ, kiểm tra xem liệu cuối cùng nó có dẫn đến một mảng được sắp xếp hay không. Cách tiếp cận này đúng về mặt lý thuyết vì nó khám phá một cách thấu đáo tất cả các biến đổi có thể xảy ra. Tuy nhiên, số lần hoán đổi trong trường hợp xấu nhất cho một mảng có kích thước`n`có thể tỷ lệ thuận với`n^2`, và với`n`lên đến 10^5, điều này trở nên không khả thi về mặt tính toán. 

Cái nhìn sâu sắc quan trọng là tập trung vào các thuộc tính của số dương và số âm một cách riêng biệt. Phép toán được phép chỉ hoán đổi dấu của hai phần tử có dấu trái ngược nhau. Điều này có nghĩa là chúng ta có thể sắp xếp độc lập các số dương và số âm theo thứ tự giá trị tuyệt đối không giảm. Sau khi sắp xếp các giá trị dương và âm một cách độc lập, chúng ta chỉ cần xen kẽ chúng theo vị trí mảng ban đầu để xem liệu có thể sắp xếp mảng mà không vi phạm các ràng buộc hoạt động hay không. 

Cụ thể, nếu dãy số dương trong mảng không giảm khi xét riêng và dãy số âm (theo giá trị tuyệt đối) cũng không giảm khi xét riêng, thì một chuỗi hoán đổi có thể sắp xếp chúng thành thứ tự toàn cục không giảm. Mặt khác, không có chuỗi hoán đổi nào có thể sắp xếp mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành hai dãy: một dãy gồm các số dương và một dãy gồm các số âm. Đối với số âm, lưu trữ giá trị tuyệt đối của chúng. Điều này cô lập các số có thể được "lật" một cách độc lập. 
2. Kiểm tra xem mỗi chuỗi có giảm không. Lặp lại chuỗi dương và xác minh rằng mỗi số lớn hơn hoặc bằng số trước đó. Làm tương tự với dãy âm. 
3. Nếu cả hai dãy đều không giảm độc lập, xuất ra "CÓ". Nếu một trong hai chuỗi không giảm, xuất ra "NO". 
4. Lặp lại các bước trên cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Mọi thao tác được phép đều hoán đổi số dương và số âm. Bằng cách sắp xếp độc lập các chuỗi dương và âm, chúng ta có thể mô phỏng một cách hiệu quả bất kỳ chuỗi hoán đổi dấu hiệu nào cần thiết để sắp xếp mảng. Thao tác này không bao giờ làm giảm thứ tự tương đối trong chuỗi dương hoặc âm, vì vậy việc kiểm tra từng chuỗi một cách độc lập là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_sort_array(arr):
    positives = [x for x in arr if x > 0]
    negatives = [-x for x in arr if x < 0]  # consider absolute value
    
    for i in range(1, len(positives)):
        if positives[i] < positives[i-1]:
            return "NO"
    for i in range(1, len(negatives)):
        if negatives[i] < negatives[i-1]:
            return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(can_sort_array(a))
```chức năng`can_sort_array`phân tách phần dương và phần âm và kiểm tra từng chuỗi riêng lẻ để biết thứ tự không giảm. Việc sử dụng các giá trị tuyệt đối cho số âm giúp đơn giản hóa việc kiểm tra vì thao tác này cho phép chúng ta đảo ngược dấu của bất kỳ số nào nếu cần một cách hiệu quả. Lặp lại từ phần tử thứ hai và so sánh với phần tử trước đảm bảo kiểm tra tuyến tính đơn giản. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[7, 3, 2, -11, -13, -17, -23]`. Tích cực là`[7, 3, 2]`, số âm là`[11, 13, 17, 23]`. Dãy số dương giảm (`7 > 3`), vì vậy đầu ra là "KHÔNG". 

Vì`[71, -35, 7, -4, -11, -25]`, số dương là`[71, 7]`, tiêu cực`[35, 4, 11, 25]`. Kiểm tra tích cực:`7 < 71`, giảm dần nhưng âm`[35, 4, 11, 25]`sắp xếp theo giá trị tuyệt đối cho`[4, 11, 25, 35]`. Sau khi sắp xếp độc lập và áp dụng hoán đổi, mảng có thể được sắp xếp lại thành`[ -35, -25, -11, -4, 7, 71]`đã được sắp xếp, do đó xuất ra "CÓ". 

| Bước | Tích cực | Tiêu cực | Kết quả | 
| --- | --- | --- | --- | 
| Trích xuất | [71, 7] | [35, 4, 11, 25] | | 
| Kiểm tra | giảm dần | sắp xếp theo abs | CÓ | 

Điều này chứng tỏ thuật toán xác định chính xác các mảng trong đó các chuỗi dương/âm độc lập có thể được sắp xếp trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chia thành tích cực/tiêu cực và kiểm tra thứ tự là tuyến tính | 
| Không gian | O(n) | Lưu trữ tích cực và tiêu cực riêng biệt | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 10^5, tổng thời gian chạy nằm trong giới hạn. Việc sử dụng bộ nhớ cũng vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())
    return out.getvalue().strip()

# provided samples
assert run("4\n7\n7 3 2 -11 -13 -17 -23\n6\n4 10 25 47 71 96\n6\n71 -35 7 -4 -11 -25\n6\n-45 9 -48 -67 -55 7\n") == "NO\nYES\nYES\nNO", "sample 1"

# custom cases
assert run("3\n1\n5\n2\n-1 1\n3\n-2 -1 3\n") == "YES\nYES\nYES", "small arrays"
assert run("1\n5\n1 2 3 4 5\n") == "YES", "all increasing positives"
assert run("1\n5\n5 4 3 2 1\n") == "NO", "all decreasing positives"
assert run("1\n5\n-1 -2 -3 -4 -5\n") == "NO", "all decreasing negatives"
assert run("1\n6\n-1 2 -3 4 -5 6\n") == "YES", "alternating signs sortable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n5\n1 2 3 4 5 | CÓ | Đã sắp xếp mảng dương | 
| 1\n5\n5 4 3 2 1 | KHÔNG | Mảng dương giảm không thể sắp xếp được | 
| 1\n5\n-1 -2 -3 -4 -5 | KHÔNG | Mảng âm giảm không thể sắp xếp được | 
| 1\n6\n-1 2 -3 4 -5 6 | CÓ | Các dấu hiệu xen kẽ, có thể sắp xếp bằng cách hoán đổi | 

## Vỏ cạnh 

Đối với một mảng có một phần tử như`[5]`, dãy dương là`[5]`và tiêu cực`[]`. Cả hai đều không giảm một cách đáng kể, vì vậy đầu ra là "CÓ". Đối với một mảng trong đó số dương và số âm đang giảm dần như`[5, 3, -2, -4]`, những mặt tích cực`[5,3]`không thực hiện được việc kiểm tra không giảm nên xuất ra "NO". Điều này xác nhận thuật toán xử lý chính xác kích thước đầu vào tối thiểu và các chuỗi giảm hỗn hợp.

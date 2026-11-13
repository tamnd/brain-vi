---
title: "CF 1691A - Đánh bại tỷ lệ cược"
description: "Chúng ta được cho một dãy số nguyên và chúng ta cần loại bỏ càng ít số càng tốt để mỗi cặp số liên tiếp trong dãy còn lại có tổng bằng một số chẵn. Tổng của hai số là số chẵn nếu cả hai số đều chẵn hoặc cả hai đều là số lẻ."
date: "2026-06-09T23:08:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1691
codeforces_index: "A"
codeforces_contest_name: "CodeCraft-22 and Codeforces Round 795 (Div. 2)"
rating: 800
weight: 1691
solve_time_s: 113
verified: true
draft: false
---

[CF 1691A - Đánh bại tỷ lệ](https://codeforces.com/problemset/problem/1691/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta cần loại bỏ càng ít số càng tốt để mỗi cặp số liên tiếp trong dãy còn lại có tổng bằng một số chẵn. Tổng của hai số là số chẵn nếu cả hai số đều chẵn hoặc cả hai đều là số lẻ. Do đó, điều kiện chuyển thành yêu cầu dãy còn lại phải bao gồm tất cả các số chẵn hoặc tất cả các số lẻ ở các vị trí liên tiếp. Chúng ta không bị ràng buộc bởi thứ tự ban đầu của các con số, nhưng chúng ta phải giữ nguyên thứ tự mà chúng ta đã giữ; chúng ta không thể sắp xếp lại các phần tử. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của chuỗi và chính chuỗi đó. Tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá 100.000. Giới hạn này ngụ ý rằng bất kỳ giải pháp nào có độ phức tạp tuyến tính kém hơn so với độ dài chuỗi có thể quá chậm. Các thuật toán có hành vi O(n^2) sẽ thực hiện khoảng 10 tỷ thao tác trong trường hợp xấu nhất và bị loại trừ. Phương pháp tiếp cận tuyến tính hoặc tuyến tính là khả thi. 

Trường hợp cạnh tinh tế là khi chuỗi đã hợp lệ, chẳng hạn như`[3, 5, 9, 7]`. Ở đây không cần loại bỏ vì các cặp liên tiếp đều chẵn. Một trường hợp phức tạp khác là khi chuỗi xen kẽ tính chẵn lẻ như`[2, 3, 4, 5]`. Một cách tiếp cận ngây thơ chỉ xem xét vi phạm đầu tiên có thể loại bỏ thành phần sai và yêu cầu loại bỏ nhiều hơn mức cần thiết. Cách tiếp cận đúng đòi hỏi phải hiểu tổng số số lẻ và số chẵn để xác định số lần loại bỏ tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại chuỗi và cố gắng loại bỏ các phần tử một cách tham lam bất cứ khi nào một cặp liên tiếp có tổng thành một số lẻ. Chúng ta có thể thử loại bỏ phần tử thứ nhất hoặc thứ hai của cặp vi phạm và tiếp tục kiểm tra trình tự một cách đệ quy. Phương pháp này hoạt động vì việc xóa một số sẽ vi phạm nhưng nó nhanh chóng trở nên không hiệu quả. Mỗi lần loại bỏ có thể dẫn đến một lệnh gọi đệ quy kiểm tra gần như toàn bộ chuỗi còn lại, mang lại độ phức tạp trong trường hợp xấu nhất là O(n^2). Đối với n lên tới 100.000, điều này là quá chậm. 

Thông tin chi tiết quan trọng là một cặp liên tiếp chỉ có tổng là số chẵn nếu cả hai số đều có cùng tính chẵn lẻ. Điều này có nghĩa là dãy cuối cùng phải chứa các số có cùng tính chẵn lẻ. Vì vậy, giải pháp tối ưu chỉ đơn giản là quyết định nên giữ tất cả số chẵn hay toàn số lẻ. Số lần loại bỏ tối thiểu là số nhỏ hơn trong số số chẵn và số lẻ. Chúng ta không cần mô phỏng việc loại bỏ các phần tử riêng lẻ hoặc kiểm tra từng cặp vì tính chẵn lẻ không phụ thuộc vào vị trí. Quan sát này làm giảm vấn đề thành quét tuyến tính để đếm tính chẵn lẻ, cho thời gian O(n) cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm sẽ được xử lý độc lập. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc độ dài chuỗi và chính chuỗi đó. 
3. Khởi tạo hai bộ đếm: một cho số chẵn và một cho số lẻ. 
4. Lặp lại trình tự. Đối với mỗi phần tử, tăng bộ đếm chẵn nếu nó chia hết cho 2, nếu không thì tăng bộ đếm lẻ. 
5. Tính giá trị nhỏ nhất giữa số chẵn và số lẻ. Đây là số lần loại bỏ tối thiểu cần thiết vì việc giữ nhóm lớn hơn sẽ bảo toàn số phần tử tối đa trong khi vẫn thỏa mãn điều kiện tổng liên tiếp. 
6. In số lượng tối thiểu này cho test case. 

Tại sao nó hoạt động: Sau khi đếm tính chẵn lẻ của tất cả các số, chúng ta biết chính xác có bao nhiêu phần tử chúng ta phải loại bỏ để làm cho tất cả các số còn lại có cùng tính chẵn lẻ. Điều này đảm bảo rằng mỗi cặp liên tiếp sẽ có tổng là một số chẵn. Không có cấu hình nào khác có thể tạo ra ít lượt xóa hơn vì bất kỳ sự kết hợp nào giữa số lẻ và số chẵn chắc chắn sẽ tạo ra ít nhất nhiều vi phạm bằng quy mô của nhóm nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    even_count = sum(1 for x in a if x % 2 == 0)
    odd_count = n - even_count
    print(min(even_count, odd_count))
```Mã đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng I/O nhanh. Nó đếm số số chẵn và số lẻ trong một lượt, sau đó in số nhỏ nhất của hai số đó. sử dụng`n - even_count`để tính số lẻ sẽ tránh vòng lặp thứ hai, làm cho mã sạch hơn và nhanh hơn một chút. Các trường hợp cạnh được xử lý một cách tự nhiên: nếu tất cả các số đều là số chẵn,`odd_count`bằng 0, do đó mức tối thiểu bằng 0, biểu thị chính xác rằng không cần loại bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trình tự đầu vào:`[2, 4, 3, 6, 8]`| Yếu tố | Chẵn lẻ | chẵn_count | lẻ_count | 
| --- | --- | --- | --- | 
| 2 | thậm chí | 1 | 0 | 
| 4 | thậm chí | 2 | 0 | 
| 3 | lẻ | 2 | 1 | 
| 6 | thậm chí | 3 | 1 | 
| 8 | thậm chí | 4 | 1 | 

Tối thiểu của`even_count`Và`odd_count`là 1. Loại bỏ số lẻ 3 sẽ dẫn đến`[2,4,6,8]`, thỏa mãn điều kiện. 

### Ví dụ 2 

Trình tự đầu vào:`[3, 5, 9, 7, 1, 3]`| Yếu tố | Chẵn lẻ | chẵn_count | lẻ_count | 
| --- | --- | --- | --- | 
| 3 | lẻ | 0 | 1 | 
| 5 | lẻ | 0 | 2 | 
| 9 | lẻ | 0 | 3 | 
| 7 | lẻ | 0 | 4 | 
| 1 | lẻ | 0 | 5 | 
| 3 | lẻ | 0 | 6 | 

Tối thiểu của`even_count`Và`odd_count`là 0. Không cần loại bỏ vì tất cả các số đều là số lẻ nên các tổng liên tiếp là số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần đi qua chuỗi để đếm số chẵn và số lẻ | 
| Không gian | O(n) | Lưu trữ trình tự tạm thời; bộ đếm sử dụng O(1) | 

Với tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 100.000, tổng số thao tác dưới 200.000, phù hợp thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        even_count = sum(1 for x in a if x % 2 == 0)
        odd_count = n - even_count
        print(min(even_count, odd_count))
    return output.getvalue().strip()

# Provided samples
assert run("2\n5\n2 4 3 6 8\n6\n3 5 9 7 1 3\n") == "1\n0", "sample 1"

# Custom cases
assert run("1\n3\n2 4 6\n") == "0", "all even, no removal"
assert run("1\n4\n1 3 5 7\n") == "0", "all odd, no removal"
assert run("1\n4\n2 3 4 5\n") == "2", "alternating parity"
assert run("1\n1\n42\n") == "0", "single element"
assert run("1\n6\n2 2 2 1 1 1\n") == "3", "half-half sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 yếu tố đều chẵn | 0 | Không cần gỡ bỏ | 
| 4 yếu tố đều lẻ | 0 | Không cần gỡ bỏ | 
| Luân phiên chẵn/lẻ | 2 | Tính toán chính xác lượng loại bỏ tối thiểu | 
| Yếu tố đơn | 0 | Vỏ cạnh có n = 1 | 
| Nửa chẵn, nửa lẻ | 3 | Xử lý đúng cách khi số lượng bằng nhau | 

## Vỏ cạnh 

Nếu chuỗi có tất cả các phần tử có cùng tính chẵn lẻ thì thuật toán sẽ đưa ra đúng 0. Ví dụ:`[2,4,6]`kết quả là`even_count=3`,`odd_count=0`,`min(even_count, odd_count)=0`. Nếu trình tự xen kẽ tính chẵn lẻ, chẳng hạn như`[2,3,4,5]`, số đếm là`even_count=2`,`odd_count=2`, mang lại số lần xóa tối thiểu là 2. Điều này đảm bảo thuật toán không bao giờ đánh giá thấp số lần xóa. Chuỗi phần tử đơn được xử lý một cách tự nhiên:`even_count`hoặc`odd_count`là 1, giá trị còn lại là 0, vì vậy giá trị tối thiểu là 0. Giải pháp này hiệu quả trên tất cả các đầu vào hợp lệ.

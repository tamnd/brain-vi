---
title: "CF 2112B - Mảng thu nhỏ"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta muốn làm cho nó \"đẹp.\" Một mảng đẹp có ít nhất hai phần tử và chứa ít nhất một cặp phần tử liền kề có hiệu nhiều nhất là một."
date: "2026-06-08T04:26:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2112
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 180 (Rated for Div. 2)"
rating: 1100
weight: 2112
solve_time_s: 71
verified: true
draft: false
---

[CF 2112B - Thu hẹp mảng](https://codeforces.com/problemset/problem/2112/B) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta muốn làm cho nó trở nên "đẹp". Một mảng đẹp có ít nhất hai phần tử và chứa ít nhất một cặp phần tử liền kề có hiệu số lớn nhất là một. Chúng ta có thể kết hợp nhiều lần hai phần tử liền kề thành một số duy nhất trong phạm vi của chúng, thu nhỏ mảng một cách hiệu quả từng phần tử một. Mục tiêu là xác định số lượng tối thiểu các thao tác như vậy cần thiết để làm cho mảng trở nên đẹp mắt hoặc báo cáo rằng điều đó là không thể. 

Kích thước đầu vào có thể lên tới 1000 phần tử cho mỗi trường hợp thử nghiệm và có thể có tới 200 trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ thuật toán nào xem xét mọi chuỗi hoạt động có thể có theo cách mạnh mẽ đều không thực tế, bởi vì số lượng kết hợp tăng theo cấp số nhân. Mỗi phần tử có thể có phạm vi lên tới một triệu, nhưng bản thân các giá trị thực tế không đặt ra nhiều hạn chế; tính năng quan trọng là sự khác biệt giữa các phần tử liền kề. 

Một điểm tinh tế là mảng có thể đã đẹp rồi. Ví dụ,`[1, 3, 3, 7]`đã chứa`3`Và`3`, do đó không cần thực hiện thao tác nào. Một tình huống phức tạp khác là một mảng có kích thước hai với các phần tử cách xa nhau, chẳng hạn như`[6, 9]`. Ở đây, việc kết hợp chúng sẽ làm giảm mảng thành một phần tử duy nhất, không thể đáp ứng yêu cầu "ít nhất hai phần tử". Việc triển khai ngây thơ có thể thực hiện sai một thao tác và trả về câu trả lời thay vì`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi chuỗi hoạt động có thể xảy ra. Đối với mỗi cặp phần tử liền kề, chúng ta có thể thử tất cả các số nguyên có thể có trong phạm vi của chúng và thu nhỏ mảng theo cách đệ quy, kiểm tra xem có xuất hiện một cặp đẹp hay không. Về nguyên tắc, điều này đúng, nhưng số lượng thao tác sẽ theo cấp số nhân theo chiều dài mảng, đại khái là`O(n * (max(a_i) - min(a_i))^n)`, điều này hoàn toàn không thể thực hiện được đối với`n = 1000`. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét tất cả các chuỗi hoạt động có thể xảy ra. Một mảng đẹp chỉ yêu cầu **một cặp liền kề có chênh lệch nhiều nhất là một**. Điều đó có nghĩa là nếu bất kỳ cặp liền kề nào đã thỏa mãn điều kiện này thì không cần thực hiện thao tác nào. Nếu không có cặp nào như vậy tồn tại và tất cả các hiệu liền kề ít nhất là hai, thì việc kết hợp hai phần tử liền kề luôn tạo ra một số trong phạm vi đó, số này sẽ cách xa một trong các số ban đầu nhiều nhất là một, đảm bảo hiệu quả rằng chúng ta có thể giảm mảng sang trạng thái mới. Vì chúng ta có thể chọn số nguyên bên trong khoảng, **một thao tác luôn đủ cho các mảng có kích thước lớn hơn hai** để tạo ra một cặp đẹp. Ngoại lệ duy nhất là một mảng có kích thước chính xác là hai với các phần tử khác nhau nhiều hơn một, vì việc thu nhỏ nó sẽ chỉ còn lại một phần tử. 

Quan sát này làm giảm giải pháp thành ba kiểm tra đơn giản. Chúng tôi quét mảng để tìm một cặp vốn đã đẹp. Nếu tìm thấy thì câu trả lời là`0`. Nếu không, chúng ta kiểm tra xem mảng có chính xác hai phần tử hay không và chúng khác nhau nhiều hơn một phần tử, trong trường hợp đó câu trả lời là`-1`. Nếu không, chúng ta luôn có thể làm cho mảng trở nên đẹp đẽ chỉ bằng một thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * (max(a_i)-min(a_i))^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Chúng tôi sẽ lặp lại quy trình cho từng trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Lặp qua các cặp liền kề`a[i]`Và`a[i + 1]`vì`i = 0`ĐẾN`n - 2`. Nếu có cặp nào thỏa mãn`|a[i] - a[i + 1]| <= 1`, quay lại ngay`0`. Mảng đã đẹp rồi. 
4. Nếu không có cặp liền kề nào thỏa mãn điều kiện, hãy kiểm tra độ dài của mảng. Nếu như`n == 2`, trở lại`-1`, bởi vì thực hiện bất kỳ thao tác nào sẽ thu hẹp mảng thành một phần tử duy nhất, không thể đẹp được. 
5. Đối với tất cả các trường hợp khác mà`n > 2`và ban đầu không có cặp nào đẹp, trả lại`1`. Một thao tác đơn lẻ trên bất kỳ cặp liền kề nào sẽ tạo ra một số mới nằm trong một trong các phần tử ban đầu, tạo ra một cặp đẹp. 

Tại sao nó hoạt động: điều bất biến là một mảng đẹp chỉ cần một cặp duy nhất có nhiều nhất là một. Bất kỳ mảng nào có kích thước lớn hơn hai đều cho phép chúng ta chọn hai số liền kề và chọn một số trong khoảng của chúng để tạo ra sự khác biệt tối đa là một với một trong các số lân cận. Do đó, chúng ta chỉ cần kiểm tra các cặp ban đầu hoặc thực hiện một thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    found = False
    for i in range(n - 1):
        if abs(a[i] - a[i + 1]) <= 1:
            found = True
            break
    
    if found:
        print(0)
    elif n == 2:
        print(-1)
    else:
        print(1)
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi mảng, nó sẽ kiểm tra tất cả các cặp liền kề để tìm sự khác biệt nhiều nhất là một. Nếu tìm thấy, câu trả lời là không. Nếu mảng có đúng hai phần tử và không có cặp phần tử đẹp thì câu trả lời là`-1`. Mặt khác, đối với các mảng lớn hơn hai không có cặp đẹp ban đầu, chỉ cần một thao tác là đủ để tạo ra cặp cần thiết. sử dụng`abs()`đảm bảo xử lý chính xác sự khác biệt bất kể thứ tự. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

| Bước | một | n | Kiểm tra cặp | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3,3,7] | 4 | | a[1]-a[2] | 
| 2 | [6,9] | 2 | | a[0]-a[1] | 
| 3 | [3,1,3,7] | 4 | | a[0]-a[1] | 
| 4 | [1,3,5,2] | 4 | | a[0]-a[1] | 

Bảng này cho thấy mã xác định chính xác các phép toán bằng 0 nếu tồn tại một cặp đẹp, xử lý trường hợp kích thước 2 không thể thực hiện được và sử dụng một phép toán khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét mảng một lần để tìm những khác biệt liền kề | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ mảng đó | 

Với`n <= 1000`Và`t <= 200`, tổng số thao tác nhiều nhất là`2*10^5`, trong giới hạn 2 giây. 

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
        
        found = False
        for i in range(n - 1):
            if abs(a[i] - a[i + 1]) <= 1:
                found = True
                break
        
        if found:
            print(0)
        elif n == 2:
            print(-1)
        else:
            print(1)
    
    return output.getvalue().strip()

# Provided samples
assert run("4\n4\n1 3 3 7\n2\n6 9\n4\n3 1 3 7\n4\n1 3 5 2\n") == "0\n-1\n1\n1"

# Custom cases
assert run("3\n2\n1 2\n2\n1 3\n3\n5 8 2\n") == "0\n-1\n1"
assert run("2\n3\n7 10 13\n4\n1 2 4 6\n") == "1\n1"
assert run("1\n2\n1000000 1000000\n") == "0"
assert run("1\n2\n1 1000000\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố đã đẹp rồi | 0 | Phát hiện mảng đã đẹp | 
| 2 yếu tố cách xa nhau | -1 | Xử lý không thể thu nhỏ cho kích thước-2 | 
| 3+ yếu tố, không có cặp nào đẹp | 1 | Xác nhận thao tác đơn lẻ là đủ | 
| Giá trị phần tử tối đa | 0 hoặc 1 | Xử lý số lượng lớn mà không bị tràn | 

## Vỏ cạnh 

Hãy xem xét`[1, 1000000]`. Thuật toán lặp lại một lần, tìm thấy`abs(1-1000000)=999999>1`, thấy`n==2`và đầu ra chính xác`-1`. Một trường hợp cạnh khác là`[1000000, 1000000, 1000000]`. Không cần phải kiểm tra cặp đẹp ban đầu, nhưng vì`n>2`, đầu ra mã`1`, điều này hiệu quả vì chúng ta có thể kết hợp bất kỳ cặp liền kề nào thành`1000000`, tạo ra sự khác biệt đẹp mắt với phần tử còn lại. Điều bất biến đúng: bất kỳ mảng nào lớn hơn hai luôn có thể trở nên đẹp trong một

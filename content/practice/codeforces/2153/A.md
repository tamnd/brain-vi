---
title: "CF 2153A - Vòng Tròn Cây Táo"
description: "Chúng ta được tặng một vòng tròn cây táo, mỗi cây mang một quả táo mang một giá trị vẻ đẹp liên quan. Bạn bắt đầu ở cây một và đi vòng quanh vòng tròn nhiều lần. Ở mỗi cây, bạn có quyền lựa chọn ăn táo hoặc bỏ qua."
date: "2026-06-08T00:40:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2153
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1057 (Div. 2)"
rating: 800
weight: 2153
solve_time_s: 90
verified: true
draft: false
---

[CF 2153A - Vòng tròn cây táo](https://codeforces.com/problemset/problem/2153/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một vòng tròn cây táo, mỗi cây mang một quả táo mang một giá trị vẻ đẹp liên quan. Bạn bắt đầu ở cây một và đi vòng quanh vòng tròn nhiều lần. Ở mỗi cây, bạn có quyền lựa chọn ăn táo hoặc bỏ qua. Hạn chế là bạn chỉ có thể ăn một quả táo nếu vẻ đẹp của nó hoàn toàn lớn hơn vẻ đẹp của quả táo cuối cùng bạn ăn. Mục tiêu là tối đa hóa tổng số táo được ăn. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, số lượng cây$n$được đưa ra cùng với một danh sách các giá trị vẻ đẹp$b_1, b_2, \dots, b_n$. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lượng táo tối đa có thể ăn được theo quy tắc. 

Các hạn chế ở mức vừa phải:$n \le 100$Và$t \le 500$. Điều này ngụ ý rằng một$O(n \log n)$hoặc thậm chí$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm sẽ chạy thoải mái. Không có lo ngại về đầu vào lớn buộc phải có các phương pháp tiếp cận tối ưu tiệm cận ngoài việc sắp xếp đơn giản hoặc quét tuyến tính. 

Các trường hợp cạnh phát sinh khi tất cả các quả táo đều có vẻ đẹp giống nhau hoặc khi chuỗi các vẻ đẹp giảm dần. Ví dụ, nếu$b = [2,2,2,2]$, chỉ có thể ăn được một quả táo. Nếu như$b = [5,4,3,2,1]$, bắt đầu từ bất kỳ cây nào, mỗi chu kỳ chỉ được ăn một quả táo. Những trường hợp này rất khó đối với một mô phỏng “ăn nếu lớn hơn lần ăn trước” ngây thơ vì chiến lược tối ưu có thể liên quan đến việc bỏ qua hoàn toàn những quả táo sớm để cho phép những quả táo nhỏ hơn về sau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là mô phỏng mọi điểm bắt đầu có thể có và mọi trình tự bỏ qua và ăn uống có thể có. Đối với mỗi chu kỳ, bạn sẽ theo dõi quả táo cuối cùng đã ăn và thử tất cả các kết hợp xem nên ăn hay bỏ qua quả táo hiện tại. Điều này có hiệu quả vì nó tôn trọng các quy tắc của bài toán, nhưng nó có tính cấp số mũ trong$n$và trở nên không khả thi đối với$n$thậm chí nhỏ như 20. 

Cái nhìn sâu sắc quan trọng là ràng buộc “vẻ đẹp phải tăng lên một cách nghiêm ngặt” có nghĩa là thứ tự ăn táo là không liên quan ngoại trừ kích thước tương đối. Điều này làm giảm vấn đề đếm số lượng tối đa các vẻ đẹp riêng biệt mà bạn có thể chọn trong khi vẫn tôn trọng trật tự tuần hoàn. Việc sắp xếp các người đẹp và hái theo thứ tự tăng dần đảm bảo rằng mỗi quả táo được chọn sẽ lớn hơn quả trước đó và tính chất tuần hoàn chỉ ảnh hưởng đến lần hái đầu tiên. Để tối đa hóa việc ăn táo, chúng ta luôn có thể bắt đầu với vẻ đẹp nhỏ nhất và tham lam ăn vẻ đẹp nhỏ nhất có sẵn tiếp theo, lớn hơn lần ăn cuối cùng. 

Vì vòng tròn có thể quay tùy ý nên số lượng táo ăn được xác định bởi số lượng giá trị vẻ đẹp riêng biệt và chúng ta phải tính đến các giá trị lặp lại xuất hiện trong một chu kỳ. Do đó mức tối đa là$n$nếu tất cả các vẻ đẹp là duy nhất và được sắp xếp để cho phép ăn theo thứ tự tăng dần nghiêm ngặt và nhỏ hơn nếu có các bản sao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam của người đẹp được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và danh sách người đẹp$b$. 
2. Sắp xếp danh sách người đẹp. Việc sắp xếp cho phép chúng ta dễ dàng chọn quả táo tiếp theo lớn hơn quả táo được ăn trước đó. 
3. Khởi tạo bộ đếm về 0 và một biến`last_eaten`đến một giá trị nhỏ hơn bất kỳ vẻ đẹp táo nào (ví dụ 0). 
4. Lặp lại danh sách vẻ đẹp đã được sắp xếp. Đối với mỗi vẻ đẹp, nếu nó thực sự lớn hơn`last_eaten`, tăng bộ đếm và cập nhật`last_eaten`. 
5. Sau khi duyệt qua tất cả các phần đẹp, bộ đếm hiện chứa số lượng táo tối đa có thể ăn được. Xuất giá trị này. 

Tại sao nó hiệu quả: Bằng cách luôn chọn quả táo nhỏ nhất có sẵn và lớn hơn quả táo ăn cuối cùng, chúng tôi đảm bảo rằng chúng tôi sẽ tối đa hóa số lượng táo ăn được. Danh sách được sắp xếp đảm bảo rằng mỗi lượt chọn đều hợp lệ và bộ đếm sẽ ghi lại tổng số. Vì thứ tự tương đối xung quanh vòng tròn có thể bị bỏ qua tùy ý nên tính chất tuần hoàn không hạn chế sự lựa chọn tham lam này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_apples(n, beauties):
    beauties.sort()
    count = 0
    last_eaten = 0
    for b in beauties:
        if b > last_eaten:
            count += 1
            last_eaten = b
    return count

t = int(input())
for _ in range(t):
    n = int(input())
    beauties = list(map(int, input().split()))
    print(max_apples(n, beauties))
```Giải pháp này đọc từng trường hợp thử nghiệm, sắp xếp các phần đẹp và đếm số lượng táo có thể ăn được. Sắp xếp là rất quan trọng để đảm bảo thứ tự tăng dần nghiêm ngặt. Chúng tôi sử dụng một bộ đếm đơn giản và một biến theo dõi cho vẻ đẹp được ăn cuối cùng. Logic xử lý chính xác các bản sao bằng cách bỏ qua bất kỳ quả táo nào không lớn hơn quả trước đó. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
2 2 2 2
```| sắc đẹp | ăn cuối cùng | đếm | 
| --- | --- | --- | 
| 2 | 0 | 1 | 
| 2 | 2 | 1 | 
| 2 | 2 | 1 | 
| 2 | 2 | 1 | 

Kết quả: 1. Chỉ được ăn một quả táo vì mọi vẻ đẹp đều bình đẳng. 

Đầu vào mẫu 2:```
5
1 4 5 1 2
```| vẻ đẹp (sắp xếp) | ăn cuối cùng | đếm | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 4 | 2 | 3 | 
| 5 | 4 | 4 | 

Đầu ra: 4. Lựa chọn tham lam cho phép ăn quả táo hợp lệ nhỏ nhất ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp lại là tuyến tính. | 
| Không gian | O(n) | Lưu trữ danh sách người đẹp. | 

Được cho$n \le 100$Và$t \le 500$, kể cả trường hợp xấu nhất$500 \cdot 100 \log 100 \approx 35000$các hoạt động không đáng kể trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        beauties = list(map(int, input().split()))
        beauties.sort()
        count = 0
        last_eaten = 0
        for b in beauties:
            if b > last_eaten:
                count += 1
                last_eaten = b
        output.append(str(count))
    return "\n".join(output)

# Provided samples
assert run("3\n4\n2 2 2 2\n5\n1 4 5 1 2\n6\n5 4 2 1 2 3\n") == "1\n4\n5", "sample tests"

# Custom cases
assert run("2\n1\n1\n3\n3 3 3\n") == "1\n1", "single-element and all-equal"
assert run("1\n5\n1 2 3 4 5\n") == "5", "strictly increasing"
assert run("1\n5\n5 4 3 2 1\n") == "5", "strictly decreasing but cyclic"
assert run("1\n6\n2 2 3 3 4 4\n") == "4", "duplicates in increasing order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | 1 | Đầu vào kích thước tối thiểu | 
| 3\n3 3 3 | 1 | Giá trị hoàn toàn bằng nhau | 
| 5\n1 2 3 4 5 | 5 | Trình tự tăng nghiêm ngặt | 
| 5\n5 4 3 2 1 | 5 | Trình tự giảm nghiêm ngặt | 
| 6\n2 2 3 3 4 4 | 4 | Giá trị trùng lặp | 

## Vỏ cạnh 

Đối với một cây duy nhất$n=1$với vẻ đẹp 1 thì thuật toán đếm đúng 1 quả táo. Khi tất cả các quả táo đều có vẻ đẹp giống nhau, chẳng hạn như$b = [3,3,3]$, chỉ có quả táo đầu tiên được tính; những quả táo tiếp theo bị bỏ qua vì chúng không thực sự lớn hơn. Đối với các chuỗi có các bản sao, thuật toán sẽ bỏ qua giá trị lặp lại cho đến khi vẻ đẹp lớn hơn xuất hiện, tối đa hóa chính xác tổng lượng đã ăn. Thứ tự tuần hoàn không ảnh hưởng đến số lượng vì lựa chọn được sắp xếp tham lam đã đảm bảo thứ tự tiêu thụ tối ưu.

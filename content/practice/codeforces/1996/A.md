---
title: "CF 1996A - Chân"
description: "Trang trại chỉ có gà và bò, và nông dân John đếm tổng cộng n chân. Gà đóng góp mỗi con 2 chân và bò mỗi con đóng góp 4 chân. Câu hỏi yêu cầu số lượng động vật nhỏ nhất có thể phù hợp với tổng số chân."
date: "2026-06-08T14:41:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1996
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 962 (Div. 3)"
rating: 800
weight: 1996
solve_time_s: 132
verified: true
draft: false
---

[CF 1996A - Chân](https://codeforces.com/problemset/problem/1996/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, toán học, tìm kiếm ba ngôi 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trang trại chỉ có gà và bò, và nông dân John đếm tổng cộng`n`chân. Gà đóng góp mỗi con 2 chân và bò mỗi con đóng góp 4 chân. Câu hỏi yêu cầu số lượng động vật nhỏ nhất có thể phù hợp với tổng số chân. Về cơ bản, chúng tôi đang phân phối`n`chân thành nhóm 2 và 4, đồng thời giảm thiểu tổng số động vật. 

Đầu vào là nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là một số nguyên chẵn`n`đại diện cho tổng số chân được tính. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm: số lượng động vật tối thiểu có thể tạo ra chính xác`n`chân. 

Các hạn chế là nhỏ:`n`tăng lên đến 2000 và`t`lên tới 1000. Vì`n`bị giới hạn, mọi nghiệm có phép toán tuyến tính trong`n`mỗi trường hợp thử nghiệm (O(n)) sẽ chạy thoải mái trong vòng chưa đầy một giây. Chúng tôi cũng biết`n`luôn là số chẵn, điều này giúp đơn giản hóa một số trường hợp biên - chúng ta không bao giờ phải xử lý tổng số chân lẻ. 

Một trường hợp cạnh phát sinh khi`n`rất nhỏ, giống như`2`. Trong trường hợp đó, khả năng duy nhất là chỉ có một con gà, mặc dù về mặt kỹ thuật có nhiều tổ hợp động vật khác nhau sẽ dẫn đến`n`chân trong các ví dụ lớn hơn. Một điểm tinh tế khác là số lượng động vật tối thiểu xảy ra khi chúng ta tối đa hóa số lượng bò vì mỗi con bò đóng góp nhiều chân hơn cho mỗi con vật, làm giảm tổng số lượng. Một cách tiếp cận bất cẩn có thể cố gắng tách tất cả các chân của gà trước, tạo ra quá nhiều động vật. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là lặp lại tất cả số lượng bò có thể có từ 0 đến`n//4`, tính số chân còn lại của gà và theo dõi tổng số động vật tối thiểu. Điều này đúng vì nó liệt kê tất cả các kết hợp khả thi. Số lần thực hiện tỷ lệ thuận với`n//4`mỗi trường hợp thử nghiệm. Với`n`cho đến năm 2000, con số này là khoảng 500 lần lặp cho mỗi trường hợp thử nghiệm, có thể chấp nhận được với các ràng buộc, nhưng điều đó là không cần thiết vì giải pháp có thể được biểu thị bằng toán học. 

Điểm mấu chốt là việc giảm thiểu số lượng động vật tương đương với việc tối đa hóa số lượng bò. Nếu mỗi con bò có 4 chân và mỗi con gà có 2 chân thì trước tiên chúng ta phân bổ càng nhiều bò càng tốt mà không vượt quá`n`. Các chân còn lại được giao cho gà. Về mặt đại số, nếu`n`là số chẵn thì số lượng động vật tối thiểu là`n // 4`bò cộng`(n % 4) / 2`gà. Từ`n`là chẵn,`(n % 4)`là 0 hoặc 2, do đó phép chia luôn cho kết quả là số nguyên. Điều này chuyển đổi vấn đề thành một phép tính số học đơn giản cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận nhưng chậm không cần thiết | 
| Tối ưu | O(1) | O(1) | Được chấp nhận và hiệu quả | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc tổng số chân`n`. Từ`n`được đảm bảo chẵn, chúng ta có thể chia cho 2 hoặc 4 một cách an toàn mà không cần lo lắng về phân số. 
2. Tính số lượng bò tối đa là`n // 4`. Mỗi con bò sử dụng 4 chân nên việc sử dụng càng nhiều con bò càng tốt sẽ làm giảm số lượng con vật còn lại. 
3. Tính các chân còn lại là`n % 4`. Bởi vì`n`chẵn, đây là 0 hoặc 2. 
4. Chân còn lại giao cho gà. Vì mỗi con gà có 2 chân nên số con gà là`(n % 4) // 2`. 
5. Cộng số bò và số gà để có tổng số lượng vật nuôi tối thiểu. 
6. In kết quả cho từng test. 

Tại sao nó hoạt động: Điều bất biến là số lượng động vật 4 chân tối đa luôn tạo ra tổng số động vật tối thiểu. Ít bò hơn sẽ cần nhiều gà hơn để đáp ứng tổng số chân như nhau, làm tăng tổng số động vật. Bởi vì`n`chẵn, số chân còn lại sau khi chia bò luôn chia hết cho 2 nên việc tính gà là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    cows = n // 4
    chickens = (n % 4) // 2
    print(cows + chickens)
```Trong mã này,`n // 4`xác định số lượng bò tối đa, và`(n % 4) // 2`xác định số gà còn lại. Chúng ta không cần bất kỳ vòng lặp hay điều kiện nào vì phép chia modulo và số nguyên xử lý tất cả các số chẵn`n`. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh cho nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào: 2 

| n | bò = n/4 | phần còn lại = n%4 | gà = còn lại//2 | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 2 | 1 | 1 | 

Điều này cho thấy 2 chân tương ứng với 1 con gà, khẳng định số lượng con vật là tối thiểu. 

### Mẫu 2 

Đầu vào: 6 

| n | bò = n/4 | phần còn lại = n%4 | gà = còn lại//2 | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 6 | 1 | 2 | 1 | 2 | 

Một con bò dùng 4 chân, để lại 2 chân cho một con gà, tổng cộng là 2 con. Điều này xác nhận thuật toán tối đa hóa số bò đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với`t`lên tới 1000, tổng thời gian cũng nằm trong giới hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        cows = n // 4
        chickens = (n % 4) // 2
        print(cows + chickens)
    return output.getvalue().strip()

# provided samples
assert run("3\n2\n6\n8\n") == "1\n2\n2", "sample 1"

# custom cases
assert run("2\n4\n10\n") == "1\n3", "boundary case"
assert run("1\n2000\n") == "500", "maximum input"
assert run("1\n14\n") == "4", "even n not divisible by 4"
assert run("1\n16\n") == "4", "n divisible by 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4,10 | 1,3 | Vỏ cạnh có số chẵn nhỏ, một chia hết cho 4 | 
| 2000 | 500 | N tối đa, hiệu suất đầu vào lớn | 
| 14 | 4 | Thậm chí n không chia hết cho 4 | 
| 16 | 4 | n chia hết cho 4 | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất, n=2, thuật toán sẽ tính`0`bò và`1`con gà, đúng 1 con. Với n không chia hết cho 4 nhưng vẫn chẵn, chẳng hạn như n=6, chúng ta gán 1 con bò và 1 con gà, ít con hơn 3 con gà, chứng tỏ rằng việc tăng tối đa số con bò sẽ làm giảm tổng số con. Đối với n lớn, như n=2000, các phép chia số nguyên và modulo vẫn chính xác và nhanh chóng. Thuật toán xử lý tất cả n thậm chí trong phạm vi cho phép mà không cần bất kỳ cách viết hoa đặc biệt nào.

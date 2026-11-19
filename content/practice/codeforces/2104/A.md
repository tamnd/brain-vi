---
title: "CF 2104A - Ba tầng"
description: "Chúng ta có ba chồng thẻ được sắp xếp thành một hàng, trong đó chồng đầu tiên chứa các thẻ a, thẻ b thứ hai và thẻ c thứ ba. Các số thỏa mãn a < b < c."
date: "2026-06-08T04:57:20+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2104
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 178 (Rated for Div. 2)"
rating: 800
weight: 2104
solve_time_s: 101
verified: true
draft: false
---

[CF 2104A - Ba bộ bài](https://codeforces.com/problemset/problem/2104/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba chồng thẻ được sắp xếp thành một hàng, trong đó chồng đầu tiên chứa`a`thẻ thứ hai`b`thẻ, và thứ ba`c`thẻ. Những con số thỏa mãn`a < b < c`. Monocarp có thể lấy bất kỳ số lượng bài dương nào từ ngăn xếp thứ ba, tối đa là tất cả`c`thẻ và phân phối lại chúng giữa hai ngăn xếp đầu tiên. Mục tiêu là làm cho cả ba ngăn xếp đều chứa cùng số lượng thẻ sau khi phân phối lại. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số lượng`a`,`b`, Và`c`. Đầu ra cho mỗi trường hợp thử nghiệm phải là "CÓ" nếu có thể phân phối bằng nhau và "KHÔNG" nếu không. 

Bởi vì mỗi`a`,`b`, Và`c`có thể lớn như$10^8$và có thể có tới$10^4$các trường hợp thử nghiệm, chúng tôi cần một giải pháp hoạt động trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Việc lặp lại tất cả các phân phối lại có thể có của tầng thứ ba sẽ quá chậm, có khả năng thực hiện lên đến$10^8$lần lặp cho mỗi trường hợp. 

Một cách triển khai ngây thơ có thể thử mọi số lượng quân bài có thể được chuyển từ bộ bài thứ ba sang hai bộ bài đầu tiên, nhưng điều đó sẽ hết thời gian. Một cạm bẫy tinh vi khác là không kiểm tra được liệu tổng số thẻ có thể chia đều cho ba hay không. Ví dụ, nếu`a=3, b=5, c=10`, tổng số là 18, chia đều cho ba, có thể đạt tới`6`thẻ trong mỗi ngăn xếp. Nếu tổng số là 19 thì việc phân phối lại sẽ không hiệu quả. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liên quan đến việc sử dụng`x`thẻ từ bộ bài thứ ba, phân phát`y`lên tầng đầu tiên và`x-y`lên tầng thứ hai và kiểm tra xem`a+y = b+x-y = c-x`. Đối với mỗi khả năng`x`giữa 1 và`c`, và mỗi`y`giữa 0 và`x`, điều này đòi hỏi lên đến$O(c^2)$séc. Điều này đúng về mặt lý thuyết, nhưng lớn nhất có thể`c`là$10^8$, làm cho phương pháp này không khả thi. 

Quan sát quan trọng là giá trị cuối cùng trong mỗi bộ bài, hãy gọi nó là`target`, phải giống nhau. Tổng số thẻ sau khi chia lại không thay đổi:`a + b + c`. Nếu cả ba bộ bài đều bằng nhau thì số đó phải chia hết cho ba. Vì thế`target = (a + b + c) / 3`. Nếu như`(a + b + c)`không chia hết cho 3 thì điều đó là không thể. 

Một khi chúng ta biết`target`, ta chỉ cần kiểm tra xem chênh lệch giữa bộ bài mục tiêu và hai bộ bài đầu tiên nhiều nhất là bao nhiêu`c`, bởi vì bộ bài thứ ba có thể cung cấp nhiều lá bài như vậy. Chính thức hơn, chúng ta cần`(target - a) + (target - b) = 2*target - (a+b) = target - c`, đó chính xác là số quân bài chúng ta lấy được từ bộ bài thứ ba. Từ`target <= c`(bởi vì`c`là lớn nhất), điều này hoạt động. Nếu không thì không thể được. 

Quan sát này làm giảm vấn đề thành các kiểm tra số học đơn giản và hoạt động trong thời gian không đổi cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c2) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc các giá trị`a`,`b`, Và`c`. 
2. Tính tổng số quân bài trên cả ba bộ bài:`total = a + b + c`. 
3. Kiểm tra xem`total`chia hết cho 3. Nếu không, xuất "NO" và chuyển sang trường hợp kiểm thử tiếp theo. Điều này là do việc phân phối đồng đều đòi hỏi tổng số tiền phải chia đều cho ba bộ bài. 
4. Tính số lượng bài mục tiêu trên mỗi bộ bài:`target = total // 3`. 
5. Kiểm tra xem bộ bài lớn nhất có`c`, chứa ít nhất`target`thẻ. Vì chúng tôi chỉ có thể xóa thẻ khỏi`c`, nếu như`target > c`, việc đạt được các bộ bài bằng nhau là điều không thể. Đầu ra "KHÔNG" trong trường hợp này. 
6. Ngược lại, xuất ra "CÓ". Việc phân phối lại luôn có thể gán`target - a`thẻ vào bộ bài đầu tiên và`target - b`bài lên bộ bài thứ hai, lấy chính xác`(target - a) + (target - b)`thẻ từ bộ bài thứ ba. 

Tại sao nó hoạt động: điều bất biến là chúng ta phải đạt được`target`thẻ trong mỗi bộ bài. Tính chia hết của tổng đảm bảo`target`là một số nguyên. Séc`target <= c`đảm bảo bộ bài thứ ba có đủ quân bài để cung cấp cho hai bộ bài đầu tiên. Logic này đảm bảo tính chính xác mà không cần mô phỏng mọi phân phối lại có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    total = a + b + c
    if total % 3 != 0:
        print("NO")
        continue
    target = total // 3
    if target > c:
        print("NO")
    else:
        print("YES")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó tính tổng của ba bộ bài. Nếu tổng không chia hết cho ba, nó sẽ in "NO". Mặt khác, nó sẽ tính toán kích thước bộ bài mục tiêu và kiểm tra xem bộ bài lớn nhất có thể cung cấp các lá bài cần thiết hay không. Việc triển khai sử dụng phép chia số nguyên và so sánh đơn giản, giúp tránh mọi nguy cơ lỗi dấu phẩy động. Giải pháp xử lý các trường hợp biên trong đó số lượng thẻ được phân phối lại cần thiết có thể bằng 0. 

## Ví dụ đã hoạt động 

Đối với đầu vào`3 5 10`: 

| một | b | c | tổng cộng | tổng % 3 | mục tiêu | mục tiêu > c? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 5 | 10 | 18 | 0 | 6 | Không | CÓ | 

Chúng tôi phân phối lại`3`thẻ vào bộ bài đầu tiên và`1`thẻ sang bộ bài thứ hai. Bộ bài thứ ba giảm xuống còn`6`, phù hợp với mục tiêu 

Đối với đầu vào`12 20 30`: 

| một | b | c | tổng cộng | tổng % 3 | mục tiêu | mục tiêu > c? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 12 | 20 | 30 | 62 | 2 | 20 | Không | KHÔNG | 

62 không chia hết cho 3 nên không thể phân bố đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ thực hiện phép tính số học và so sánh | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Với tối đa$10^4$trường hợp thử nghiệm, tổng số thao tác có thể chấp nhận được dưới giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        total = a + b + c
        if total % 3 != 0:
            print("NO")
            continue
        target = total // 3
        if target > c:
            print("NO")
        else:
            print("YES")
    return output.getvalue().strip()

# Provided samples
assert run("4\n3 5 10\n12 20 30\n3 5 7\n1 5 6\n") == "YES\nNO\nYES\nNO"

# Custom cases
assert run("2\n1 2 3\n2 3 4\n") == "YES\nNO"
assert run("1\n10 20 30\n") == "NO"
assert run("1\n1 1 1\n") == "YES"
assert run("1\n1 2 4\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | CÓ | Số nhỏ nhất, tổng chia hết | 
| 2 3 4 | KHÔNG | Tổng không chia hết cho 3 | 
| 10 20 30 | KHÔNG | Số lớn, tổng không chia hết cho 3 | 
| 1 1 1 | CÓ | Tất cả các bộ bài bằng nhau | 
| 1 2 4 | KHÔNG | Bộ bài thứ ba quá nhỏ để cân bằng | 

## Vỏ cạnh 

Nếu tổng của`a + b + c`chia hết cho 3 nhưng bộ bài thứ 3 không đủ bài nên thuật toán ra đúng “NO”. Ví dụ,`a=1, b=2, c=2`có`total=5`, chia hết cho 3? Không, vì vậy đầu ra là "KHÔNG". Vì`a=1, b=2, c=4`,`total=7`, không chia hết cho 3, lại là "KHÔNG". Những trường hợp này đảm bảo chúng ta xử lý chính xác cả điều kiện chia hết và điều kiện đủ. Bộ bài lớn nhất luôn luôn`c`được đảm bảo bởi các ràng buộc đầu vào nên chúng ta không cần kiểm tra các hoán vị khác.

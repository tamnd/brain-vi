---
title: "CF 1353B - Hai mảng và hoán đổi"
description: "Bài toán đưa ra hai mảng có độ dài bằng nhau, chúng ta có thể coi chúng là hai bộ số nằm trên các giá riêng biệt. Chúng ta được phép hoán đổi số giữa các kệ nhưng chỉ tối đa k lần hoán đổi."
date: "2026-06-11T14:08:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1353
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 642 (Div. 3)"
rating: 800
weight: 1353
solve_time_s: 554
verified: true
draft: false
---

[CF 1353B - Hai mảng và hoán đổi](https://codeforces.com/problemset/problem/1353/B) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 9 phút 14 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra hai mảng có độ dài bằng nhau, chúng ta có thể coi chúng là hai bộ số nằm trên các giá riêng biệt. Chúng ta được phép hoán đổi số giữa các kệ nhưng chỉ tối đa là`k`trao đổi. Mục tiêu của chúng tôi là làm cho tổng giá trị đầu tiên càng lớn càng tốt sau khi thực hiện các giao dịch hoán đổi này. Mỗi trường hợp thử nghiệm cung cấp độ dài của mảng, số lần hoán đổi tối đa được phép và bản thân các mảng. Đầu ra đơn giản là tổng lớn nhất của mảng đầu tiên có thể đạt được theo ràng buộc hoán đổi. 

Các hạn chế là nhỏ:`n`có thể nhiều nhất là 30 và`k`nhiều nhất`n`, trong khi số lượng ca kiểm thử`t`lên tới 200. Điều này có nghĩa là chúng ta không cần phải lo lắng về việc mở rộng tới hàng triệu phép tính, do đó, bất kỳ giải pháp bậc hai hoặc sắp xếp mảng nào cũng sẽ đủ hiệu quả. Mỗi số cũng nhỏ, tối đa 30, vì vậy việc tràn số nguyên không phải là vấn đề đáng lo ngại. Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó số lần hoán đổi tối ưu bằng 0, chẳng hạn khi tất cả các phần tử của mảng đầu tiên đã lớn hơn tất cả các phần tử của mảng thứ hai. Một trường hợp phức tạp khác là khi`k`vượt quá số lượng giao dịch hoán đổi có lợi; thực hiện các giao dịch hoán đổi không cần thiết có thể làm giảm số tiền nếu thực hiện bất cẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử tất cả các chuỗi hoán đổi có thể có. Với mỗi nước đi chúng ta sẽ chọn một cặp`(i, j)`và trao đổi, khám phá đệ quy bước đi tiếp theo cho đến khi chúng ta đạt được`k`khả năng hoán đổi hoặc xả thải. Điều này đúng vì nó liệt kê tất cả các kết quả có thể xảy ra, nhưng số lượng các chuỗi tăng lên cực kỳ nhanh - lên tới`(n^2)^k`- quá chậm ngay cả đối với`n = 30`Và`k = 30`. 

Điều quan trọng cần lưu ý là chúng ta muốn tăng tổng của mảng đầu tiên. Để thực hiện điều này một cách tối ưu, chúng ta nên hoán đổi các phần tử nhỏ nhất của nó với các phần tử lớn nhất của mảng thứ hai, nhưng chỉ khi phần tử từ mảng thứ hai lớn hơn. Trao đổi bất cứ điều gì khác sẽ không giúp ích. Việc sắp xếp mảng đầu tiên theo thứ tự tăng dần và mảng thứ hai theo thứ tự giảm dần cho phép chúng ta ghép các ứng cử viên tốt nhất để hoán đổi. Sau đó, chúng tôi lặp lại lần đầu tiên`k`các phần tử, chỉ thực hiện hoán đổi nếu phần tử từ mảng thứ hai lớn hơn phần tử trong mảng thứ nhất. Cách tiếp cận tham lam này đảm bảo rằng mọi hoán đổi chúng tôi thực hiện đều đóng góp tích cực vào tổng của mảng đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n^2)^k) | O(n) | Quá chậm | 
| Tối ưu (Tham lam với việc sắp xếp) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`k`, rồi đọc mảng`a`Và`b`. 
3. Sắp xếp`a`theo thứ tự tăng dần để xác định các phần tử nhỏ nhất. 
4. Sắp xếp`b`theo thứ tự giảm dần để xác định các phần tử lớn nhất. 
5. Lặp lại lần đầu tiên`k`các yếu tố, xem xét`a[i]`Và`b[i]`. Nếu như`b[i]`lớn hơn`a[i]`, trao đổi chúng. Nếu không, hãy phá vỡ vòng lặp vì không có giao dịch hoán đổi nào nữa có thể cải thiện tổng số. 
6. Tính tổng mảng đã sửa đổi`a`. 
7. Xuất tổng này cho từng test case. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng chúng ta luôn so sánh phần tử nhỏ nhất có sẵn trong`a`với phần tử lớn nhất có sẵn trong`b`, đó chính xác là sự hoán đổi giúp tối đa hóa mức tăng ngay lập tức trong`a`tổng của. Quyết định tham lam dừng việc hoán đổi khi`b[i] <= a[i]`hợp lệ vì không có phần tử nào sau này của`b`sẽ lớn hơn các phần tử trước đó do thứ tự giảm dần và không có phần tử nào sau đó của`a`sẽ nhỏ hơn các phần tử trước đó do thứ tự tăng dần. Điều này bảo tồn sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort(reverse=True)
        
        for i in range(min(k, n)):
            if b[i] > a[i]:
                a[i], b[i] = b[i], a[i]
            else:
                break
        
        print(sum(a))

solve()
```Đầu tiên, mã sẽ sắp xếp hai mảng để thực hiện việc hoán đổi tham lam một cách đơn giản. Vòng lặp chạy tối đa`k`lặp đi lặp lại và chỉ thực hiện hoán đổi nếu nó cải thiện nghiêm ngặt tổng của`a`. Việc sử dụng`min(k, n)`đảm bảo chúng tôi không truy cập các chỉ mục ngoài phạm vi, đây là một điểm tinh tế có thể bị bỏ qua. Tính toán`sum(a)`sau khi hoán đổi sẽ đưa ra câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:```
2 1
1 2
3 4
```| tôi | một (sắp xếp) | b (được sắp xếp desc) | tráo đổi? | sau khi trao đổi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | vâng | 4,2 | 

Chúng ta đổi chỗ 1 bằng 4. Tổng của`a`là 6. 

Một ví dụ khác:```
5 3
1 2 3 4 5
10 9 10 10 9
```Sắp xếp mang lại`a = [1,2,3,4,5]`Và`b = [10,10,10,9,9]`. 

| tôi | một [tôi] | b[i] | tráo đổi? | sau khi trao đổi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 10 | vâng | 10,2,3,4,5 | 
| 1 | 2 | 10 | vâng | 10,10,3,4,5 | 
| 2 | 3 | 10 | vâng | 10,10,10,4,5 | 

Sau 3 lần hoán đổi, tổng số`a`là 10+10+10+4+5 = 39. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Việc sắp xếp cả hai mảng chiếm ưu thế trong tính toán; vòng lặp của k là O(n) và không đáng kể. | 
| Không gian | O(n) | Mảng`a`Và`b`được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Giải pháp phù hợp thoải mái trong các ràng buộc, như`n`nhiều nhất là 30 và`t`nhiều nhất là 200, do đó việc sắp xếp theo từng trường hợp thử nghiệm là không đáng kể về mặt thời gian chạy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n2 1\n1 2\n3 4\n5 5\n5 5 6 6 5\n1 2 5 4 3\n5 3\n1 2 3 4 5\n10 9 10 10 9\n4 0\n2 2 4 3\n2 4 2 3\n4 4\n1 2 2 1\n4 4 5 4") == "6\n27\n39\n11\n17", "provided samples"

# Custom test cases
assert run("1\n3 2\n1 2 3\n3 2 1") == "8", "swaps limited by k"
assert run("1\n3 3\n5 5 5\n1 1 1") == "15", "no swaps needed"
assert run("1\n4 4\n1 2 3 4\n4 3 2 1") == "14", "max swaps used"
assert run("1\n1 1\n1\n100") == "100", "single element swap"
assert run("1\n2 0\n1 2\n3 4") == "3", "zero swaps allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 / 1 2 3 / 3 2 1 | 8 | Chỉ các giao dịch hoán đổi có lợi mới được thực hiện, bị giới hạn bởi k | 
| 3 3 / 5 5 5 / 1 1 1 | 15 | Không cần trao đổi vì`a`đã tối ưu | 
| 4 4 / 1 2 3 4 / 4 3 2 1 | 14 | Tất cả các giao dịch hoán đổi được sử dụng chính xác để tối đa hóa tổng | 
| 1 1/1/100 | 100 | Kịch bản hoán đổi phần tử đơn | 
| 2 0 / 1 2 / 3 4 | 3 | Không cho phép hoán đổi, thuật toán tôn trọng k | 

## Vỏ cạnh 

Đối với trường hợp hoán đổi không làm tăng tổng, chẳng hạn như:```
2 2
5 6
1 2
```Thuật toán sắp xếp`a`tăng dần`[5,6]`Và`b`giảm dần`[2,1]`. Khi lặp lại`i=0`,`b[0]=2`không lớn hơn`a[0]=5`, do đó vòng lặp sẽ bị ngắt ngay lập tức. Không có giao dịch hoán đổi nào xảy ra và tổng vẫn bằng 11. Điều này xác nhận thuật toán xử lý chính xác các tình huống trong đó việc hoán đổi sẽ phản tác dụng.

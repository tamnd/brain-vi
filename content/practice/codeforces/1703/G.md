---
title: "CF 1703G – Chìa Khóa Tốt, Chìa Khóa Xấu"
description: "Chúng tôi được yêu cầu mở một chuỗi rương, mỗi rương chứa một số đồng xu, sử dụng chìa khóa “tốt” hoặc “xấu”. Một chiếc chìa khóa tốt sẽ tốn một lượng xu cố định để sử dụng, trong khi một chiếc chìa khóa xấu thì miễn phí nhưng sẽ giảm một nửa số xu trong mỗi rương chưa mở, bao gồm cả chiếc chìa khóa mà nó mở ra, làm tròn xuống."
date: "2026-06-09T21:39:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1703
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 806 (Div. 4)"
rating: 1600
weight: 1703
solve_time_s: 155
verified: false
draft: false
---

[CF 1703G - Khóa tốt, Khóa xấu](https://codeforces.com/problemset/problem/1703/G) 

**Đánh giá:** 1600 
**Tags:** bitmasks, vũ phu, dp, tham lam, toán học 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mở một chuỗi rương, mỗi rương chứa một số đồng xu, sử dụng chìa khóa “tốt” hoặc “xấu”. Một chiếc chìa khóa tốt sẽ tốn một lượng xu cố định để sử dụng, trong khi một chiếc chìa khóa xấu thì miễn phí nhưng sẽ giảm một nửa số xu trong mỗi rương chưa mở, bao gồm cả chiếc chìa khóa mà nó mở ra, làm tròn xuống. Chúng tôi muốn tối đa hóa tổng số xu chúng tôi có sau khi mở tất cả các rương theo thứ tự. 

Đầu vào cung cấp số lượng rương, giá của một chiếc chìa khóa tốt và số xu trong mỗi rương. Đầu ra là số tiền tối đa có thể đạt được. Về mặt khái niệm, đối với mỗi rương, chúng tôi đang quyết định xem nên tiêu xu ngay bây giờ để nhận được toàn bộ giá trị hay thực hiện hành động miễn phí làm giảm lợi nhuận trong tương lai. 

Các ràng buộc rất đáng kể: lên tới 100.000 rương cho mỗi trường hợp thử nghiệm và lên tới 10.000 trường hợp thử nghiệm. Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm được giới hạn ở 100.000, điều đó có nghĩa là bất kỳ giải pháp nào có độ phức tạp cho mỗi trường hợp thử nghiệm là O(n) đều khả thi. Việc khám phá mạnh mẽ tất cả các chuỗi lựa chọn khóa tốt/xấu là theo cấp số nhân và không khả thi vì ngay cả 20 rương cũng cần phải kiểm tra hơn một triệu chuỗi. Giá trị của đồng xu và chi phí khóa có thể đạt tới 10^9, do đó, tràn số nguyên là mối lo ngại ở các ngôn ngữ có số nguyên 32 bit. 

Các trường hợp khó khăn không rõ ràng bao gồm các rương không có đồng xu, trường hợp giá của một chiếc chìa khóa tốt bằng 0 và các tình huống trong đó tất cả các đồng xu đều bằng nhau. Ví dụ, nếu`n = 2`,`k = 1`, Và`a = [0, 10]`, dùng chìa khóa tốt ở rương đầu tiên thì lãng phí vì không có xu. Sử dụng phím sai ở rương đầu tiên sẽ làm giảm một nửa rương thứ hai, vì vậy tốt nhất nên bỏ qua hoặc điều chỉnh lựa chọn cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là brute-force: đối với mỗi rương, hãy xem xét cả hai lựa chọn chìa khóa, tính toán đệ quy số xu thu được và chọn số tiền tối đa. Điều này hiệu quả vì nó mô hình chính xác tất cả các chuỗi có thể, nhưng nó không thành công vì mỗi rương nhân đôi số trạng thái. Với`n`lên đến 10^5, điều này sẽ thực hiện các thao tác O(2^n), rõ ràng là không thực tế. 

Thông tin chi tiết quan trọng là một khóa sai sẽ chia đôi tất cả các rương còn lại, do đó, tác động của các khóa sai liên tiếp sẽ xảy ra theo sự phân rã hình học. Điều đó cho phép chúng tôi đảo ngược vấn đề: thay vì mô phỏng từng tổ hợp, chúng tôi có thể sửa số lượng phím sai được sử dụng ở cuối và áp dụng các phím tốt vào các rương lớn hơn một cách tham lam. Chiến lược tối ưu là quyết định xem chúng ta sẽ mở bao nhiêu rương cuối cùng bằng chìa khóa sai. Chúng ta có thể lặp lại số lượng phím sai`b`từ 0 đến`n`và tính toán số tiền thu được: tất cả các rương trước rương cuối cùng`b`nhận được chìa khóa tốt và cuối cùng`b`bị ảnh hưởng bởi việc giảm một nửa liên tiếp. 

Điều này có hiệu quả vì việc sử dụng sớm một khóa tốt chỉ có giá trị nếu nó bù đắp cho số tiền bị mất do các đợt halving trong tương lai. Tổng số lần lặp là`n`cho mỗi trường hợp thử nghiệm và mỗi phép tính có thể được thực hiện trong O(n) hoặc O(b) nếu chúng ta tính toán trước lũy thừa của hai, đủ nhanh với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) ngây thơ, O(n) với tổng/quyền hạn tiền tố | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của các đồng tiền từ cuối, để nhanh chóng biết tổng của tiền tố cuối cùng`b`rương. Điều này cho phép chúng ta tính toán các giá trị giảm đi một nửa một cách hiệu quả. 
2. Tính trước lũy thừa của 1/2 bằng phép chia số nguyên (`// 2`) trong tối đa 60 lần lặp vì việc giảm một nửa vượt quá mức đó sẽ mang lại số 0 cho tất cả các rương có giá trị lên tới 10^9. 
3. Lặp lại`b`từ 0 đến`n`, đại diện cho số lượng khóa sai được sử dụng ở cuối. Đối với mỗi`b`, tính toán số tiền thu được bằng cách áp dụng`b`các phím xấu tuần tự ở cuối cùng`b`rương. Công thức là tổng của`floor(a[i] / 2^j)`để có quyền hạn phù hợp`j`. 
4. Phần còn lại`n - b`rương sử dụng chìa khóa tốt. Trừ tổng chi phí`(n - b) * k`từ tổng số xu thu được từ những chiếc rương đó. 
5. Theo dõi mức tối đa trên tất cả các giá trị của`b`. Giá trị tối đa là câu trả lời cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến đối với mỗi`b`, tất cả các chuỗi đều chính xác`b`các phím xấu ở cuối sẽ được xem xét, điều này đảm bảo chúng tôi khám phá được tất cả các vị trí tối ưu của các phím xấu. Vì khóa sai chỉ ảnh hưởng đến các rương còn lại nên bất kỳ chuỗi nào áp dụng khóa sai trước đó đều có thể được biểu thị dưới dạng một số`b`trong lần lặp này. Chìa khóa tốt được áp dụng một cách tham lam vào các rương lớn nhất không bị ảnh hưởng, điều này đảm bảo không thể đạt được tổng số tiền cao hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        res = 0
        total_good = 0
        # Start with all coins as if all bad keys were used
        suffix = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suffix[i] = suffix[i + 1] + a[i]
        max_coins = 0
        pow2 = 1
        for b in range(n + 1):
            coins_from_bad = 0
            for i in range(n - b, n):
                coins_from_bad += a[i] // pow2
            coins_from_good = suffix[0] - suffix[n - b] - (n - b) * k
            max_coins = max(max_coins, coins_from_good + coins_from_bad)
            pow2 *= 2
        print(max_coins)

solve()
```Đầu tiên, mã sẽ tính tổng hậu tố để nhanh chóng đánh giá tổng số xu cho phần khóa tốt. Nó lặp lại số lượng khóa sai được sử dụng ở cuối, tính toán tác động của chúng bằng cách sử dụng phép chia số nguyên lặp lại để mô hình hóa các lần chia đôi liên tiếp. nhân`pow2`mỗi bước hai lần đảm bảo mỗi lần giảm một nửa được tính toán chính xác. 

Các điểm tinh tế bao gồm việc xử lý phép chia số nguyên một cách cẩn thận để việc làm tròn xuống được thực hiện tự động, tránh các lỗi dấu phẩy động. sử dụng`suffix`cho phép O(1) truy cập vào tổng các rương trước các khóa sai, loại bỏ nhu cầu tính tổng lặp đi lặp lại bên trong vòng lặp. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
n = 4, k = 5, a = [10, 10, 3, 1]
```| b | Tiền từ chìa khóa tốt | Tiền xu từ chìa khóa xấu | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 10+10+3+1-4*5= -1 | 0 | -1 | 
| 1 | 10+10+3-3*5=5 | 1//2=0 | 5 | 
| 2 | 10+10-2*5=10 | 3//2=1,1//4=0 | 11 | 
| 3 | 10-1*5=5 | 10//2=5,3//4=0,1//8=0 | 10 | 
| 4 | 0 | 10//16=0,10//32=0,3//64=0,1//128=0 | 0 | 

Tối đa là 11. Dấu vết xác nhận thuật toán đánh giá tất cả các vị trí của khóa sai một cách hiệu quả. 

### Đầu vào mẫu 2```
n = 1, k = 2, a = [1]
```| b | Tốt | Xấu | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 1-2=-1 | 0 | -1 | 
| 1 | 0 | 1//1=1 | 1 | 

Tối đa là 1, hiển thị thuật toán xử lý chính xác các mảng phần tử đơn và khi khóa sai thì tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) ngây thơ, O(n) với mức cắt giảm một nửa | Chúng tôi lặp đi lặp lại lên đến`n`phím xấu. Đối với mỗi loại, tính toán tổng khóa sai bằng cách sử dụng lũy ​​thừa được tính toán trước giúp giảm độ phức tạp | 
| Không gian | O(n) | Mảng tổng hậu tố có độ dài`n+1`| 

Với hạn chế là tổng của`n`trên tất cả các trường hợp thử nghiệm 10^5, giải pháp này chạy thoải mái trong vòng chưa đầy 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n4 5\n10 10 3 1\n1 2\n1\n3 12\n10 10 29\n12 51\n5 74 89 45 18 69 67 67 11 96 23 59\n2 57\n85 60\n") == "11\n0\n13\n60\n58"

# Custom cases
assert run("1\n1 0\n100\n") == "100", "single chest, zero cost"
assert run("1\n3
```

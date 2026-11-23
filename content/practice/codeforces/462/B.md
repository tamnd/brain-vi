---
title: "CF 462B - Appleman và trò chơi bài"
description: "Appleman có một bộ sưu tập n thẻ, mỗi thẻ được dán nhãn bằng một chữ cái in hoa. Toastman được phép chọn chính xác k thẻ và với mỗi thẻ anh ta chọn, Appleman trả cho anh ta số xu bằng số thẻ của Toastman có cùng chữ cái với thẻ đó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 462
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 263 (Div. 2)"
rating: 1300
weight: 462
solve_time_s: 69
verified: true
draft: false
---

[CF 462B - Appleman và trò chơi bài](https://codeforces.com/problemset/problem/462/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Appleman có một bộ sưu tập _n_ thẻ, mỗi thẻ được dán nhãn bằng chữ in hoa. Toastman được phép chọn chính xác _k_ thẻ và với mỗi thẻ anh ta chọn, Appleman trả cho anh ta số xu bằng số thẻ của Toastman có cùng chữ cái với thẻ đó. Nói cách khác, nếu Toastman chọn một số thẻ có cùng một chữ cái, thì mỗi thẻ đó sẽ đóng góp số lượng chữ cái đó trên tay anh ta vào tổng số tiền. 

Dữ liệu đầu vào cung cấp số lượng thẻ _n_, số lượng thẻ cần chọn _k_ và một chuỗi có độ dài _n_ trong đó mỗi ký tự đại diện cho một chữ cái của thẻ. Đầu ra phải là một số nguyên duy nhất: số xu tối đa mà Toastman có thể kiếm được nếu anh ta chọn các lá bài một cách tối ưu. 

Các giới hạn là đáng kể. Với _n_ lên tới 100.000 và giới hạn thời gian một giây, bất kỳ thuật toán nào kém hơn O(n log n) đều có nguy cơ hết thời gian chờ. Một cách tiếp cận đơn giản để kiểm tra tất cả các tập con có thể có kích thước _k_ là hoàn toàn không khả thi vì số tập con tăng lên một cách tổ hợp, theo thứ tự n chọn k. 

Trường hợp cạnh không rõ ràng xảy ra khi nhiều chữ cái xuất hiện nhiều lần nhưng ít hơn _k_ riêng lẻ. Ví dụ, nếu các thẻ`AABBC`và _k_ là 4, một cách tiếp cận đơn giản là chỉ cần lấy các thẻ _k_ đầu tiên theo thứ tự có thể chọn`AABB`hoặc`ABBC`. Tối ưu là`AABB`bởi vì việc chọn hai chữ A và hai chữ B sẽ tối đa hóa các chữ cái lặp lại và do đó là các đồng xu. Một trường hợp đặc biệt khác là khi _k_ vượt quá số lượng chữ cái thường xuyên nhất; thuật toán sau đó phải bổ sung các chữ cái thường xuyên nhất tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là kiểm tra tất cả các kết hợp thẻ _k_, tính tổng số xu cho mỗi kết hợp và giữ mức tối đa. Về nguyên tắc, điều này đúng vì nó kiểm tra mọi khả năng, nhưng số lượng kết hợp rất lớn về mặt thiên văn đối với _n_ = 100.000 và thậm chí là _k_ vừa phải, nên không thể sử dụng được. 

Quan sát quan trọng là mức tăng xu là bậc hai về số lần một chữ cái xuất hiện trong số các thẻ đã chọn. Nếu Toastman chọn _x_ thẻ có cùng chữ cái, chúng sẽ đóng góp _x²_ vào tổng số xu. Do đó, để tối đa hóa số tiền, trước tiên anh ta nên chọn càng nhiều thẻ càng tốt từ chữ cái xuất hiện thường xuyên nhất, sau đó là chữ cái xuất hiện thường xuyên nhất tiếp theo, v.v., cho đến khi anh ta chọn được tổng cộng _k_ thẻ. Nếu _k_ còn lại nhỏ hơn số lượng của chữ cái tiếp theo, anh ta chỉ cần lấy số thẻ đó, đóng góp vào bình phương của số đã chọn. 

Đây là một chiến lược tham lam cổ điển: luôn chọn các chữ cái có tần số cao nhất trước tiên vì chúng tối đa hóa hệ số bậc hai. Sắp xếp số đếm theo thứ tự giảm dần đảm bảo chúng ta luôn chọn được các chữ cái tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn k) | O(n) | Quá chậm | 
| Tối ưu | O(n + 26 log 26) ≈ O(n) | O(26) ≈ O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi chữ cái trong chuỗi thẻ. Sử dụng từ điển hoặc mảng 26 phần tử cho chữ in hoa. Điều này cho biết tần số của mỗi chữ cái. 
2. Sắp xếp tần số theo thứ tự giảm dần. Điều này đảm bảo chúng tôi luôn chọn những chữ cái đóng góp nhiều tiền nhất trước tiên. 
3. Khởi tạo một biến`coins = 0`Và`remaining = k`. Lặp lại các tần số được sắp xếp. 
4. Đối với mỗi tần số`f`, nếu như`f <= remaining`, thêm vào`f * f`ĐẾN`coins`và trừ`f`từ`remaining`. 
5. Nếu`f > remaining`, chỉ lấy`remaining`thẻ của chữ cái đó, thêm`remaining * remaining`ĐẾN`coins`, và đặt`remaining = 0`. Phá vỡ vòng lặp vì chúng ta đã chọn chính xác thẻ _k_. 
6. In`coins`. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, chúng tôi đã chọn tập hợp các chữ cái giúp tối đa hóa tổng bình phương cho các lựa chọn _k_ còn lại. Bình phương một số lớn hơn luôn mang lại số gia tăng lớn hơn so với việc chia nó thành các nhóm nhỏ hơn. Do đó, không có sự phân phối nào khác của cùng một thẻ _k_ có thể mang lại nhiều tiền hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
cards = input().strip()

# Step 1: Count letter frequencies
freq = [0] * 26
for c in cards:
    freq[ord(c) - ord('A')] += 1

# Step 2: Sort frequencies descending
freq.sort(reverse=True)

coins = 0
remaining = k

# Step 3: Greedy selection
for f in freq:
    if remaining == 0:
        break
    take = min(f, remaining)
    coins += take * take
    remaining -= take

print(coins)
```Phần đầu tiên đếm các chữ cái sử dụng chỉ số ASCII để truy cập liên tục. Việc sắp xếp đảm bảo sự lựa chọn tham lam sẽ chọn nhóm lớn nhất có sẵn trước tiên. Trong vòng lặp,`take = min(f, remaining)`xử lý trường hợp các khe còn lại ít hơn tần số, điều này dễ bị bỏ qua và là nguồn phổ biến gây ra các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
15 10
DZFDFZDFDDDDDDF
```| Bước | sắp xếp tần số | còn lại k | tiền tích lũy | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [9,3,2,1,0...] | 10 | 0 | lấy 9 của 'D' → xu += 81 | 
| 2 | 3 | 1 | 81 | lấy 1 của 'F' → xu += 1 → tổng 82 | 
| 3 | ... | 0 | 82 | xong | 

Điều này xác nhận rằng việc chọn nhóm lớn nhất trước tiên sẽ tối đa hóa số tiền. 

Mẫu 2: 

đầu vào:```
5 3
AABBC
```| Bước | sắp xếp tần số | còn lại k | tiền tích lũy | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [2,2,1,...] | 3 | 0 | lấy 2 của 'A' → xu += 4 → còn lại=1 | 
| 2 | 2 | 1 | 4 | lấy 1 của 'B' → xu += 1 → tổng 5 | 

Thuật toán xử lý chính xác trường hợp tần số còn lại <. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26 log 26) ≈ O(n) | Đếm chữ cái là O(n), sắp xếp 26 tần số không đổi | 
| Không gian | O(26) ≈ O(1) | Chỉ sử dụng mảng tần số gồm 26 chữ cái | 

Với n lên tới 100.000, độ phức tạp O(n) có thể chấp nhận được và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    cards = input().strip()
    freq = [0] * 26
    for c in cards:
        freq[ord(c) - ord('A')] += 1
    freq.sort(reverse=True)
    coins = 0
    remaining = k
    for f in freq:
        if remaining == 0:
            break
        take = min(f, remaining)
        coins += take * take
        remaining -= take
    return str(coins)

# provided sample
assert run("15 10\nDZFDFZDFDDDDDDF\n") == "82"

# custom cases
assert run("5 3\nAABBC\n") == "5", "mixed letters, k less than max count"
assert run("3 3\nAAA\n") == "9", "all letters same"
assert run("4 2\nABCD\n") == "1", "all different, pick 2"
assert run("1 1\nZ\n") == "1", "single card"
assert run("100000 100000\n" + "A"*100000 + "\n") == "10000000000", "max size all same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3, AABBC | 5 | lựa chọn tham lam với tần số một phần | 
| 3 3, AAA | 9 | tất cả các thẻ đều giống nhau | 
| 4 2, ABCD | 1 | tất cả các chữ cái khác nhau, k < n | 
| 1 1, Z | 1 | đầu vào tối thiểu | 
| 100000 100000, tất cả A | 10000000000 | kích thước đầu vào tối đa | 

## Vỏ cạnh 

Khi _k_ vượt quá tần số của chữ cái phổ biến nhất, thuật toán sẽ tiếp tục chính xác đến tần số cao nhất tiếp theo. Ví dụ, với đầu vào`AABBC`và *k=4`, chữ cái thường gặp nhất 'A' có tần số 2. Thuật toán lấy cả As, còn lại 2. Tiếp theo, 'B' có tần số 2 và vì còn lại = 2 nên cần có cả B. Tổng số xu = 222 + 22 = 8. Một cách tiếp cận ngây thơ luôn lấy k chữ cái đầu tiên theo thứ tự có thể tính tổng không chính xác theo cách khác. Bất biến tham lam đảm bảo ở mỗi bước rằng đóng góp lớn nhất có thể được chọn, đảm bảo tính chính xác.

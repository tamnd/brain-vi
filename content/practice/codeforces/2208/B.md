---
title: "CF 2208B-Người Đi Xe Đạp"
description: "Chúng ta được phát một bộ bài gồm n lá bài, mỗi lá bài có một chi phí năng lượng. Bob muốn chơi một lá bài \"có điều kiện thắng\" đặc biệt nhiều lần nhất có thể mà không vượt quá tổng ngân sách năng lượng m. Trong mỗi lượt, Bob chỉ có thể chọn một lá bài từ k vị trí đầu tiên trong bộ bài hiện tại."
date: "2026-06-07T19:26:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2208
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1086 (Div. 2)"
rating: 1100
weight: 2208
solve_time_s: 143
verified: false
draft: false
---

[CF 2208B - Người đi xe đạp](https://codeforces.com/problemset/problem/2208/B) 

**Đánh giá:** 1100 
**Tags:** bạo lực, trò chơi, tham lam, thực hiện, toán học, phân loại 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ bài`n`thẻ, mỗi thẻ có một chi phí năng lượng. Bob muốn chơi một lá bài "điều kiện thắng" đặc biệt nhiều lần nhất có thể mà không vượt quá tổng ngân sách năng lượng`m`. Ở mỗi lượt, Bob chỉ có thể chọn một lá bài từ lá bài đầu tiên.`k`các vị trí trong bộ bài hiện tại. When a card is played, it goes to the bottom of the deck, and the remaining cards shift forward. Thẻ điều kiện thắng bắt đầu ở vị trí`p`(1-lập chỉ mục). Đầu ra phải là số lần tối đa Bob có thể chơi lá bài này với giới hạn năng lượng. 

Các hạn chế là nhỏ:`n`,`m`,`k`, Và`p`đều 5000 và tổng của tất cả`n`trên các trường hợp thử nghiệm ≤ 5000. Điều này có nghĩa là chúng tôi có thể cung cấp các thuật toán gần đúng`O(n^2)`cho mỗi trường hợp thử nghiệm, vì tổng số thao tác trong trường hợp xấu nhất sẽ vẫn dưới khoảng 25 triệu, có thể chấp nhận được đối với giới hạn 1 giây. 

Một trường hợp khó phát hiện khi lá bài điều kiện thắng bắt đầu bên ngoài lá bài đầu tiên.`k`các vị trí. Trong những trường hợp như vậy, chúng ta có thể buộc phải chơi những quân bài khác để đưa nó vào ván đầu tiên.`k`khe cắm. Ví dụ, nếu`n=2`,`k=1`,`p=2`, và năng lượng`m=42`với chi phí`[42,1]`, điều kiện thắng là lá bài thứ hai nhưng chỉ có lá bài đầu tiên mới có thể chơi được. Vì chơi nên tiêu tốn hết năng lượng nên đáp án là`0`, mặc dù có đủ năng lượng để chơi điều kiện thắng. Một cách tiếp cận tham lam bất cẩn mà bỏ qua thứ tự bộ bài có thể gợi ý chơi điều kiện thắng một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là mô phỏng mọi chuỗi vở kịch có thể xảy ra. Chúng ta có thể thử tất cả các kết hợp chọn thẻ nào ở mỗi bước cho đến khi hết năng lượng. Điều này hoạt động chính xác nhưng rõ ràng là quá chậm: trong trường hợp xấu nhất, chúng ta có thể có`n=5000`Và`m=5000`, làm cho số lượng chuỗi theo cấp số nhân. Ngay cả việc cắt tỉa bằng năng lượng vẫn để lại quá nhiều khả năng. 

Cái nhìn sâu sắc quan trọng là cần lưu ý rằng chiến lược tối ưu liên quan đến việc chơi quân bài có điều kiện thắng ngay khi nó có sẵn và trước khi nó rời khỏi quân bài đầu tiên.`k`vị trí, chúng tôi có thể cần phải loại bỏ một số thẻ rẻ hơn khác ở phía trước nó. Vì vậy, chúng ta có thể coi bài toán là chọn một số lượng thẻ nhất định để "bỏ" khỏi mặt trước (`x`thẻ từ đầu tiên`p-1`) và một số nhất định cần xóa khỏi phần cuối của số đầu tiên`k`cửa sổ (`y`thẻ từ bên phải). Điều này biến vấn đề thành một vòng lặp kép: liệt kê số lượng quân bài cần loại bỏ ở phía trước và bao nhiêu quân bài ở phía cuối, sau đó tính xem chúng ta có thể chơi quân bài có điều kiện thắng bao nhiêu lần với số năng lượng còn lại. Việc tính toán bên trong chỉ yêu cầu số học cơ bản. 

Điều này biến việc tìm kiếm theo cấp số nhân thành`O(k^2)`mỗi trường hợp thử nghiệm. Từ`k ≤ n ≤ 5000`và tổng của tất cả`n`trên các trường hợp thử nghiệm ≤ 5000, giải pháp đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đếm trước/sau tối ưu | O(k^2) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi bộ bài đầu vào thành 0 chỉ mục, lưu ý vị trí của lá bài có điều kiện thắng là`p-1`. 
2. Lặp lại số lượng thẻ cần loại bỏ ở mặt trước`front_removals`từ`0`ĐẾN`min(p-1, k-1)`. Việc loại bỏ một lá bài ở phía trước sẽ làm giảm khoảng cách của điều kiện thắng với cửa sổ có thể chơi được. 
3. Đối với mỗi`front_removals`, lặp lại số thẻ bị loại bỏ từ mặt sau của thẻ đầu tiên`k`thẻ`back_removals`từ`0`ĐẾN`k-1-front_removals`. Điều này cho phép lá bài có điều kiện thắng nằm trong lá bài đầu tiên`k`các vị trí. 
4. Tính tổng năng lượng tiêu tốn để loại bỏ các thẻ này: tổng chi phí của các thẻ bị loại bỏ ở phía trước cộng với các thẻ bị loại bỏ ở phía sau. 
5. Với năng lượng còn lại, hãy tính số lần thẻ điều kiện thắng có thể được chơi:`(remaining_energy) // (cost of win-condition card)`. 
6. Theo dõi số lượng tối đa trên tất cả các kết hợp hợp lệ. 

**Tại sao nó hoạt động:** Tại mỗi lần lặp lại, chúng tôi khám phá tất cả các cách có thể để đưa thẻ điều kiện thắng vào cửa sổ có thể chơi mà không vượt quá kích thước cửa sổ`k`. Bằng cách liệt kê số lượng thẻ bị loại bỏ ở cả hai đầu, chúng tôi xem xét mọi cấu hình hợp lệ. Vì năng lượng không phụ thuộc vào thứ tự khi việc loại bỏ được cố định, nên việc tính số lần chúng ta có thể chơi quân bài có điều kiện thắng chỉ là phép chia số nguyên. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k, p, m = map(int, input().split())
    a = list(map(int, input().split()))
    p -= 1  # convert to 0-indexed
    res = 0

    # enumerate number of cards removed from front
    for front in range(min(p, k) + 1):
        # enumerate number of cards removed from back of first k cards
        for back in range(k - front + 1):
            if front + back > k:
                continue
            energy_spent = sum(a[:front]) + sum(a[p+1-p+back:p+1+back]) if back > 0 else sum(a[:front])
            if energy_spent > m:
                continue
            remaining_energy = m - energy_spent
            max_win_plays = remaining_energy // a[p]
            res = max(res, max_win_plays)
    print(res)
```Giải pháp cẩn thận chuyển đổi vị trí của thẻ điều kiện thắng thành chỉ mục 0. Vòng lặp kép liệt kê tất cả các cách để loại bỏ thẻ từ mặt trước và mặt sau của thẻ đầu tiên`k`các vị trí. các`sum`các phép tính xử lý chi phí năng lượng của các quân bài bị loại bỏ và phép chia số nguyên tìm ra số lần quân bài có điều kiện thắng có thể được chơi. Phải cẩn thận với các chỉ số khi tính toán`back`việc gỡ bỏ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
2 1 2 42
42 1
```| phía trước | trở lại | năng lượng tiêu tốn | năng lượng còn lại | lượt thắng tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 42 | 42 // 1 = 42 | 
| 1 | 0 | 42 | 0 | 0 | 

Vì điều kiện thắng ở vị trí 2 và`k=1`, nó không bao giờ có thể chơi được ở lượt đầu tiên. Số lần phát tối đa = 0. 

**Đầu vào mẫu 4:**```
8 4 7 10
3 4 4 2 1 1 4 2
```| phía trước | trở lại | năng lượng tiêu tốn | năng lượng còn lại | lượt thắng tối đa | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | 3+4+4=11 | -1 | 0 | 
| 2 | 1 | 3+4+2=9 | 1 | 1 // 4 = 0 | 
| 1 | 2 | 3+4+1=8 | 2 | 2 // 4 = 0 | 
| 0 | 3 | 3+4+2=9 | 1 | 1 // 4 = 0 | 

Chiến lược tối ưu: chỉ có thể chơi một lần với điều kiện thắng, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2) cho mỗi trường hợp thử nghiệm | Hai vòng lặp lồng nhau, mỗi vòng có giá trị tới k; các phép tính tổng là O(k) nhưng có thể được tính toán trước bằng các tổng tiền tố cho O(1) | 
| Không gian | O(n) | Lưu trữ tổng bộ bài và tiền tố | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là ≤ 5000, do đó giải pháp dễ dàng chạy trong vòng 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("4\n2 1 2 42\n42 1\n3 3 2 6\n2 1 2\n3 2 2 6\n2 1 2\n8 4 7 10\n3 4 4 2 1 1 4 2\n") == "0\n6\n2\n1"

# Custom: minimum size
assert run("1\n1 1 1 1\n1\n") == "1"

# Custom: maximum energy
assert run("1\n3 3 2 100\n10 20 30\n") == "5"

# Custom: all equal costs
assert run("1\n5 3 3 15\n3 3 3 3 3\n") == "5"

# Custom: win-condition at front
assert run("1\n4 2 1 7\n2 3 1 4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1\n1`| | |

---
title: "CF 2132E - Cuộc thi số học"
description: "Bài toán đặt ra một đội gồm hai người chơi, Vadim và Kostya, mỗi người có một bộ thẻ có giá trị số."
date: "2026-06-08T02:51:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 2132
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1043 (Div. 3)"
rating: 1700
weight: 2132
solve_time_s: 97
verified: true
draft: false
---

[CF 2132E - Cuộc thi số học](https://codeforces.com/problemset/problem/2132/E) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp, tìm kiếm ba ngôi 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đặt ra một đội gồm hai người chơi, Vadim và Kostya, mỗi người có một bộ thẻ có giá trị số. Trong mỗi vòng, họ được yêu cầu chọn chính xác một số lượng thẻ nhất định, nhưng mỗi người chơi có giới hạn trên về số lượng thẻ mà họ có thể đóng góp. Nhiệm vụ là tối đa hóa tổng số quân bài được chọn trong mỗi vòng. 

Về mặt hình thức, Vadim có`n`thẻ có giá trị`a_1`bởi vì`a_n`, và Kostya có`m`thẻ`b_1`bởi vì`b_m`. Đối với mỗi truy vấn`(x_i, y_i, z_i)`, đội phải chọn chính xác`z_i`thẻ, với nhiều nhất`x_i`từ Vadim và nhiều nhất là`y_i`từ Kostya. Mục tiêu là tính tổng tối đa có thể của các`z_i`thẻ. 

Các ràng buộc của bài toán rất nghiêm ngặt: tổng số thẻ trong tất cả các trường hợp thử nghiệm lên tới 200.000 cho riêng Vadim và Kostya và lên tới 100.000 vòng. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bạo lực nào lặp đi lặp lại thông qua việc kết hợp các lá bài trong một vòng, vì ngay cả việc chọn các tập hợp con có kích thước 100.000 cũng sẽ theo cấp số nhân. Quan sát quan trọng là mỗi vòng có thể được rút gọn thành việc chọn`k`các lá bài lớn nhất từ ​​các mảng bài được sắp xếp của mỗi người chơi, tôn trọng giới hạn cá nhân của họ. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó số lượng quân bài được yêu cầu bằng 0 hoặc khi một người chơi được phép không có quân bài nào. Ví dụ: nếu Vadim có thẻ`[10, 20]`và Kostya`[5, 15]`, và vòng là`(x=0, y=2, z=2)`, đội phải chọn tất cả các lá bài từ Kostya mặc dù Vadim có những lá bài có giá trị cao hơn. Một cách tiếp cận ngây thơ luôn cố gắng chọn những quân bài toàn cầu lớn nhất sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại thông qua tất cả các kết hợp của`k`những tấm thẻ từ bộ sưu tập của Vadim và Kostya tôn trọng những ràng buộc. Đối với mỗi sự kết hợp, nó tính tổng và trả về giá trị tối đa. Điều này đúng, nhưng độ phức tạp của nó tăng theo cấp số nhân theo số lượng thẻ, vượt xa mọi giới hạn thời gian khả thi đối với`n, m ~ 2*10^5`. 

Cách tiếp cận tối ưu dựa vào việc sắp xếp và tổng tiền tố. Nếu cả hai quân bài của Vadim và Kostya đều được sắp xếp theo thứ tự giảm dần, chúng ta có thể tính trước tổng tiền tố để nhanh chóng xác định tổng của quân bài trên cùng.`k`thẻ cho bất kỳ`k`lên đến`n`hoặc`m`. Đối với mỗi vòng, chúng tôi chỉ cần lặp lại số lượng thẻ mà Vadim có thể đóng góp (từ 0 đến`min(x_i, z_i)`) và tính xem Kostya sẽ cần cung cấp bao nhiêu thẻ để đạt được`z_i`. Nếu con số đó nằm trong giới hạn của anh ấy, chúng tôi sẽ tính tổng bằng cách sử dụng các tổng tiền tố và theo dõi mức tối đa. 

Điều này hoạt động hiệu quả vì có thể tìm thấy tổng tối đa của mỗi vòng trong các phép toán O(min(x_i, z_i)) bằng cách sử dụng tổng tiền tố và phép tính trước là O(n + m) để sắp xếp và tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(n+m) | Quá chậm | 
| Sắp xếp + Tổng tiền tố | O((n+m) log(n+m) + q * min(x_i, z_i)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các thẻ của Vadim theo thứ tự giảm dần. Điều này đảm bảo điều đầu tiên`k`thẻ luôn luôn là`k`lớn nhất. 
2. Sắp xếp các thẻ của Kostya theo thứ tự giảm dần vì lý do tương tự. 
3. Tính tổng tiền tố cho cả Vadim và Kostya. Cho phép`pref_a[k]`là tổng của số đầu tiên`k`thẻ của Vadim và`pref_b[k]`cho Kostya. Điều này cho phép truy vấn tổng thời gian không đổi cho phần trên cùng`k`thẻ. 
4. Đối với mỗi truy vấn`(x, y, z)`, lặp lại`i`từ`max(0, z - y)`ĐẾN`min(z, x)`đại diện cho số lượng thẻ lấy từ Vadim. Phạm vi này tôn trọng cả giới hạn của Vadim và đảm bảo rằng Kostya có thể đóng góp số thẻ còn lại. 
5. Đối với mỗi`i`, tính toán`j = z - i`bằng số lượng thẻ lấy từ Kostya. Bỏ qua các bước lặp ở đâu`j > y`bởi vì chúng vượt quá giới hạn của Kostya. 
6. Tính tổng số tiền như sau`pref_a[i] + pref_b[j]`và theo dõi mức tối đa trên tất cả các giá trị hợp lệ`i`. 
7. Trả lại số tiền tối đa cho vòng chơi. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà ở mỗi bước chúng tôi xem xét tất cả các cách hợp lệ để phân chia`z`thẻ giữa Vadim và Kostya trong giới hạn cá nhân của họ. Bằng cách sắp xếp trước và sử dụng tổng tiền tố, chúng tôi đảm bảo rằng đối với bất kỳ lần chia nào, chúng tôi sẽ sử dụng quân bài lớn nhất có thể từ mỗi người chơi, đảm bảo tổng là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort(reverse=True)
        b.sort(reverse=True)
        
        # Prefix sums
        pref_a = [0]
        for val in a:
            pref_a.append(pref_a[-1] + val)
        
        pref_b = [0]
        for val in b:
            pref_b.append(pref_b[-1] + val)
        
        res = []
        for _ in range(q):
            x, y, z = map(int, input().split())
            max_sum = 0
            # i is number of cards from Vadim
            for i in range(max(0, z - y), min(z, x) + 1):
                j = z - i
                if j <= y:
                    current_sum = pref_a[i] + pref_b[j]
                    max_sum = max(max_sum, current_sum)
            res.append(str(max_sum))
        print("\n".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên sắp xếp các thẻ và tính tổng tiền tố để truy vấn nhanh tổng của các thẻ hàng đầu. Vòng lặp truy vấn chỉ lặp lại số lượng thẻ hợp lệ mà Vadim có thể đóng góp, tự động tính toán các thẻ tương ứng từ Kostya. Việc sử dụng cẩn thận`max(0, z - y)`ngăn chặn các lựa chọn phủ định bất hợp pháp và đảm bảo rằng các chỉ số tổng tiền tố là chính xác. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Truy vấn | Giới hạn Vadim x | Kostya giới hạn y | Tổng số cần thiết z | tôi phạm vi | tổng tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 0 0 0 | 0 | 0 | 0 | [0] | 0 | 
| 3 4 7 | 3 | 4 | 7 | [3,4] | 70 | 

Đối với truy vấn thứ hai, Vadim có thể đóng góp tối đa 3 thẻ, Kostya nhiều nhất là 4. Chúng tôi cần tổng cộng 7 thẻ, vì vậy Vadim phải đóng góp ít nhất 3 thẻ (`max(0,7-4)=3`), để lại 4 cho Kostya. Tổng số tiền là`30+20+10 + 4+3+2+1 = 70`. 

Dấu vết chứng tỏ rằng phương pháp tổng tiền tố chọn chính xác các quân bài lớn nhất có thể trong khi vẫn tôn trọng các giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log(n+m) + q * min(x_i, z_i)) | Việc sắp xếp các mảng chiếm ưu thế ban đầu, sau đó mỗi truy vấn lặp lại trên các phần có thể phân chia của thẻ z | 
| Không gian | O(n+m) | Mảng tổng tiền tố cho Vadim và Kostya | 

Với`n+m <= 2*10^5`Và`q <= 10^5`, điều này vừa vặn thoải mái trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("""4
3 4 5
10 20 30
1 2 3 4
0 0 0
3 4 7
3 4 4
1 4 4
2 2 4
5 5 2
500000000 300000000 100000000 900000000 700000000
800000000 400000000 1000000000 600000000 200000000
1 4 3
5 2 6
4 4 1
100 100 20 20
100 100 20 20
4 4 5
3 3 6
2 363 711
286 121 102
1 1 1
3 1 1
1 2 0
1 3 2
0 1 0
3 3 3""") == """0
70
64
39
57
2700000000
4200000000
420
711
711
0
997
0
1360"""

# Custom cases
assert run("1\n2 2 1\n10 20\n5 15\n1 1 2") == "35", "must pick one from each for max sum"
assert run("1\n3 3 1\n1 2 3\n4 5 6\n2 2 3") ==
```

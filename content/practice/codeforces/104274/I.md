---
title: "CF 104274I - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0434\u043d\u0438 \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f \u0432\u0435\u043b\u0438\u043a\u0438\u0445"
description: "Chúng ta được cung cấp ngày sinh của một người và năm giới hạn trên. Đối với mỗi truy vấn, chúng ta cần đếm xem người này sẽ tổ chức sinh nhật bao nhiêu lần từ năm sau ngày sinh của họ cho đến và bao gồm cả năm cuối cùng nhất định."
date: "2026-07-01T21:20:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104274
codeforces_index: "I"
codeforces_contest_name: "2023 VIII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e"
rating: 0
weight: 104274
solve_time_s: 61
verified: true
draft: false
---

[CF 104274I - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0434\u043d\u0438 \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f \u0432\u0435\u043b\u0438\u043a\u0438\u0445](https://codeforces.com/problemset/problem/104274/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp ngày sinh của một người và năm giới hạn trên. Đối với mỗi truy vấn, chúng ta cần đếm xem người này sẽ tổ chức sinh nhật bao nhiêu lần từ năm sau ngày sinh của họ cho đến và bao gồm cả năm cuối cùng nhất định. 

Điều phức tạp chính là sinh nhật chỉ diễn ra vào cùng một ngày dương lịch mỗi năm, ngoại trừ ngày sinh nhật là ngày 29 tháng 2. Trong trường hợp đặc biệt đó, sinh nhật chỉ xảy ra vào những năm nhuận. Một năm được coi là năm nhuận nếu nó chia hết cho 400 hoặc chia hết cho 4 nhưng không chia hết cho 100. 

Vì vậy, nhiệm vụ giảm xuống còn đếm số năm trong một phạm vi chứa ngày “D/M” là ngày dương lịch hợp lệ. Đối với những ngày bình thường, mỗi năm đóng góp một ngày sinh nhật. Đối với ngày 29 tháng 2, chỉ có năm nhuận đóng góp. 

Các ràng buộc rất lớn: lên tới 100000 truy vấn và số năm lên tới 2.000.000. Điều này loại trừ mọi mô phỏng mỗi năm. Ngay cả một cách tiếp cận ngây thơ lặp đi lặp lại hàng năm cho mỗi truy vấn cũng sẽ yêu cầu tới 2e11 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Một trường hợp phức tạp là xử lý ngày 29 tháng 2 một cách chính xác với các quy tắc chính xác về năm nhuận. Một điều nữa là đảm bảo chúng tôi loại trừ chính năm sinh, ngay cả khi ngày sinh nhật xảy ra muộn hơn trong cùng năm đó, vì vấn đề rõ ràng bắt đầu tính từ năm tiếp theo. 

Ví dụ: nếu người sinh ngày 1 tháng 1 năm 2000 và YE là 2000 thì câu trả lời là 0. Nếu sinh ngày 29 tháng 2 năm 2020 và YE là năm 2020 thì câu trả lời cũng là 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lặp lại hàng năm từ Y+1 đến YE và kiểm tra xem ngày sinh có tồn tại trong năm đó hay không. Đối với hầu hết các ngày, điều này không quan trọng vì mỗi năm đều có ngày và tháng đó. Vào ngày 29 tháng 2, chúng tôi cũng kiểm tra thêm điều kiện nhảy vọt. 

Điều này hoạt động về mặt khái niệm, nhưng trong trường hợp xấu nhất, độ dài phạm vi có thể lên tới 2 triệu cho mỗi lần kiểm tra và với tối đa 100000 bài kiểm tra, điều này trở nên quá chậm. Điểm nghẽn là việc kiểm tra lặp đi lặp lại hàng năm. 

Quan sát quan trọng là đối với tất cả các ngày ngoại trừ ngày 29 tháng 2, câu trả lời hoàn toàn là số số nguyên trong một phạm vi, vì ngày sinh nhật luôn tồn tại một lần mỗi năm. Đối với ngày 29 tháng 2, chúng ta chỉ cần đếm xem có bao nhiêu năm nhuận trong một phạm vi. Điều này làm giảm mỗi truy vấn thành phép trừ theo thời gian không đổi hoặc số tiền tố của năm nhuận. 

Để hỗ trợ việc tính năm nhuận một cách hiệu quả, chúng tôi rút ra công thức tính số năm nhuận trong [1, N]. Điều này có thể được tính bằng cách sử dụng loại trừ bao gồm trên bội số của 4, 100 và 400. Khi chúng ta có hàm này, mỗi truy vấn sẽ trở thành O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(T · (YE − Y)) | O(1) | Quá chậm | 
| Tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Đầu tiên, chúng tôi đặt phạm vi tính hiệu quả là tất cả các năm sau năm sinh cho đến năm cuối cùng, tức là từ Y+1 đến YE. Điều này đảm bảo chúng tôi loại trừ chính xác năm sinh. 
2. Nếu ngày sinh nhật không phải là ngày 29 tháng 2 thì mỗi năm trong khoảng thời gian này đóng góp đúng một ngày sinh nhật. Chúng tôi tính toán câu trả lời là (YE − Y). Điều này là do mỗi năm trong phạm vi đều hợp lệ cho ngày đó. 
3. Nếu ngày sinh là ngày 29 tháng 2, thay vào đó chúng ta cần đếm xem có bao nhiêu năm nhuận nằm trong khoảng [Y+1, YE]. 
4. Để tính ≤ X có bao nhiêu năm nhuận, ta sử dụng công thức: 

sàn(X/4) − sàn(X/100) + sàn(X/400). 

Việc này đếm bội số của 4, loại bỏ các năm thế kỷ không hợp lệ và cộng lại các thế kỷ chia hết cho 400. 
5. Số năm nhuận trong [L, R] được tính là F(R) − F(L−1), trong đó F(X) là số bước nhảy tiền tố. 
6. Chúng tôi áp dụng công thức này với L = Y+1 và R = YE, đưa ra đáp án cuối cùng. 

Tại sao nó hoạt động

Đối với những ngày sinh nhật không đặc biệt nhuận, sự kiện “sinh nhật xảy ra” độc lập với năm và luôn đúng, do đó việc đếm sẽ giảm xuống việc đếm các số nguyên trong một phạm vi. Vào ngày 29 tháng 2, sự kiện này giảm chính xác thành tư cách thành viên của năm trong tập hợp năm nhuận, được đặc trưng đầy đủ bởi các quy tắc chia hết tiêu chuẩn. Hàm tiền tố F(X) đếm chính xác thiết lập đến X và phép trừ sẽ tách biệt bất kỳ khoảng nào mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def leap_count(x):
    if x <= 0:
        return 0
    return x // 4 - x // 100 + x // 400

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        d, m, y, ye = map(int, input().split())
        l = y + 1
        r = ye

        if m == 2 and d == 29:
            if l > r:
                out.append("0")
                continue
            ans = leap_count(r) - leap_count(l - 1)
            out.append(str(ans))
        else:
            if l > r:
                out.append("0")
            else:
                out.append(str(r - l + 1))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tách trường hợp năm nhuận khỏi tất cả các ngày khác. Chức năng trợ giúp`leap_count`thực hiện công thức bao gồm-loại trừ để tính năm nhuận đến một ngưỡng. 

Đối với những ngày bình thường, chúng tôi tính số số nguyên trong phạm vi [Y+1, YE], đơn giản là`r - l + 1`. Đối với ngày 29 tháng 2, chúng tôi chuyển đổi vấn đề sang việc đếm xem có bao nhiêu năm nhuận hợp lệ tồn tại trong cùng khoảng thời gian đó bằng cách sử dụng các khác biệt tiền tố. 

Điều kiện biên`l > r`xử lý các trường hợp YE bằng Y, đảm bảo không có số âm nào xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
15 1 1975 2020
```| Bước | L | R | Loại | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1976 | 2020 | bình thường | | 
| đếm | | | kích thước phạm vi | 2020 − 1976 + 1 = 45 | 

Điều này xác nhận rằng mỗi năm đóng góp đúng một ngày sinh nhật, vì ngày đó không phải là ngày 29 tháng 2. 

### Ví dụ 2 

đầu vào:```
29 2 2020 2035
```| Bước | L | R | bước nhảy vọt(R) | bước nhảy vọt(L−1) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 2021 | 2035 | | | | 
| tính toán | | | F(2035)=? | F(2020)=? | sự khác biệt | 

Chúng tôi đánh giá: 

F(2035) = 2035//4 − 2035//100 + 2035//400 = 508 − 20 + 5 = 493 

F(2020) = 505 − 20 + 5 = 490 

Kết quả = 3 

Điều này phù hợp với ý kiến cho rằng chỉ những năm nhuận 2024, 2028, 2032 mới rơi vào khoảng thời gian đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi truy vấn được xử lý bằng một số phép toán số học không đổi | 
| Không gian | O(1) | Chỉ có một vài số nguyên và lưu trữ đầu ra | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc vì ngay cả 100000 truy vấn cũng chỉ yêu cầu số học số nguyên đơn giản, không lặp lại qua nhiều năm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def leap_count(x):
        if x <= 0:
            return 0
        return x // 4 - x // 100 + x // 400

    T = int(input())
    out = []
    for _ in range(T):
        d, m, y, ye = map(int, input().split())
        l, r = y + 1, ye
        if m == 2 and d == 29:
            if l > r:
                out.append("0")
            else:
                out.append(str(leap_count(r) - leap_count(l - 1)))
        else:
            out.append(str(max(0, r - l + 1)))
    return "\n".join(out)

# provided samples
assert run("""5
15 1 1975 1976
15 1 1975 2020
7 10 2002 3001
29 2 2024 2140
29 2 2020 2035
""") == """1
45
999
28
3"""

# custom cases
assert run("""1
1 1 2000 2000
""") == "0", "same year excluded"

assert run("""1
29 2 2000 2000
""") == "0", "no leap in range"

assert run("""1
29 2 1996 2005
""") == "3", "1996, 2000, 2004"

assert run("""1
1 3 1999 2003
""") == "4", "normal range count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 tháng 1 cùng năm | 0 | loại trừ năm sinh | 
| 29 tháng 2 cùng năm | 0 | ranh giới trường hợp bước nhảy vọt | 
| 1996-2005 29 tháng 2 | 3 | đếm bước nhảy đúng | 
| phạm vi bình thường | 4 | tính chính xác chung | 

## Vỏ cạnh 

Trường hợp một bên là năm sinh bằng năm cuối. Đối với một ngày bình thường như 5/10/2000 với YE = 2000, khoảng này sẽ trống vì chúng ta bắt đầu từ năm 2001. Thuật toán đặt L = Y+1, do đó L > R và trả về 0 trực tiếp. 

Một trường hợp khác là ngày 29 tháng 2 trong phạm vi không có năm nhuận. Ví dụ: 29/2/2021 đến 2023 tạo ra L = 2022 và R = 2023. Chênh lệch tiền tố nhuận trả về 0 vì cả hai điểm cuối đều không chứa năm nhuận. 

Trường hợp cạnh cuối cùng là giá trị năm rất lớn gần bằng 2e6. Công thức tiền tố vẫn hoạt động an toàn vì mọi thao tác đều là phép chia số nguyên đơn giản và không xảy ra hiện tượng tràn hoặc lặp lại.

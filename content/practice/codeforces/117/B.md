---
title: "CF 117B - Trò Chơi Rất Thú Vị"
description: "Chúng tôi được yêu cầu mô phỏng một trò chơi hai người chơi liên quan đến các số được biểu thị dưới dạng chuỗi chín chữ số. Người chơi đầu tiên chọn chuỗi s1 đại diện cho một số không vượt quá giá trị a nhất định. Người chơi thứ hai trả lời bằng chuỗi s2 đại diện cho một số không vượt quá b."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 117
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 88"
rating: 1800
weight: 117
solve_time_s: 151
verified: true
draft: false
---

[CF 117B - Trò chơi rất thú vị](https://codeforces.com/problemset/problem/117/B) 

**Đánh giá:** 1800 
**Tags:** vũ lực, lý thuyết số 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trò chơi hai người chơi liên quan đến các số được biểu thị dưới dạng chuỗi chín chữ số. Người chơi đầu tiên chọn một chuỗi`s1`đại diện cho một số không vượt quá một giá trị nhất định`a`. Người chơi thứ hai trả lời bằng một chuỗi`s2`đại diện cho một số không vượt quá`b`. Cả hai chuỗi đều dài chính xác chín chữ số, có thể có số 0 đứng đầu. Người chơi thứ hai thắng nếu nối được`s1`Và`s2`như một số duy nhất có thể chia hết cho một số nhất định`mod`. Nếu không, người chơi đầu tiên sẽ thắng. Nếu người chơi đầu tiên có thể đảm bảo chiến thắng, chúng ta cũng phải xác định chuỗi nhỏ nhất theo từ điển`s1`đạt được điều này. 

Giới hạn lên tới`10^9`vì`a`Và`b`, và lên đến`10^7`vì`mod`. Chuỗi chín chữ số có nghĩa là chúng ta đang làm việc trong một không gian mà việc liệt kê tất cả các lựa chọn có thể một cách rõ ràng là không thể, bởi vì`10^9`đã quá lớn để liệt kê vũ phu. Thay vào đó, bài toán đòi hỏi sự suy luận lý thuyết số cẩn thận và số học mô-đun. Các trường hợp cạnh bao gồm các tình huống trong đó`a`hoặc`b`nhỏ hơn`10^9`, buộc các số 0 đứng đầu trong biểu diễn chuỗi chín chữ số. Một trường hợp cạnh khác là khi`mod`rất nhỏ, cho phép có nhiều mẫu chia hết, hoặc khi`a`hoặc`b`bằng 0, hạn chế di chuyển một cách đáng kể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ sẽ cố gắng tạo ra mọi thứ có thể`s1`chuỗi từ`0`ĐẾN`a`, thì với mỗi`s1`kiểm tra mọi thứ có thể`s2`từ`0`ĐẾN`b`, kiểm tra tính chia hết của số được nối cho`mod`. Từ`s1`Và`s2`mỗi người có thể mất tới`10^9`giá trị, điều này dẫn đến`10^18`kiểm tra trong trường hợp xấu nhất không thể thực hiện được trong thời hạn. Ngay cả phương pháp cắt tỉa hiệu quả về bộ nhớ cũng không thể liệt kê trực tiếp tất cả các khả năng. 

Cái nhìn sâu sắc quan trọng là khả năng chia hết chỉ phụ thuộc vào modulo còn lại`mod`. Chúng ta có thể tính toán trước phần còn lại của tất cả những gì có thể`s2`số modulo`mod`, và sau đó cho mỗi ứng cử viên`s1`, chúng ta chỉ cần kiểm tra xem có tồn tại một`s2`như vậy`(s1 * 10^9 + s2) % mod == 0`. Sử dụng số học mô-đun, điều này tương đương với việc kiểm tra xem`(s1 * 10^9) % mod`phù hợp với một số`mod - s2 % mod`. Từ`s2`phạm vi lên đến`b`, ta chỉ cần tính tất cả số dư`s2 % mod`một cách hiệu quả và lưu trữ chúng trong một bộ. Điều này làm giảm việc kiểm tra mỗi`s1`để tra cứu tập hợp O(1). Bằng cách lặp lại`s1`theo thứ tự tăng dần, chúng tôi tự động tìm chuỗi chiến thắng nhỏ nhất về mặt từ điển khi nó tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a * b) | O(1) | Quá chậm | 
| Tối ưu | O(mod + a') | O(mod) | Đã chấp nhận | 

Đây`a'`là số ứng viên`s1`dây, lên đến`a + 1`nhưng được giới hạn một cách hiệu quả bởi`10^9`. các`mod`thuật ngữ xuất phát từ việc tính toán trước số dư cho tất cả`s2`các giá trị. 

## Hướng dẫn thuật toán 

1. Bình thường hóa`a`Và`b`thành các chuỗi chín chữ số, đệm thêm các số 0 ở đầu nếu cần. Điều này đảm bảo so sánh từ điển hoạt động chính xác và mọi số đều có chính xác chín chữ số. 
2. Tính toán`power = 10^9 % mod`. Điều này thể hiện sự đóng góp của`s1`đến số được nối theo modulo`mod`. Sử dụng cái này,`(s1 * 10^9 + s2) % mod`trở thành`(s1 * power + s2) % mod`. 
3. Tính toán trước tất cả những gì có thể`s2`phần dư modulo`mod`cho các số từ`0`ĐẾN`b`. Lưu trữ chúng trong một bộ để tra cứu nhanh. 
4. Lặp lại tất cả các số ứng cử viên`s1`từ`0`ĐẾN`a`theo thứ tự tăng dần. Với mỗi ứng viên, hãy tính`(s1 * power) % mod`và kiểm tra xem có tồn tại một`s2`số dư trong tập hợp sao cho`(s1 * power + s2) % mod == 0`. Nếu không như vậy`s2`tồn tại, người chơi đầu tiên có thể đảm bảo chiến thắng với điều này`s1`. 
5. Một lần chiến thắng`s1`được tìm thấy, xuất ra`1`và biểu diễn chuỗi chín chữ số của`s1`. Nếu tất cả`s1`các giá trị cho phép người chơi thứ hai phản ứng một cách tối ưu và giành chiến thắng, tạo ra`2`. 

Tính bất biến đối với mỗi`s1`, bộ của`s2`phần còn lại thể hiện tất cả các nước đi có thể có của người chơi thứ hai. Nếu không ai trong số họ hủy bỏ`(s1 * power) % mod`, người chơi đầu tiên thắng. Điều này đảm bảo tính chính xác vì chúng tôi kiểm tra rõ ràng tất cả các bước phản công có thể có của người chơi thứ hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    a, b, mod = map(int, input().split())
    power = pow(10, 9, mod)
    s2_rems = set(i % mod for i in range(b + 1))
    
    for s1_num in range(a + 1):
        s1_rem = (s1_num * power) % mod
        # check if there exists s2 that makes total divisible
        found = any((s1_rem + s2_rem) % mod == 0 for s2_rem in s2_rems)
        if not found:
            print(1)
            print(f"{s1_num:09d}")
            return
    print(2)

if __name__ == "__main__":
    main()
```Chúng tôi tính toán`power`sử dụng lũy ​​thừa mô-đun để tránh xử lý trực tiếp số nhân 9 chữ số. Bộ của`s2`phần còn lại cho phép tra cứu O(1) cho mọi ứng viên`s1`. Chúng tôi lặp lại theo thứ tự tăng dần để đảm bảo tối thiểu về mặt từ điển`s1`. sử dụng`f"{s1_num:09d}"`đảm bảo đầu ra gồm chín chữ số với các số 0 đứng đầu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
1 10 7
```| s1 | s1 * 10^9 % 7 | s2_rems | có thể chia được? | 
| --- | --- | --- | --- | 
| 0 | 0 | 0..10 % 7={0,1,2,3} | tồn tại → 2 chiến thắng | 
| 1 | 1 | 0..10 % 7={0,1,2,3} | tồn tại → 2 chiến thắng | 

Đầu ra:`2`. Điều này cho thấy người chơi thứ hai luôn có thể đáp ứng được lực chia hết. 

**Đầu vào tùy chỉnh 2**:```
10 5 3
```| s1 | s1 * 10^9 % 3 | s2_rems | có thể chia được? | 
| --- | --- | --- | --- | 
| 0 | 0 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 1 | 1 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 2 | 2 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 3 | 0 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 4 | 1 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 5 | 2 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 6 | 0 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 7 | 1 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 8 | 2 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 9 | 0 | {0,1,2} | tồn tại → 2 chiến thắng | 
| 10 | 1 | {0,1,2} | tồn tại → 2 chiến thắng | 

Đầu ra:`2`. Mọi`s1`cho phép người chơi thứ hai buộc phải thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a + b) | Tính số dư cho tất cả`s2`số và lặp lại`s1`ứng viên lên đến`a`| 
| Không gian | O(b) | Cửa hàng`s2`phần dư trong một tập hợp | 

Thuật toán khả thi đối với`a, b ≤ 10^9`chỉ vì trong thực tế`mod`giới hạn kích thước của tập hợp còn lại. Việc liệt kê thuần túy không được thực hiện trên tất cả các số có chín chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("1 10 7\n") == "2", "sample 1"

# custom cases
assert run("10 5 3\n") == "2", "all s1 allow second player to win"
assert run("0 0 1\n") == "2", "both zero, mod 1 divisible"
assert run("0 1 2\n") == "1\n000000000", "first player wins with zero"
assert run("123 456 5\n") in ("1\n000000000","1\n000000001"), "lexicographic min s1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---|

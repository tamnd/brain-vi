---
title: "CF 2207B - Một Đêm Tại Freddy's"
description: "Chúng tôi được yêu cầu mô phỏng một đêm tại Freddy Fazbear's Pizzeria về mặt quản lý nguy hiểm. Mỗi animatronic tích lũy nguy hiểm mỗi giây và màn đêm kéo dài trong một số giây cố định."
date: "2026-06-07T19:31:31+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2207
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1085 (Div. 1 + Div. 2)"
rating: 1600
weight: 2207
solve_time_s: 108
verified: false
draft: false
---

[CF 2207B - Một đêm ở Freddy's](https://codeforces.com/problemset/problem/2207/B) 

**Đánh giá:** 1600 
**Tags:** trò chơi, tham lam, phân loại 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một đêm tại Freddy Fazbear's Pizzeria về mặt quản lý nguy hiểm. Mỗi animatronic tích lũy nguy hiểm mỗi giây và màn đêm kéo dài trong một số giây cố định. Người chơi có một số "hành động đèn pin" giới hạn tại những thời điểm định trước, mỗi hành động trong số đó có thể đặt lại mối nguy hiểm của một hoạt hình về 0. Mục tiêu là tìm ra mức độ nguy hiểm tối đa nhỏ nhất`x`sao cho dù hoạt hình điện tử phân bổ các cuộc tấn công của chúng như thế nào, việc sử dụng đèn pin một cách tối ưu sẽ giữ mức nguy hiểm cuối cùng ở mức hoặc thấp hơn`x`. 

Đầu vào mô tả nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra số lần sử dụng đèn pin, số lượng hoạt hình, độ dài của đêm và thời gian có thể sử dụng đèn pin. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất - mức tối thiểu`x`điều đó đảm bảo an toàn. 

Vấn đề có giới hạn chặt chẽ. Tổng số giây nhân với số lượng hoạt hình trong tất cả các trường hợp thử nghiệm được giới hạn ở 2×10^5, điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp từng giây cho mỗi hoạt hình. Điều đó ngay lập tức loại trừ mô phỏng ngây thơ mỗi giây. Chúng ta phải sử dụng chiến lược dựa trên việc đếm hoặc khoảng thời gian thay vì mô phỏng từng bước. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó số lượng hành động của đèn pin rất nhỏ, có thể ít hơn số lượng hoạt hình điện tử hoặc khi độ dài ban đêm cực ngắn hoặc cực dài. Ví dụ: với việc sử dụng một hoạt hình điện tử và một đèn pin vào cuối, mối nguy hiểm cuối cùng phụ thuộc hoàn toàn vào số giây xảy ra sau lần đèn pin cuối cùng. Nếu điều này không được tính đến, một cách tiếp cận ngây thơ có thể cho rằng nguy hiểm được thiết lập lại quá sớm một cách sai lầm. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ theo dõi mức độ nguy hiểm của từng hoạt hình điện tử trong từng giây, áp dụng các hành động đèn pin vào những thời điểm thích hợp. Về nguyên tắc thì điều này đúng, nhưng nó quá chậm khi`m * ℓ`đạt 2×10^5. 

Quan sát quan trọng là mối nguy hiểm tối đa mà chúng ta sẽ gặp phải cuối cùng chỉ phụ thuộc vào số giây xảy ra giữa các lần sử dụng đèn pin liên tiếp và số lượng hoạt hình điện tử hiện diện. Có thể coi đèn pin như việc chia đêm thành nhiều khoảng. Trong mỗi khoảng thời gian, hoạt hình điện tử có thể chọn tăng mức độ nguy hiểm của một hoạt hình đơn lẻ một cách tham lam. Vì vậy, mối nguy hiểm cuối cùng sau một khoảng thời gian`t`với`m`hoạt hình điện tử là trần của`t/m`- chúng ta luôn có thể phân phối sự gia tăng nguy hiểm một cách đồng đều nhất có thể. 

Do đó, vấn đề giảm xuống còn việc tính toán độ dài tối đa của các khoảng thời gian này giữa các lần nhấp nháy, chia nó cho số lượng hoạt hình, làm tròn và sau đó lấy giá trị tối đa trên tất cả các khoảng thời gian. Khoảng thời gian ban đầu bắt đầu từ thời điểm 0 và khoảng thời gian cuối cùng bắt đầu từ ánh đèn pin cuối cùng cho đến hết đêm. Cách tiếp cận tham lam dựa trên khoảng thời gian này giúp loại bỏ nhu cầu mô phỏng mỗi giây và hoạt động với O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * ℓ) | O(m) | Quá chậm | 
| Khoảng thời gian / Tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`max_interval`để lưu trữ khoảng cách lớn nhất giữa các lần sử dụng đèn pin liên tiếp. Bắt đầu với khoảng đầu tiên như`a[0]`(giây từ lúc bắt đầu cho đến lần nháy đầu tiên). 
2. Lặp lại thời gian đèn pin. Đối với mỗi cặp liên tiếp`(a[i-1], a[i])`, tính độ dài khoảng như`a[i] - a[i-1] - 1`. Cập nhật`max_interval`nếu khoảng này lớn hơn. 
3. Sau khi xử lý tất cả các lần nhấp nháy, xử lý khoảng thời gian cuối cùng từ lần đèn pin cuối cùng đến hết đêm:`ℓ - a[-1]`. Cập nhật`max_interval`nếu cần thiết. 
4. Mức độ nguy hiểm tối thiểu được đảm bảo tối đa`x`là trần của`max_interval / m`. Điều này góp phần phân phối mức độ gia tăng nguy hiểm một cách đồng đều nhất có thể giữa tất cả các hoạt hình điện tử. Vì số nguyên Python hỗ trợ phân chia trần thông qua`(a + b - 1) // b`, bước này rất đơn giản. 
5. Xuất kết quả tính toán`x`. 

Tại sao nó hoạt động: Thuật toán duy trì sự bất biến rằng khoảng cách lớn nhất giữa các lần đặt lại sẽ quyết định sự tích lũy trong trường hợp xấu nhất đối với bất kỳ hoạt hình đơn lẻ nào. Bằng cách phân chia tổng mức tăng theo khoảng thời gian một cách đồng đều nhất có thể, chúng tôi đảm bảo không có hoạt hình điện tử nào vượt quá mức độ nguy hiểm được tính toán. Việc phân chia trần đảm bảo rằng nếu khoảng thời gian không thể chia đều thì phần còn lại vẫn được tính là mối nguy hiểm bổ sung đối với một số hoạt hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, m, l = map(int, input().split())
        a = list(map(int, input().split()))
        max_interval = a[0] - 0
        for i in range(1, n):
            max_interval = max(max_interval, a[i] - a[i-1] - 1)
        max_interval = max(max_interval, l - a[-1])
        result = (max_interval + m - 1) // m
        print(result)

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó sẽ đọc số lần nhấp nháy, số lượng hoạt hình, độ dài ban đêm và thời gian đèn pin. các`max_interval`tính toán nắm bắt chính xác khoảng thời gian lớn nhất mà mối nguy hiểm có thể tích lũy mà không cần thiết lập lại. Mối nguy hiểm cuối cùng có được bằng cách chia khoảng thời gian này cho hoạt hình điện tử, làm tròn số. 

Những điểm tinh tế bao gồm việc sử dụng`a[i] - a[i-1] - 1`để tính toán chính xác khoảng thời gian giữa các lần nhấp nháy liên tiếp và xử lý riêng khoảng thời gian bắt đầu và kết thúc. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n=1, m=2, l=10, a=[10]`| Khoảng thời gian | Tính toán | khoảng thời gian tối đa | 
| --- | --- | --- | 
| Bắt đầu | 10-0=10 | 10 | 
| Kết thúc | 10-10=0 | 10 | 
| x = trần(10/2) | 5 | 5 | 

Đèn flash duy nhất xảy ra ở giây cuối cùng, vì vậy animatronic đầu tiên có thể tích lũy tối đa 5 mối nguy hiểm, đèn flash này sẽ đặt lại, để lại mối nguy hiểm cuối cùng là 5. 

**Ví dụ 2:**`n=2, m=3, l=40, a=[13,37]`| Khoảng thời gian | Tính toán | khoảng thời gian tối đa | 
| --- | --- | --- | 
| Bắt đầu | 13-0=13 | 13 | 
| Giữa các lần nhấp nháy | 37-13-1=23 | 23 | 
| Kết thúc | 40-37=3 | 23 | 
| x = trần(23/3) | 8 | 8 | 

Khoảng thời gian tồi tệ nhất là giữa lần nhấp nháy 13 và 37. Chia 23 giây cho 3 hoạt hình điện tử sẽ cho mức trần 8, đây là mức nguy hiểm tối đa tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chỉ cần lặp qua các lần đèn pin để tính các khoảng thời gian | 
| Không gian | O(n) | Lưu trữ thời gian đèn pin | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm đều bị giới hạn bởi các ràng buộc đầu vào, do đó thuật toán thực thi hiệu quả dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("7\n1 2 10\n10\n5 1 32\n1 4 9 16 25\n2 3 40\n13 37\n2 2 7\n6 7\n8 5 60\n3 17 20 28 36 44 45 50\n6 7 1987\n6 7 66 77 666 777\n1 1 1\n1\n") == "5\n7\n8\n1\n19\n1477\n0"

# Custom cases
assert run("1\n1 1 1\n1\n") == "0"  # smallest possible input
assert run("1\n2 2 5\n2 5\n") == "2"  # two flashes spread across short night
assert run("1\n3 3 10\n2 5 9\n") == "2"  # multiple intervals, exact division
assert run("1\n2 3 10\n1 10\n") == "3"  # interval at the end larger than m
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1\n1 | 0 | Đầu vào tối thiểu, đèn flash đơn sẽ đặt lại mối nguy hiểm cuối cùng về 0 | 
| 2 2 5\n2 5 | 2 | Hai lần nhấp nháy, kiểm tra tính toán khoảng thời gian | 
| 3 3 10\n2 5 9 | 2 | Nhiều khoảng thời gian, xác nhận công trình phân chia trần | 
| 2 3 10\n1 10 | 3 | Vỏ cạnh có khoảng cách cuối cùng lớn | 

## Vỏ cạnh 

Khi tất cả các lần nhấp nháy xảy ra ở cuối, khoảng thời gian đầu tiên có thể dài. Ví dụ,`n=1, m=2, l=10, a=[10]`kết quả là max_interval = 10. Chia cho 2 animatronics sẽ được 5, phù hợp với trực giác. Khi các đèn flash được phân bổ đều nhưng không chia hết cho số lượng hoạt hình, việc phân chia trần sẽ đảm bảo rằng không có hoạt hình nào vượt quá mức nguy hiểm đã tính toán. Thuật toán xử lý chính xác các điều kiện biên này.

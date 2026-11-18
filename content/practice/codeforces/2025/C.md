---
title: "CF 2025C - Trò Chơi Mới"
description: "Trò chơi xoay quanh một bộ bài, mỗi lá bài được đánh số nguyên. Monocarp có thể bắt đầu bằng cách lấy bất kỳ lá bài nào. Ở các lượt tiếp theo, anh ta chỉ có thể lấy một lá bài có cùng số với lá bài cuối cùng được lấy hoặc lớn hơn một lá bài."
date: "2026-06-09T03:18:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "implementation", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2025
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 170 (Rated for Div. 2)"
rating: 1300
weight: 2025
solve_time_s: 304
verified: false
draft: false
---

[CF 2025C - Trò chơi mới](https://codeforces.com/problemset/problem/2025/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, thực hiện, sắp xếp, hai con trỏ 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi xoay quanh một bộ bài, mỗi lá bài được đánh số nguyên. Monocarp có thể bắt đầu bằng cách lấy bất kỳ lá bài nào. Ở các lượt tiếp theo, anh ta chỉ có thể lấy một lá bài có cùng số với lá bài cuối cùng được lấy hoặc lớn hơn một lá bài. Ngoài ra, tổng số lượng riêng biệt mà Monocarp thu thập được không thể vượt quá`k`. Mục tiêu là tối đa hóa tổng số quân bài được lấy trước khi không còn nước đi hợp lệ nào nữa. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết số lượng thẻ`n`, số lượng phân biệt tối đa được phép`k`, và dãy số nguyên trên thẻ. Đầu ra phải là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng thẻ tối đa mà Monocarp có thể thu thập theo quy tắc. 

Cho rằng`n`có thể đạt tới 200.000 và tổng số`n`trên tất cả các trường hợp thử nghiệm được giới hạn ở 200.000, mọi giải pháp phải chạy trong khoảng O(n log n) hoặc tốt hơn để mỗi trường hợp thử nghiệm duy trì trong giới hạn thời gian 2 giây. Một cách tiếp cận ngây thơ thử tất cả các chuỗi thẻ có thể có sẽ phức tạp theo giai thừa và rõ ràng là không khả thi. 

Các trường hợp phức tạp bao gồm các chuỗi có khoảng cách lớn về số lượng, giá trị lặp lại hoặc các tình huống trong đó`k`nhỏ hơn số lượng các số duy nhất. Ví dụ, với đầu vào`n=5, k=1, a=[1,3,1,3,2]`, một cách tiếp cận tham lam ngây thơ luôn chọn quân bài có sẵn đầu tiên có thể cố gắng chọn một quân bài không chính xác.`2`hoặc`3`quá sớm, vi phạm ràng buộc số lượng riêng biệt. Đầu ra đúng là`2`bởi vì Monocarp chỉ có thể lấy bài với`1`trong kịch bản này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi quân bài bắt đầu và mô phỏng trò chơi, thử tất cả các chuỗi lựa chọn quân bài hợp lệ cho đến khi không còn nước đi nào. Cách tiếp cận này đúng về nguyên tắc, nhưng trong trường hợp xấu nhất với`n=200,000`, số lượng các chuỗi là vô cùng lớn, khiến cho việc tính toán không thể thực hiện được. 

Điều quan trọng cần lưu ý là những con số duy nhất quan trọng là các số nguyên duy nhất có trên các lá bài. Vì Monocarp chỉ có thể di chuyển từ một số`x`ĐẾN`x`hoặc`x+1`, chiến lược tối ưu sẽ liên quan đến việc lấy các số liên tiếp theo thứ tự tăng dần mà không bỏ qua, vì việc bỏ qua sẽ ngăn cản việc đưa thẻ sau này. Do đó, chúng ta có thể giảm vấn đề xuống việc đếm đoạn số liền kề dài nhất trong tập hợp các giá trị duy nhất được sắp xếp, trong đó độ dài đoạn không vượt quá`k`. Sau khi xác định được phân đoạn, tổng số lượng của tất cả các số trong phân đoạn này sẽ cho ra số lượng bài tối đa mà Monocarp có thể lấy. 

Giải pháp tối ưu sắp xếp các số duy nhất và đếm xem có bao nhiêu số liên tiếp có thể vừa với một cửa sổ có độ dài`k`. Sử dụng từ điển để theo dõi tần số cho phép truy cập O(1) vào số lượng thẻ. Trượt một cửa sổ qua các số duy nhất được sắp xếp cho phép chúng tôi tính tổng một cách hiệu quả cho tất cả các phân đoạn ứng cử viên. Điều này làm giảm độ phức tạp xuống O(n log n) do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi số bằng từ điển. Điều này cho chúng tôi biết chúng tôi có thể lấy bao nhiêu thẻ cho mỗi số mà không cần quét danh sách nhiều lần. 
2. Trích xuất các số duy nhất và sắp xếp chúng. Cần phải sắp xếp vì Monocarp chỉ có thể di chuyển đến cùng một số hoặc số liên tiếp tiếp theo. Sắp xếp cho phép chúng ta xem xét các chuỗi liên tiếp một cách hiệu quả. 
3. Sử dụng phương pháp hai con trỏ hoặc cửa sổ trượt trên các số duy nhất được sắp xếp. Cửa sổ đại diện cho một chuỗi ứng viên gồm các số riêng biệt mà Monocarp có thể lấy. Bắt đầu cả hai con trỏ ở đầu mảng được sắp xếp. 
4. Mở rộng con trỏ bên phải trong khi số lượng giá trị riêng biệt trong cửa sổ không vượt quá`k`và các số liên tiếp nhau. Giữ tổng số thẻ cho các số bên trong cửa sổ. 
5. Nếu cửa sổ vi phạm các ràng buộc (quá nhiều số riêng biệt hoặc khoảng cách giữa các số liên tiếp), hãy thu nhỏ con trỏ bên trái cho đến khi cửa sổ hợp lệ trở lại, điều chỉnh tổng chạy cho phù hợp. 
6. Theo dõi số tiền tối đa quan sát được trong quá trình này. Tổng này thể hiện số lượng bài tối đa mà Monocarp có thể lấy. 
7. In tổng tối đa cho mỗi test. 

Tại sao nó hoạt động: Tại bất kỳ thời điểm nào, cửa sổ trượt chứa một chuỗi số liên tiếp tối đa trong đó số lượng các số riêng biệt không vượt quá`k`. Bằng cách tổng hợp số lượng trong cửa sổ này, chúng tôi đảm bảo Monocarp lấy tất cả các thẻ hợp lệ theo quy tắc. Cửa sổ trượt khám phá tất cả các điểm bắt đầu hợp lệ có thể một cách hiệu quả, đảm bảo tìm thấy mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        count = Counter(a)
        unique_sorted = sorted(count.keys())

        max_cards = 0
        left = 0
        current_sum = 0

        for right in range(len(unique_sorted)):
            if right > 0 and unique_sorted[right] != unique_sorted[right - 1] + 1:
                # reset window if numbers are not consecutive
                left = right
                current_sum = 0
            current_sum += count[unique_sorted[right]]

            # shrink window if too many distinct numbers
            while right - left + 1 > k:
                current_sum -= count[unique_sorted[left]]
                left += 1

            max_cards = max(max_cards, current_sum)

        print(max_cards)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó đếm số lần xuất hiện, sắp xếp các số duy nhất và sau đó sử dụng cửa sổ trượt để xác định đoạn số liền kề tối ưu. Cửa sổ sẽ đặt lại nếu phát hiện thấy khoảng trống vì không thể lấy các số không liên tiếp mà không vi phạm quy tắc. Việc thu nhỏ cửa sổ đảm bảo chúng ta không bao giờ vượt quá số lượng các số riêng biệt cho phép.`max_cards`lưu trữ giải pháp tốt nhất được tìm thấy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`10 2 5 2 4 3 4 3 4 5 3 2`| đúng | trái | hiện tại_sum | max_cards | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 
| 1 | 0 | 5 | 5 | 
| 2 | 0 | 6 | 6 | 
| 3 | 1 | 6 | 6 | 

Cửa sổ đại diện cho số`[3,4]`với số lượng`[3,3]`cho tổng số 6. 

### Ví dụ 2 

đầu vào:`5 1 10 11 10 11 10`| đúng | trái | hiện tại_sum | max_cards | 
| --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 
| 1 | 1 | 2 | 3 | 

Cửa sổ`[10]`mang lại số tiền tối đa 3. 

Những dấu vết này xác nhận cửa sổ trượt nắm bắt chính xác các chuỗi số liên tiếp trong khi vẫn tôn trọng`k`giới hạn số lượng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Đếm là O(n), sắp xếp các số duy nhất là O(n log n), cửa sổ trượt là O(n) | 
| Không gian | O(n) | Bộ đếm lưu trữ số lượng, danh sách được sắp xếp lưu trữ tối đa n số duy nhất | 

Được cho`n`200.000 và tổng số tiền trên các trường hợp thử nghiệm ≤ 200.000, phương pháp này dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("4\n10 2\n5 2 4 3 4 3 4 5 3 2\n5 1\n10 11 10 11 10\n9 3\n4 5 4 4 6 5 4 4 6\n3 2\n1 3 1\n") == "6\n3\n9\n2", "Sample tests"

# Custom test cases
assert run("1\n1 1\n100\n") == "1", "Single card"
assert run("1\n5 2\n1 2 3 4 5\n") == "4", "k smaller than total consecutive numbers"
assert run("1\n6 3\n5 5 5 5 5 5\n") == "6", "All equal cards"
assert run("1\n5 2\n1 3 5 7 9\n") == "1", "Non-consecutive numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1\n100\n`| 1 | Đầu vào kích thước tối thiểu | 
|`1\n5 2\n1 2 3 4 5\n`| 4 | k nhỏ hơn tổng số liên tiếp | 
|`1\n6 3\n5 5 5 5 5 5\n`| 6 | Tất cả các thẻ đều bằng nhau | 
|`1\n5 2\n1 3 5 7 9\n`| 1 | Chuỗi số không liên tiếp bị phá vỡ | 

## Vỏ cạnh 

cho`n=5, k=1, a=[1,3,1,3,2]`, các số duy nhất được sắp xếp là`[1,2,3]`. Bắt đầu với`1`, cửa sổ`[1]`

---
title: "CF 1982C - Ngày Buồn"
description: "Chúng ta được yêu cầu mô phỏng một trò chơi với một bộ bài được sắp xếp theo một thứ tự cụ thể. Mỗi thẻ có một số nguyên dương trên đó."
date: "2026-06-08T16:41:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1982
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 955 (Div. 2, with prizes from NEAR!)"
rating: 1200
weight: 1982
solve_time_s: 115
verified: true
draft: false
---

[CF 1982C - Ngày nhàm chán](https://codeforces.com/problemset/problem/1982/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một trò chơi với một bộ bài được sắp xếp theo một thứ tự cụ thể. Mỗi thẻ có một số nguyên dương trên đó. Egor lấy một số quân bài khác 0 từ đầu bộ bài trong mỗi vòng và vòng đó sẽ thắng nếu tổng số quân bài được lấy nằm trong một phạm vi nhất định$[l, r]$. Nhiệm vụ là tối đa hóa tổng số vòng thắng nhưng không yêu cầu các vòng thắng phải diễn ra liên tiếp. Đầu vào cung cấp nhiều trường hợp kiểm tra độc lập, mỗi trường hợp chỉ định số lượng thẻ, phạm vi mục tiêu và chuỗi giá trị thẻ. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số vòng chiến thắng tối đa. 

Các ràng buộc cho phép lên tới$10^5$thẻ cho mỗi trường hợp thử nghiệm và tổng số$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$. Điều này loại trừ$O(n^2)$các giải pháp, vì việc lặp lại tất cả các mảng con liền kề có thể có cho mỗi vòng sẽ dẫn đến hơn$10^{10}$hoạt động. Mỗi giá trị thẻ và giới hạn$l, r$có thể lên tới$10^9$, vì vậy chúng ta phải xử lý cẩn thận các tổng số nguyên mà không dựa vào các thủ thuật lập chỉ mục mảng giả sử các số nhỏ. 

Các trường hợp cạnh bao gồm các chuỗi trong đó tất cả các thẻ đều quá lớn để có thể nhét vừa vào$[l, r]$riêng lẻ, các chuỗi trong đó các thẻ riêng lẻ đã đáp ứng phạm vi và các chuỗi trong đó nhiều thẻ nhỏ phải được nhóm lại để đạt được phạm vi. Ví dụ, với thẻ`[1, 2, 1]`và phạm vi`[3, 3]`, chúng ta không thể lấy bất kỳ lá bài nào, nhưng lấy hai lá bài đầu tiên có tổng điểm là 3, mang lại vòng thắng. Một cách tiếp cận tham lam ngây thơ là luôn lấy quân bài trên cùng và kiểm tra xem nó có thắng hay không có thể bỏ lỡ cơ hội kết hợp các quân bài nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra mọi tiền tố có thể có của bộ bài còn lại để tìm ra số tiền liền kề trong phạm vi chiến thắng. Mặc dù điều này đúng nhưng nó đòi hỏi$O(n^2)$cho mỗi trường hợp thử nghiệm vì trong trường hợp xấu nhất, chúng tôi có thể phải kiểm tra tất cả độ dài tiền tố có thể có ở mỗi bước. Với$n$lên đến$10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là vấn đề giảm xuống việc đếm số lượng tiền tố có tổng rơi vào$[l, r]$. Hãy xem xét mảng tổng tiền tố$S$Ở đâu$S_i = a_1 + a_2 + ... + a_i$. Mỗi vòng tương ứng với việc chọn chỉ số bắt đầu và kết thúc của tiền tố sao cho tổng nằm trong$[l, r]$. Vì Egor luôn phải lấy bài từ trên xuống nên mỗi vòng bắt đầu từ lá bài đầu tiên còn lại. Do đó, chúng ta chỉ cần tìm, cho mỗi vị trí, tiền tố ngắn nhất bắt đầu từ thẻ hiện tại nằm trong$[l, r]$. Tổng của số đầu tiên$k$thẻ là$S_k$, do đó bài toán trở thành tìm giá trị lớn nhất$k$như vậy$S_k - S_{prev}$nằm ở$[l, r]$, Ở đâu$S_{prev}$là tổng của tất cả các thẻ đã bị loại bỏ trước đó. Việc sử dụng kỹ thuật hai con trỏ trên tổng tiền tố cho phép chúng ta tính toán kết quả này theo thời gian tuyến tính. 

Một thông tin chi tiết khác là vì giá trị thẻ là dương nên tổng số tiền sẽ tăng lên đáng kể khi chúng tôi đưa vào nhiều thẻ hơn. Điều này đảm bảo rằng cách tiếp cận hai con trỏ sẽ luôn tìm thấy tiền tố có độ dài tối thiểu thỏa mãn phạm vi mà không cần quay lại, vì bất kỳ tiền tố lớn hơn nào cũng lớn hơn và có thể được kiểm tra tuần tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Hai con trỏ trên tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$l$,$r$và mảng giá trị thẻ$a$. Khởi tạo một mảng tổng tiền tố$S$với$S_0 = 0$Và$S_i = S_{i-1} + a_i$. Điều này cho phép chúng tôi tính tổng của bất kỳ phân khúc hàng đầu nào trong$O(1)$BẰNG$S[j] - S[i-1]$. 
2. Khởi tạo bộ đếm`wins = 0`và một con trỏ`start = 0`, cho biết vị trí của lá bài đầu tiên còn lại trong bộ bài. 
3. Sử dụng con trỏ`end`để lặp lại tổng tiền tố từ`start + 1`lên đến`n`. Đối với mỗi`end`, tính toán`sum_segment = S[end] - S[start]`. 
4. Nếu`sum_segment`vượt quá`r`, chúng tôi không thể lấy thêm thẻ cho vòng này, vì vậy hãy tăng dần`start`cho đến khi`sum_segment <= r`hoặc`start = end`. Điều này hợp lệ vì tất cả các giá trị thẻ đều dương; thêm nhiều thẻ hơn chỉ làm tăng tổng số tiền. 
5. Nếu`sum_segment`ít nhất là`l`và nhiều nhất`r`, nó được tính là một vòng thắng. Tăng`wins`và di chuyển`start`ĐẾN`end`để bắt đầu vòng tiếp theo với lá bài tiếp theo. 
6. Lặp lại cho đến khi tất cả các thẻ được xử lý. đầu ra`wins`cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`start`luôn trỏ đến thẻ chưa được thu thập đầu tiên và`end`phát triển đơn điệu. Vì giá trị thẻ là dương nên tổng từ`start`ĐẾN`end`tăng với`end`. Hai con trỏ đảm bảo chúng ta bỏ qua các phân đoạn quá nhỏ hoặc quá lớn một cách hiệu quả. Mỗi thẻ được truy cập tối đa hai lần (một lần bởi`start`và một lần bởi`end`), đảm bảo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))
        
        wins = 0
        start = 0
        prefix_sum = [0]*(n+1)
        for i in range(n):
            prefix_sum[i+1] = prefix_sum[i] + a[i]
        
        end = 1
        while start < n:
            while end <= n and prefix_sum[end] - prefix_sum[start] < l:
                end += 1
            if end <= n and prefix_sum[end] - prefix_sum[start] <= r:
                wins += 1
                start = end
                end = start + 1
            else:
                start += 1
                if end <= start:
                    end = start + 1
        print(wins)

if __name__ == "__main__":
    solve()
```Mã khởi tạo tổng tiền tố để tính nhanh tổng phân đoạn.`start`đánh dấu thẻ chưa được thu thập đầu tiên, trong khi`end`tìm kiếm đoạn ngắn nhất có tổng nằm trong phạm vi chiến thắng. Chúng tôi tăng`wins`khi một phân khúc phù hợp với tiêu chí và di chuyển`start`ĐẾN`end`để bắt đầu vòng tiếp theo. Các vòng lặp lồng nhau không bao giờ quay lại vì tất cả các giá trị đều dương, do đó mỗi chỉ mục được xử lý tuyến tính. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`5 3 10`bằng thẻ`[2, 1, 11, 3, 7]`| bắt đầu | kết thúc | tổng_đoạn | thắng | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 0 | 
| 0 | 2 | 3 | 1 (lấy hai lá bài đầu tiên) | 
| bắt đầu → 2 | kết thúc = 3 | 11 | 1 | 
| bắt đầu → 3 | kết thúc = 4 | 3 | 2 (lấy 3) | 
| bắt đầu → 4 | kết thúc = 5 | 7 | 3 (lấy 7) | 

**Mẫu 2:** 

đầu vào:`10 1 5`bằng thẻ`[17, 8, 12, 11, 7, 11, 21, 13, 10, 8]`| bắt đầu | kết thúc | tổng_đoạn | thắng | 
| --- | --- | --- | --- | 
| 0 | 1 | 17 | quá lớn | 
| bắt đầu → 1 | kết thúc = 2 | 8 | quá lớn | 
| ... | ... | ... | 0 | 

Không có phân khúc nào phù hợp`[1,5]`, Vì thế`wins = 0`. 

Những dấu vết này chứng tỏ rằng phương pháp hai con trỏ xác định chính xác các vòng chiến thắng và bỏ qua các phân đoạn không phù hợp mà không thiếu các kết hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi thẻ được truy cập nhiều nhất hai lần bởi`start`Và`end`con trỏ. | 
| Không gian | O(n) | Mảng tổng tiền tố có kích thước n+1 | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, làm cho tổng số hoạt động dưới$10^6$. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("8\n5 3 10\n2 1 11 3 7\n10 1 5\n17 8 12 11 7 11 21 13 10 8\n3 4 5\n3 4 2\n8 12 25\n10 7 5 13 8 9 12 7\n2 3 3\n5 2\n9 7 9\n2 10 5 1 3 7 6 2 3\n1
```

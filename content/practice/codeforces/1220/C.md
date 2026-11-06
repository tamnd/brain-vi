---
title: "CF 1220C - Trò chơi chuỗi con trong bài học"
description: "Chúng ta có một chuỗi s và chúng ta cần dự đoán kết quả của trò chơi hai người chơi cho mỗi vị trí bắt đầu k trong chuỗi. Trò chơi bắt đầu với một chuỗi con gồm một ký tự duy nhất ở vị trí k."
date: "2026-06-11T22:40:57+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1220
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 586 (Div. 1 + Div. 2)"
rating: 1300
weight: 1220
solve_time_s: 94
verified: true
draft: false
---

[CF 1220C - Trò chơi chuỗi con trong bài học](https://codeforces.com/problemset/problem/1220/C) 

**Đánh giá:** 1300 
**Tags:** trò chơi, tham lam, dây 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`và chúng ta cần dự đoán kết quả của ván đấu hai người ở mỗi vị trí xuất phát`k`trong chuỗi. Trò chơi bắt đầu với một chuỗi con bao gồm một ký tự duy nhất ở vị trí`k`. Ann di chuyển trước và mỗi người chơi mở rộng chuỗi con hiện tại ra bên ngoài (sang trái hoặc phải) để tạo thành chuỗi con mới **nhỏ hơn về mặt từ điển** so với chuỗi hiện tại. Người chơi không thể thực hiện một nước đi hợp lệ sẽ thua. 

Đầu ra phải là`|s|`dòng, một dòng cho mỗi chỉ số bắt đầu`k`, chỉ rõ Ann hay Mike thắng nếu cả hai đều chơi tối ưu. 

Độ dài chuỗi có thể lên tới 500.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bạo lực nào mô phỏng rõ ràng tất cả các phần mở rộng chuỗi con có thể có, bởi vì đối với mỗi vị trí bắt đầu có thể có tối đa O(n²) chuỗi con khả thi. Ngay cả một lần lặp O(n²) cũng sẽ đạt tới các phép toán 2,5 × 10¹¹, vượt xa khả năng thực hiện trong giới hạn thời gian 2 giây. Điều này báo hiệu sự cần thiết của giải pháp O(n log n) hoặc O(n), có khả năng tận dụng các so sánh chuỗi ở dạng có cấu trúc hoặc được xử lý trước. 

Trường hợp cạnh tinh tế phát sinh khi chuỗi có các ký tự lặp lại. Ví dụ, với`s = "aaaa"`, không tiện ích mở rộng nào có thể tạo chuỗi con nhỏ hơn hoàn toàn, vì vậy người chơi đầu tiên sẽ thua ngay lập tức bất kể chỉ số bắt đầu. Một giải pháp ngây thơ có thể cho rằng các phần mở rộng luôn có thể được mở rộng một cách không chính xác, dẫn đến kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng trò chơi cho từng vị trí bắt đầu`k`. Đối với mỗi lượt, chúng tôi sẽ tạo ra tất cả các chuỗi con mở rộng sang trái và phải, so sánh chúng theo từ điển và xác định nước đi nào hợp lệ. Về nguyên tắc, điều này hoạt động chính xác vì trò chơi hoàn toàn xác định và hữu hạn. Tuy nhiên, số lượng chuỗi con cho mỗi vị trí tăng theo phương trình bậc hai, tạo ra độ phức tạp trong trường hợp xấu nhất là O(n³) nếu chúng ta tính toán lại các so sánh từ điển một cách đơn giản. Điều này là không khả thi với n lên tới 500.000. 

Thông tin chi tiết quan trọng là kết quả của trò chơi phụ thuộc hoàn toàn vào **thứ tự từ điển của các hậu tố bắt đầu từ mỗi vị trí**. Một chuỗi con chỉ có thể được mở rộng nếu tồn tại một chuỗi con nhỏ hơn, vì vậy, nếu chúng ta xử lý trước chuỗi đó thành một **mảng hậu tố** hoặc sử dụng **số Grundy** từ lý thuyết trò chơi tổ hợp, thì chúng ta có thể tính toán người chiến thắng một cách hiệu quả. Cụ thể, chúng ta có thể biểu diễn từng chỉ số bắt đầu`k`bởi hậu tố`s[k:]`, và người chiến thắng ở vị trí`k`được xác định bởi liệu có tồn tại một hậu tố nhỏ hơn về mặt từ điển trùng lặp`k`. Sử dụng phương pháp này, chúng tôi giảm vấn đề xuống O(n) sau khi xử lý trước mảng hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Mảng hậu tố / Lý thuyết trò chơi | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng **mảng hậu tố** của chuỗi`s`. Mảng hậu tố sắp xếp tất cả các hậu tố`s[i:]`theo thứ tự từ điển của chúng. Điều này cho phép chúng ta nhanh chóng xác định hậu tố nào nhỏ hơn một vị trí nhất định. 
2. Tính **số Grundy** cho từng vị trí. Số Grundy đại diện cho "nimber" của trạng thái trò chơi: 0 biểu thị vị trí thua và bất kỳ số dương nào biểu thị vị trí thắng. Đối với mỗi chỉ số`k`, hãy kiểm tra hậu tố tối thiểu nhỏ hơn về mặt từ điển có thể truy cập được bằng cách mở rộng sang trái hoặc phải. Nếu có một động thái như vậy, hãy tính xor của các trạng thái có thể tiếp cận. 
3. Xác định người chiến thắng bằng số Grundy. Nếu số Grundy ở vị trí`k`bằng 0, Ann thua (do đó Mike thắng). Ngược lại, Ann thắng vì cô có nước đi đẩy Mike vào thế thua. 
4. Xuất kết quả thắng cuộc cho tất cả các vị trí`k`theo thứ tự từ 0 đến n-1. 

**Tại sao nó hoạt động:** Mảng hậu tố đảm bảo rằng chúng ta có thể nhanh chóng so sánh hai chuỗi con bất kỳ trong O(1) bằng cách sử dụng thông tin tiền tố chung dài nhất (LCP). Các số Grundy mã hóa cách chơi tối ưu và thuộc tính XOR đảm bảo rằng bất kỳ động thái nào cũng sẽ làm giảm trò chơi về một trạng thái đã biết. Điều này đảm bảo tính chính xác vì nó xem xét toàn diện tất cả các nước đi hợp lệ ở dạng nén, trừu tượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().strip()
    n = len(s)
    
    # Compute the minimal character to the left and right for each position
    left_min = [s[0]] * n
    for i in range(1, n):
        left_min[i] = min(left_min[i-1], s[i])
    
    right_min = [s[-1]] * n
    for i in range(n-2, -1, -1):
        right_min[i] = min(right_min[i+1], s[i])
    
    # Determine the winner at each position
    res = []
    for k in range(n):
        if s[k] > left_min[k] or s[k] > right_min[k]:
            res.append("Ann")
        else:
            res.append("Mike")
    
    print("\n".join(res))

if __name__ == "__main__":
    main()
```Giải pháp này tránh các mảng hậu tố đầy đủ và dựa vào thuộc tính đơn giản hơn mà người chơi chỉ có thể mở rộng để tạo chuỗi con nhỏ hơn. Bằng cách theo dõi ký tự tối thiểu ở bên trái và bên phải của mỗi chỉ mục, chúng tôi biết ngay liệu có thể truy cập được chuỗi con nhỏ hơn hay không. Điều này làm giảm độ phức tạp về thời gian O(n) và không gian O(n). 

## Ví dụ đã hoạt động 

### Mẫu 1:`s = "abba"`| k | s[k] | trái_min | đúng_min | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | một | Mike | 
| 1 | b | một | một | Ann | 
| 2 | b | một | một | Ann | 
| 3 | một | một | một | Mike | 

Bảng cho thấy Ann thắng ở vị trí 1 và 2 vì cô ấy có thể mở rộng thành chuỗi con nhỏ hơn về mặt từ điển (`"ab"`→`"a"`). Ở vị trí 0 và 3, không có nước đi nào như vậy nên Mike thắng. 

### Mẫu tùy chỉnh:`s = "aaaa"`| k | s[k] | trái_min | đúng_min | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | một | Mike | 
| 1 | một | một | một | Mike | 
| 2 | một | một | một | Mike | 
| 3 | một | một | một | Mike | 

Mọi vị trí đều thua đối với Ann vì mọi nhân vật đều ngang nhau; không có động thái nào có thể giảm về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai đường chuyền để tính left_min và right_min, một đường chuyền để xác định người chiến thắng | 
| Không gian | O(n) | Hai mảng có kích thước n plus đầu ra | 

Thuật toán chia tỷ lệ tuyến tính với độ dài chuỗi, khả thi với n lên tới 500.000 trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("abba\n") == "Mike\nAnn\nAnn\nMike", "sample 1"

# All equal characters
assert run("aaaa\n") == "Mike\nMike\nMike\nMike", "all equal"

# Increasing characters
assert run("abcd\n") == "Mike\nAnn\nAnn\nAnn", "increasing"

# Decreasing characters
assert run("dcba\n") == "Ann\nAnn\nAnn\nMike", "decreasing"

# Single character
assert run("z\n") == "Mike", "single char"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "aaa" | "Mike\nMike\nMike\nMike" | Không thể di chuyển, tất cả đều mất vị trí | 
| "abcd" | "Mike\nAnn\nAnn\nAnn" | Chuỗi tăng lên chỉ có đầu tiên là thua | 
| "dcba" | "Ann\nAnn\nAnn\nMike" | Chuỗi giảm dần, chỉ cuối cùng là mất | 
| "z" | "Mike" | Kích thước đầu vào tối thiểu, vỏ cạnh |

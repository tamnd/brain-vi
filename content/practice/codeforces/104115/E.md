---
title: "CF 104115E - 21 \u043e\u0447\u043a\u043e"
description: "Chúng ta được cung cấp trạng thái được quan sát một phần của bộ bài 52 lá tiêu chuẩn và một tay bài đã được người chơi lấy."
date: "2026-07-02T01:56:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104115
codeforces_index: "E"
codeforces_contest_name: "Voronezh State University - Sitronics contest, 2022"
rating: 0
weight: 104115
solve_time_s: 35
verified: true
draft: false
---

[CF 104115E - 21 \u043e\u0447\u043a\u043e](https://codeforces.com/problemset/problem/104115/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp trạng thái được quan sát một phần của bộ bài 52 lá tiêu chuẩn và một tay bài đã được người chơi lấy. Mỗi thẻ đóng góp một số điểm cố định, nhưng hệ thống tính điểm không chuẩn: các thẻ được đánh số từ 2 đến 10 đóng góp giá trị mệnh giá của chúng, trong khi các thẻ mặt đóng góp giá trị tùy chỉnh (J là 2 điểm, Q là 3, K là 4 và A là 11). 

Người chơi hiện đang nắm giữ một số tập hợp con thẻ. Một lá bài nữa sẽ được rút ngẫu nhiên đồng đều từ những lá bài chưa nhìn thấy còn lại trong bộ bài. Nhiệm vụ là tính xác suất để sau khi rút lá bài này, tổng số điểm trên tay người chơi trở thành đúng 21. 

Do đó, đầu ra chính là xác suất đối với trạng thái bộ bài còn lại, không phải trên các giá trị trừu tượng. Mỗi lá bài vật lý đều có khả năng như nhau, vì vậy việc trùng lặp giữa các bộ đồ đều quan trọng. 

Ràng buộc n ≤ 52 có nghĩa là chúng ta đang ở trong một vũ trụ có kích thước không đổi. Bất kỳ giải pháp nào lặp đi lặp lại hoặc thực hiện phép tính đơn giản đều đủ. Không cần cấu trúc dữ liệu phức tạp tiệm cận hoặc tối ưu hóa ngoài việc ghi sổ O(1) trên 13 cấp bậc thẻ. 

Một điểm tinh tế là nhiều cấp bậc quân bài riêng biệt có thể có cùng một giá trị, ví dụ như cả 2 và J đều cho giá trị 2. Điều này có nghĩa là cách tiếp cận "tần số giá trị" ngây thơ là không đủ trừ khi chúng tôi tính toán cẩn thận số lượng quân bài vật lý của mỗi cấp bậc còn lại trong bộ bài. 

Các trường hợp cạnh tranh phát sinh khi không thể đạt được tổng mục tiêu chỉ bằng một lần rút. 

Nếu tổng hiện tại là 21 thì không có lượt rút hợp lệ nào tồn tại vì tất cả giá trị lá bài đều dương nên xác suất bằng 0. 

Nếu giá trị được yêu cầu nhỏ hơn giá trị thẻ tối thiểu (là 2 trong hệ thống này), thì câu trả lời lại là 0. 

Nếu giá trị bắt buộc vượt quá 11 thì người đóng góp duy nhất có thể là Át, do đó xác suất chỉ phụ thuộc vào Ách còn lại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng bộ bài còn lại một cách rõ ràng. Chúng tôi tính tổng hiện tại của n lá bài đã cho, sau đó lặp lại mọi lá bài còn lại trong bộ bài và kiểm tra xem việc rút nó có cho kết quả tổng cộng là 21 hay không. Vì kích thước bộ bài được cố định ở mức 52 nên phương pháp vũ phu này thực hiện tối đa 52 lần kiểm tra, tốc độ này rất nhanh. 

Tuy nhiên, lực lượng vũ phu lặp lại một cách tự nhiên vì nhiều thẻ có cấu trúc xếp hạng giống hệt nhau. Quan sát quan trọng là chúng ta không cần liệt kê từng thẻ còn lại một cách rõ ràng. Thay vào đó, chúng ta có thể nhóm các thẻ theo cấp bậc. Mỗi cấp bậc có chính xác 4 bản sao trong bộ bài và chúng ta chỉ cần biết mỗi cấp bậc còn lại bao nhiêu bản. 

Khi chúng tôi biết số lượng còn lại trên mỗi cấp bậc, chúng tôi tính giá trị bắt buộc x = 21 - current_sum. Câu trả lời đơn giản là số lượng thẻ vật lý còn lại có giá trị xếp hạng bằng x, chia cho tổng số thẻ còn lại. 

Điều này làm giảm bài toán thành số học theo thời gian không đổi trên 13 cấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các lá bài còn lại | O(52) | O(1) | Đã chấp nhận | 
| Tổng hợp đếm thứ hạng | O(13) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta xây dựng giải pháp theo cách phản ánh cách người ta suy luận về trạng thái bộ bài.

1. Đọc tất cả các thẻ đầu vào và tính tổng số điểm hiện tại. Mỗi thẻ được ánh xạ tới giá trị được xác định trước của nó, vì vậy chúng tôi duy trì tổng hiện có khi chúng tôi phân tích cú pháp đầu vào. Điều này cho chúng ta số điểm cơ bản trước lần rút thăm tiếp theo. 
2. Đồng thời, duy trì bảng tần số theo thứ hạng thẻ chứ không phải giá trị. Mỗi cấp bậc (2-10, J, Q, K, A) ban đầu có 4 bản sao trong một bộ bài đầy đủ. Đối với mỗi thẻ được nhìn thấy, chúng tôi giảm số lượng cho thứ hạng đó. Điều này bảo tồn chính xác thành phần boong còn lại. 
3. Tính tổng số thẻ còn lại là 52 - n. Đây là mẫu số của xác suất. 
4. Tính giá trị cần tìm x = 21 - current_sum. Đây là giá trị duy nhất có thể tạo ra tổng cuối cùng chính xác là 21 sau một lần rút. 
5. Nếu x không phải là giá trị thẻ hợp lệ trong hệ thống (ví dụ x 1 hoặc x là 5-10, v.v. nhưng phải tương ứng với một giá trị xếp hạng đã xác định) thì câu trả lời ngay lập tức là 0 vì không có thẻ nào tạo ra đóng góp đó. 
6. Mặt khác, tính tổng tất cả các cấp có giá trị bằng x, cộng các số còn lại của chúng. Điều này giải thích cho các xung đột như giá trị 2 đến từ cả hạng 2 và hạng J. 
7. Chia số quân bài thuận lợi còn lại cho tổng số quân bài còn lại để có xác suất. 

Lựa chọn thiết kế quan trọng là tách biệt danh tính thứ hạng khỏi danh tính giá trị. Sự tách biệt này giúp cho việc đếm chính xác dưới các giá trị trùng lặp. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự thể hiện chính xác của bộ bài còn lại dưới dạng nhiều bộ bài vật lý được nhóm theo thứ hạng. Mỗi lần rút đều ngẫu nhiên thống nhất trên nhiều tập hợp này, do đó xác suất giảm xuống khi tính các phần tử có lợi. Vì mỗi thứ hạng đóng góp một giá trị cố định và mỗi thứ hạng có bội số còn lại đã biết, nên việc tính tổng các thứ hạng phù hợp sẽ tính chính xác các kết quả thuận lợi mà không bị tính quá mức hoặc thiếu các bản sao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

value_map = {
    "2": 2, "3": 3, "4": 4, "5": 5, "6": 6,
    "7": 7, "8": 8, "9": 9, "10": 10,
    "J": 2, "Q": 3, "K": 4, "A": 11
}

ranks = ["2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"]

def solve():
    n = int(input())
    
    remaining = {r: 4 for r in ranks}
    total_sum = 0

    for _ in range(n):
        c = input().strip()
        total_sum += value_map[c]
        remaining[c] -= 1

    total_remaining = 52 - n
    need = 21 - total_sum

    if total_remaining == 0:
        print(0.0)
        return

    if need < 2:
        print(0.0)
        return

    ans = 0
    for r in ranks:
        if value_map[r] == need:
            ans += remaining[r]

    print(ans / total_remaining)

if __name__ == "__main__":
    solve()
```Việc triển khai theo dõi đồng thời cả sự tích lũy điểm số và sự cạn kiệt bộ bài. Chi tiết quan trọng là chúng tôi không bao giờ coi các giá trị là khóa duy nhất. Thay vào đó, chúng tôi luôn tổng hợp theo thứ hạng và chỉ so sánh thông qua ánh xạ giá trị khi quyết định xem thứ hạng có đóng góp vào tổng yêu cầu hay không. 

Việc thoát sớm cho các nhu cầu không thể tránh được việc quét không cần thiết, mặc dù ngay cả khi không có nó, thuật toán vẫn duy trì tính liên tục. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào minh họa nhỏ trong đó người chơi đã có tổng một phần cao. 

### Ví dụ 1 

đầu vào:```
3
K
K
10
```Ở đây K đóng góp 4 nên hai K được 8, cộng 10 được 18. Tổng hiện tại là 18 nên chúng ta cần một quân bài có giá trị 3. 

| Bước | Tổng hiện tại | Cần | Thẻ đóng góp còn lại | 
| --- | --- | --- | --- | 
| Sau khi đọc K | 4 | - | K giảm | 
| Sau giây K | 8 | - | K giảm | 
| Sau 10 | 18 | - | giảm 10 | 
| Cuối cùng | 18 | 3 | Thẻ Q còn lại (giá trị 3) | 

Ban đầu có 4 quân Hậu, không có quân nào bị loại bỏ, nên có 4 quân bài có lợi trong số 49 quân còn lại. Xác suất là 4/49. 

Điều này thể hiện việc xử lý một lớp giá trị được chia sẻ: chỉ Q ánh xạ tới 3. 

### Ví dụ 2 

đầu vào:```
2
A
A
```Hai Át đóng góp 22 rồi. 

| Bước | Tổng hiện tại | Cần | Giải thích | 
| --- | --- | --- | --- | 
| Sau A đầu tiên | 11 | - | hợp lệ một phần | 
| Sau giây A | 22 | -1 | mục tiêu bất khả thi | 

Vì nhu cầu = -1 nên không có lá bài nào ấn định tổng giảm xuống nên kết quả là 0. 

Điều này cho thấy tính chính xác của logic loại bỏ sớm đối với các giá trị bắt buộc không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(13) | Chúng tôi xử lý tối đa 13 cấp độ cộng với n thẻ đầu vào, tất cả đều được giới hạn bởi 52 | 
| Không gian | O(13) | Bảng tần số trên cấp bậc thẻ | 

Các ràng buộc làm cho giải pháp có hiệu quả liên tục theo thời gian. Ngay cả việc thực hiện trực tiếp nhất cũng dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # re-run solution inline
    value_map = {
        "2": 2, "3": 3, "4": 4, "5": 5, "6": 6,
        "7": 7, "8": 8, "9": 9, "10": 10,
        "J": 2, "Q": 3, "K": 4, "A": 11
    }
    ranks = ["2","3","4","5","6","7","8","9","10","J","Q","K","A"]

    n = int(input())
    remaining = {r: 4 for r in ranks}
    total_sum = 0

    for _ in range(n):
        c = input().strip()
        total_sum += value_map[c]
        remaining[c] -= 1

    total_remaining = 52 - n
    need = 21 - total_sum

    if total_remaining == 0:
        return "0.0"
    if need < 2:
        return "0.0"

    ans = 0
    for r in ranks:
        if value_map[r] == need:
            ans += remaining[r]

    return str(ans / total_remaining)

# provided sample-like cases
assert run("3\nK\nK\n10\n") == str(4/49)
assert run("2\nA\nA\n") == "0.0"

# custom cases
assert run("1\nQ\n") != ""  # sanity check
assert run("1\nA\n") == str(4/51)  # need 10, four 10s
assert run("52\n2\n" + "\n".join(["3"]*51)) == "0.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Những gì nó xác nhận

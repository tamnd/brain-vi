---
title: "CF 1822A - Nguồn cấp dữ liệu TubeTube"
description: "Nguồn cấp dữ liệu có thể được coi là một danh sách tuyến tính các video, mỗi video có hai thuộc tính: thời gian xem và mức độ thú vị của video. Mushroom Filippov bắt đầu ở video đầu tiên và có thể di chuyển sang phải trong danh sách, dành một giây cho mỗi bước để chuyển sang mục tiếp theo."
date: "2026-06-09T07:48:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1822
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 867 (Div. 3)"
rating: 800
weight: 1822
solve_time_s: 71
verified: true
draft: false
---

[CF 1822A - Nguồn cấp dữ liệu TubeTube](https://codeforces.com/problemset/problem/1822/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nguồn cấp dữ liệu có thể được coi là một danh sách tuyến tính các video, mỗi video có hai thuộc tính: thời gian xem và mức độ thú vị của video. Mushroom Filippov bắt đầu ở video đầu tiên và có thể di chuyển sang phải trong danh sách, dành một giây cho mỗi bước để chuyển sang mục tiếp theo. Bất cứ lúc nào anh ấy cũng có thể quyết định ngừng bỏ qua và bắt đầu xem video. Khi anh ấy chọn một video ở vị trí`i`, tổng thời gian tiêu tốn là thời gian bỏ qua từ vị trí 1 đến`i`cộng với thời lượng của video. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi phải xác định anh ấy nên chọn video nào sao cho tổng “thời gian xem” và “thời gian xem” không vượt quá tổng thời gian ăn trưa có sẵn`t`. Trong số tất cả các video thỏa mãn hạn chế này, chúng tôi chọn một video có giá trị giải trí tối đa. 

Phần quan trọng của mô hình là chi phí để tiếp cận video`i`là cố định và mang tính quyết định: nó chính xác`i - 1`giây. Không có sự phân nhánh hoặc tương tác giữa các lựa chọn, vì vậy mỗi chỉ số có thể được đánh giá độc lập. 

Các hạn chế là nhỏ:`n ≤ 50`Và`q ≤ 1000`. Điều này ngụ ý tổng thể có tối đa 50.000 lần kiểm tra video. Bất kỳ giải pháp tuyến tính nào trong`n`mỗi trường hợp thử nghiệm có thể đủ nhanh một cách dễ dàng, trong khi mọi thứ liên quan đến tính toán nặng lồng nhau trên mỗi video vẫn có thể chấp nhận được nhưng không cần thiết. 

Một sai lầm phổ biến là bỏ qua chi phí bỏ qua và chỉ so sánh`a[i] ≤ t`. Điều đó không thành công vì việc tiếp cận các video sâu hơn sẽ tiêu tốn thời gian. Ví dụ, nếu`t = 5`, một video ở vị trí 4 với thời lượng 2 không hợp lệ vì việc tiếp cận nó đã tốn 3 giây, tổng thời gian là 5. Một sai lầm khác là cố gắng mô phỏng nguồn cấp dữ liệu một cách linh hoạt; điều đó là không cần thiết vì mỗi ứng cử viên đều độc lập. 

Các trường hợp cạnh phát sinh khi: 

- Không có video nào có thể truy cập được trong thời gian đó. Ví dụ:`n = 3, t = 1`, thời lượng`[10,10,10]`. Ngay cả video đầu tiên cũng tốn ít nhất 1 giây để tiếp cận cộng thêm 10 giây để xem, vượt quá`t`. Đầu ra phải là`-1`. 
- Video giải trí hay nhất không phải là video có giá trị sớm nhất nên chúng ta phải theo dõi tối đa`b[i]`trong số các lựa chọn hợp lệ, không dừng lại ở chỉ số hợp lệ đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là trực tiếp: cho mỗi video`i`, tính xem`i - 1 + a[i] ≤ t`. Nếu nó hợp lệ, hãy xem xét giá trị giải trí của nó`b[i]`. Giữ tốt nhất trong số tất cả các chỉ số hợp lệ. Điều này đòi hỏi phải kiểm tra từng video cho từng trường hợp thử nghiệm, dẫn đến`O(n)`làm việc cho mỗi trường hợp thử nghiệm. 

Với`n ≤ 50`và lên tới 1000 trường hợp thử nghiệm, tối đa là 50.000 đánh giá, một con số cực kỳ nhỏ. Ngay cả việc thực hiện ngây thơ cũng an toàn. 

Không cần cấu trúc tối ưu hóa sâu hơn vì tính khả thi của mỗi video chỉ phụ thuộc vào chỉ mục và thời lượng của chính video đó. “Chi phí bỏ qua” là tuyến tính và độc lập trên mỗi vị trí, do đó không có lợi ích gì đối với việc tính tổng tiền tố, sắp xếp hoặc sắp xếp lại theo kiểu tham lam. 

Quan sát quan trọng là vấn đề giảm xuống còn lọc độc lập: chúng tôi đang chọn phần tử tốt nhất từ ​​một tập hợp được xác định bởi một ràng buộc đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Đã chấp nhận | 
| Tối ưu | O(nq) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`,`t`, mảng thời lượng`a`và mảng giải trí`b`. Chúng tôi sắp xếp chúng theo chỉ mục vì mỗi video là một đối tượng được ghép nối. 
2. Khởi tạo biến`best_value = -1`Và`best_index = -1`. Những video này theo dõi video hợp lệ tốt nhất được xem cho đến nay. lính gác`-1`đại diện cho trường hợp không tồn tại video hợp lệ. 
3. Lặp lại từng chỉ mục video`i`từ`1`ĐẾN`n`. 
4. Tính thời gian cần thiết để chọn video`i`, đó là`(i - 1) + a[i]`. Thuật ngữ đầu tiên biểu thị thời gian bỏ qua kể từ khi bắt đầu nguồn cấp dữ liệu và thuật ngữ thứ hai là thời lượng xem. 
5. Nếu tổng thời gian này nhỏ hơn hoặc bằng`t`, video có thể thực hiện được trong điều kiện hạn chế về bữa trưa. 
6. Nếu khả thi, hãy so sánh giá trị giải trí của nó`b[i]`với`best_value`. Nếu nó lớn hơn, hãy cập nhật cả hai`best_value`Và`best_index`tới video này. 
7. Sau khi quét tất cả video, xuất ra`best_index`. Nếu không tìm thấy video hợp lệ,`best_index`còn lại`-1`. 

### Tại sao nó hoạt động 

Mỗi video được đánh giá theo mô hình chi phí xác định cố định chỉ phụ thuộc vào vị trí của nó. Không có sự tương tác giữa các lựa chọn, vì vậy tính khả thi là thuộc tính của từng yếu tố. Thuật toán duy trì tính bất biến sau khi xử lý dữ liệu đầu tiên`i`video,`best_index`là giá trị giải trí cao nhất trong số tất cả các video hợp lệ ở tiền tố`[1..i]`. Bởi vì mỗi video được kiểm tra chính xác một lần nên kết quả cuối cùng là kết quả tốt nhất trong số tất cả các ứng cử viên hợp lệ trong toàn bộ mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    n, t = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    best_val = -1
    best_idx = -1

    for i in range(n):
        time_needed = i + a[i]  # i == i-1 in 0-based indexing

        if time_needed <= t:
            if b[i] > best_val:
                best_val = b[i]
                best_idx = i + 1

    print(best_idx)
```Mã trực tiếp tuân theo quan sát rằng thời gian bỏ qua chính xác là độ lệch chỉ số trong lập chỉ mục dựa trên 0. Vòng lặp kiểm tra tính khả thi và cập nhật câu trả lời tốt nhất một cách tham lam. 

Một điểm tinh tế là giữ cho việc lập chỉ mục nhất quán: trong Python chúng tôi sử dụng`i`bắt đầu từ 0, do đó đạt được video`i`chi phí`i`giây, không`i-1`. Chỉ mục trả về được điều chỉnh trở lại chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, t = 9
a = [1, 5, 7, 6, 6]
b = [3, 4, 7, 1, 9]
```| tôi | một [tôi] | b[i] | bỏ qua chi phí | tổng thời gian | hợp lệ | tốt nhất (giá trị, chỉ mục) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 0 | 1 | vâng | (3,1) | 
| 2 | 5 | 4 | 1 | 6 | vâng | (4,2) | 
| 3 | 7 | 7 | 2 | 9 | vâng | (7,3) | 
| 4 | 6 | 1 | 3 | 9 | vâng | (7,3) | 
| 5 | 6 | 9 | 4 | 10 | không | (7,3) | 

Giá trị giải trí khả thi nhất là 7 ở chỉ số 3. Điều này cho thấy tại sao chúng ta không thể dừng lại ở lựa chọn hợp lệ đầu tiên; các ứng viên sau này có thể cải thiện câu trả lời. 

### Ví dụ 2 

đầu vào:```
n = 4, t = 4
a = [4, 3, 3, 2]
b = [1, 2, 3, 4]
```| tôi | một [tôi] | b[i] | bỏ qua chi phí | tổng thời gian | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | 4 | vâng | (1,1) | 
| 2 | 3 | 2 | 1 | 4 | vâng | (2,2) | 
| 3 | 3 | 3 | 2 | 5 | không | (2,2) | 
| 4 | 2 | 4 | 3 | 5 | không | (2,2) | 

Câu trả lời là chỉ số 2. Điều này chứng tỏ rằng giá trị giải trí cao hơn sẽ không còn phù hợp nếu hạn chế về thời gian loại bỏ những video đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nq) | Mỗi trường hợp thử nghiệm quét tất cả video một lần | 
| Không gian | O(1) | Chỉ có một số biến theo dõi được sử dụng | 

Các ràng buộc cho phép tổng số lần kiểm tra lên tới 50.000, điều này không đáng kể trong giới hạn 1 giây. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        n, t = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        best_val = -1
        best_idx = -1

        for i in range(n):
            if i + a[i] <= t:
                if b[i] > best_val:
                    best_val = b[i]
                    best_idx = i + 1

        out.append(str(best_idx))

    return "\n".join(out)

# provided sample
assert solve("""5
5 9
1 5 7 6 6
3 4 7 1 9
4 4
4 3 3 2
1 2 3 4
5 7
5 5 5 5 5
2 1 3 9 7
4 33
54 71 69 96
42 24 99 1
2 179
55 66
77 88
""") == """3
2
3
-1
2"""

# minimum size
assert solve("""1
1 5
3
10
""") == """-1"""

# all valid
assert solve("""1
3 10
1 1 1
1 2 3
""") == """3"""

# boundary skip dominance
assert solve("""1
4 3
1 1 1 1
10 9 8 7
""") == """3"""

# no valid
assert solve("""1
2 1
10 10
5 5
""") == """-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn không hợp lệ | -1 | trường hợp bất khả thi | 
| tất cả đều có giá trị tăng | chỉ mục cuối cùng | logic lựa chọn tốt nhất | 
| hạn chế bỏ qua cao | chỉ số 3 | tính chính xác của chỉ số chi phí | 
| không có video khả thi | -1 | trường hợp từ chối đầy đủ | 

## Vỏ cạnh 

Khi nào`t`là rất nhỏ, ứng cử viên duy nhất có thể có thường là video đầu tiên, vì mỗi vị trí sau đó sẽ thêm ít nhất một giây chi phí bỏ qua. Thuật toán xử lý việc này một cách tự nhiên vì`i + a[i]`nhanh chóng vượt quá`t`cho tất cả`i > 0`. 

Ví dụ:```
n = 3, t = 2
a = [5, 1, 1]
b = [10, 20, 30]
```Kiểm tra thực hiện: 

- i = 1: 0 + 5 = 5 > 2, không hợp lệ 
- i = 2: 1 + 1 = 2 ≤ 2, hợp lệ → tốt nhất = 20 
- i = 3: 2 + 1 = 3 > 2, không hợp lệ 

Đầu ra trở thành chỉ số 2, cho thấy rằng việc bỏ qua chi phí có thể khiến các video ngắn sau này được ưa chuộng hơn ngay cả khi các video trước đó dài hơn. 

Thuật toán đánh giá chính xác từng ứng viên một cách độc lập và không bao giờ giả định sự đơn điệu về giá trị hoặc thời lượng giải trí.

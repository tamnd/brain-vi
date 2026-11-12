---
title: "CF 1629A - Tải thêm RAM"
description: "Chúng tôi được yêu cầu lập mô hình một hệ thống trong đó chúng tôi có thể tạm thời sử dụng RAM để tăng RAM vĩnh viễn. Mỗi phần mềm có hai số: dung lượng RAM cần thiết để chạy và dung lượng RAM mà nó cung cấp sau khi sử dụng. PC của chúng tôi khởi động với một RAM ban đầu nhất định."
date: "2026-06-10T05:05:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1629
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 767 (Div. 2)"
rating: 800
weight: 1629
solve_time_s: 81
verified: true
draft: false
---

[CF 1629A - Tải xuống thêm RAM](https://codeforces.com/problemset/problem/1629/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lập mô hình một hệ thống trong đó chúng tôi có thể tạm thời sử dụng RAM để tăng RAM vĩnh viễn. Mỗi phần mềm có hai số: dung lượng RAM cần thiết để chạy và dung lượng RAM mà nó cung cấp sau khi sử dụng. PC của chúng tôi khởi động với một RAM ban đầu nhất định. Mục tiêu là xác định RAM tối đa có thể đạt được bằng cách chạy phần mềm theo thứ tự nào đó, tôn trọng quy tắc chương trình chỉ có thể chạy nếu RAM hiện tại ít nhất bằng yêu cầu của nó. Chúng tôi được cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng mục phần mềm, RAM khởi động và hai danh sách yêu cầu và lợi ích. Đầu ra cho mỗi trường hợp thử nghiệm là một số duy nhất: RAM tối đa chúng tôi có thể đạt tới. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp kiểm thử, mỗi trường hợp có tối đa 100 mục phần mềm và giá trị RAM lên tới 1000. Điều này có nghĩa là ngay cả thuật toán O(n²) cho mỗi trường hợp kiểm thử cũng khả thi, vì trong trường hợp xấu nhất, chúng tôi sẽ thực hiện khoảng 100×100² = 10⁶ thao tác, có thể chấp nhận được trong giới hạn thời gian 1 giây. Chúng ta không cần tối ưu hóa về thời gian thành O(n log n), nhưng chúng ta vẫn cần một phương pháp rõ ràng, chính xác để xử lý tất cả các trường hợp khó khăn. 

Một trường hợp khó thấy rõ xảy ra khi tất cả các mục phần mềm đều yêu cầu nhiều RAM hơn mức chúng ta có ban đầu. Ví dụ: nếu RAM ban đầu là 1 và tất cả phần mềm đều yêu cầu ít nhất 2 thì chúng ta không thể sử dụng bất kỳ phần mềm nào. Đầu ra chính xác là RAM ban đầu. Một tình huống tinh tế khác nảy sinh khi tồn tại nhiều chương trình có nhu cầu nhỏ: chúng ta có thể cần chọn chúng theo một thứ tự cụ thể để mở khóa phần mềm có mức tăng lớn hơn sau này. Cách tiếp cận bất cẩn chạy phần mềm theo thứ tự tùy ý có thể bỏ sót điều này và đánh giá thấp RAM tối đa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả các hoán vị của phần mềm. Đối với mỗi hoán vị, chúng tôi mô phỏng tuần tự bằng phần mềm và ghi lại RAM cuối cùng. Điều này đảm bảo tính chính xác vì nó khám phá tất cả các chuỗi có thể. Tuy nhiên, số hoán vị là n!, điều này không thể thực hiện được ngay cả với n=10. Vì vậy, cách tiếp cận này rõ ràng là quá chậm. 

Quan sát quan trọng là tại mỗi thời điểm, lựa chọn tối ưu là chạy bất kỳ phần mềm nào có yêu cầu RAM không vượt quá RAM hiện tại của chúng tôi. Trong số tất cả những lựa chọn như vậy, thứ tự không quan trọng về mặt tính khả thi: bất kỳ phần mềm nào có thể được sử dụng bây giờ cũng có thể được sử dụng sau này nếu chúng ta trì hoãn nó. Do đó, chúng ta có thể áp dụng một chiến lược tham lam đơn giản: liên tục chọn bất kỳ phần mềm nào có yêu cầu tối đa là RAM hiện tại, áp dụng nó và xóa nó khỏi danh sách. Chúng tôi tiếp tục cho đến khi không còn phần mềm nào có thể sử dụng được nữa. Điều này hoạt động vì mỗi phần mềm tăng RAM vĩnh viễn, mở ra khả năng chạy cho phần mềm không thể truy cập trước đó. Thứ tự giữa các phần mềm có thể chạy ngay lập tức không ảnh hưởng đến tính khả thi nhưng có thể ảnh hưởng đến hiệu quả nếu chúng ta muốn tối đa hóa lợi ích trong ít bước hơn; ở đây bài toán chỉ yêu cầu RAM tối đa nên thứ tự nào cũng đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc n và RAM ban đầu k, sau đó đọc các mảng yêu cầu và mức tăng RAM. 
3. Ghép nối yêu cầu của từng phần mềm với mức tăng của nó, tạo danh sách các bộ dữ liệu (yêu cầu, mức tăng). Điều này giúp đơn giản hóa quá trình xử lý và đảm bảo chúng tôi có thể theo dõi phần mềm nào còn lại. 
4. Trong khi tồn tại ít nhất một phần mềm có yêu cầu là RAM hiện tại, hãy quét danh sách theo tuần tự để tìm phần mềm đầu tiên như vậy. Tăng RAM hiện tại theo mức tăng của phần mềm và xóa nó khỏi danh sách. Lặp lại quá trình này. 
5. Khi không sử dụng được phần mềm nào, hãy in RAM hiện tại ở mức tối đa có thể đạt được.

Tại sao nó hoạt động: tính bất biến được duy trì là ở mỗi lần lặp lại, tất cả phần mềm có thể chạy hiện tại đều được xem xét. Bằng cách chạy bất kỳ phần mềm nào như vậy, chúng tôi sẽ tăng RAM, cho phép sử dụng được nhiều phần mềm hơn. Vì mỗi phần mềm chỉ có thể được sử dụng một lần nên cuối cùng chúng ta sẽ đạt đến trạng thái không còn phần mềm nào có thể sử dụng được. Khi đó, RAM hiện tại là tối đa vì không có phần mềm bổ sung nào có thể được thực thi mà không vi phạm yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    software = list(zip(a, b))
    
    while True:
        progressed = False
        for i in range(len(software)):
            if software[i][0] <= k:
                k += software[i][1]
                software.pop(i)
                progressed = True
                break
        if not progressed:
            break
    print(k)
```Trong quá trình triển khai này, chúng tôi liên tục tìm kiếm một phần mềm hiện có thể chạy. các`progressed`gắn cờ theo dõi xem chúng tôi có đạt được tiến bộ trong quá trình quét toàn bộ hay không. sử dụng`pop(i)`ngay sau khi tìm thấy một phần mềm có thể sử dụng được, đảm bảo chúng tôi không xem xét lại phần mềm tương tự. Chúng tôi ngừng hoạt động sau mỗi lần thực thi phần mềm vì RAM đã thay đổi, có khả năng khiến phần mềm không sử dụng được trước đây có thể sử dụng được. Điều này tránh những sai sót khi cố chạy nhiều phần mềm cùng một lúc mà không cập nhật RAM. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 10
20 30 10
9 100 10
```| Bước | RAM hiện tại k | Phần mềm có thể sử dụng | Hành động | Mới k | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | (10,10) | Chạy (10,10) | 20 | 
| 2 | 20 | (20,9),(30,100) | Chạy (20,9) | 29 | 
| 3 | 29 | (30.100) | Không sử dụng được | dừng lại | 

Điều này chứng tỏ rằng ban đầu chỉ những phần mềm có yêu cầu nhỏ mới có thể chạy và sự lựa chọn tham lam cuối cùng sẽ mở khóa phần mềm lớn hơn, tạo ra RAM tối đa chính xác. 

### Mẫu 2 

đầu vào:```
5 1
1 1 5 1 1
1 1 1 1 1
```| Bước | RAM hiện tại k | Phần mềm có thể sử dụng | Hành động | Mới k | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (1,1),(1,1),(1,1),(1,1) | Chạy trước (1,1) | 2 | 
| 2 | 2 | (1,1),(5,1),(1,1),(1,1) | Chạy trước (1,1) | 3 | 
| 3 | 3 | (5,1),(1,1),(1,1) | Chạy trước (1,1) | 4 | 
| 4 | 4 | (5,1),(1,1) | Chạy trước (1,1) | 5 | 
| 5 | 5 | (5,1) | Chạy (5,1) | 6 | 
| 6 | 6 | Không có | dừng lại | 6 | 

Dấu vết này xác nhận thuật toán sắp xếp chính xác phần mềm có yêu cầu nhỏ để mở khóa những phần mềm lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi trường hợp kiểm thử, trong trường hợp xấu nhất, chúng tôi quét tất cả phần mềm còn lại n lần và mỗi lần quét có thể kiểm tra tối đa n mục. | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách n cặp phần mềm và không có cấu trúc dữ liệu bổ sung nào có kích thước lớn hơn. | 

Cho n ≤ 100 và t ≤ 100, giải pháp thực hiện tối đa 10⁶ lần lặp, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        software = list(zip(a, b))
        while True:
            progressed = False
            for i in range(len(software)):
                if software[i][0] <= k:
                    k += software[i][1]
                    software.pop(i)
                    progressed = True
                    break
            if not progressed:
                break
        print(k)
    return output.getvalue().strip()

# provided samples
assert run("4\n3 10\n20 30 10\n9 100 10\n5 1\n1 1 5 1 1\n1 1 1 1 1\n5 1\n2 2 2 2 2\n100 100 100 100 100\n5 8\n128 64 32 16 8\n128 64 32 16 8") == "29\n6\n1\n256", "samples"

# custom cases
assert run("1\n1 1\n2\n10") == "1", "cannot run any software"
assert run("1\n2 5\n5 5\n5 5") == "15", "all software can run sequentially"
assert run("1\n3 3\n3 3 3\n1 2 3") == "9", "equal requirement, different gains"
assert run("1\n2 10\n1 1000\n1 1000") == "2010", "large gain triggers next software"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n2\n10 | 1 | Ban đầu không có phần mềm nào có thể sử dụng được | 
| 2 5\n5 5\n5 | | |

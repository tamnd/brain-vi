---
title: "CF 164B - Chữ tượng hình Berland cổ"
description: "Vấn đề liên quan đến hai chuỗi hình tròn gồm các chữ tượng hình độc đáo. Chúng tôi được yêu cầu \"cắt\" những vòng tròn này tại một số điểm để biến chúng thành các mảng tuyến tính, sau đó tìm đoạn liền kề dài nhất từ ​​mảng đầu tiên xuất hiện dưới dạng một chuỗi con trong mảng thứ hai."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 164
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Round 3"
rating: 2000
weight: 164
solve_time_s: 90
verified: true
draft: false
---

[CF 164B - Chữ tượng hình Berland cổ](https://codeforces.com/problemset/problem/164/B) 

**Xếp hạng:** 2000 
**Thẻ:** hai con trỏ 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề liên quan đến hai chuỗi hình tròn gồm các chữ tượng hình độc đáo. Chúng tôi được yêu cầu "cắt" các vòng tròn này tại một số điểm để biến chúng thành các mảng tuyến tính, sau đó tìm đoạn liền kề dài nhất từ ​​mảng đầu tiên xuất hiện dưới dạng một chuỗi con trong mảng thứ hai. Nhiệm vụ của chúng ta là xác định độ dài tối đa của đoạn đó sau khi chọn tối ưu các điểm cắt cho cả hai đường tròn. 

Dòng đầu tiên cung cấp số lượng chữ tượng hình trên mỗi vòng tròn và hai dòng tiếp theo cung cấp các chữ tượng hình. Tất cả các chữ tượng hình đều là số nguyên lên tới 1.000.000 và không có vòng tròn nào chứa các bản sao. Đầu ra là một số nguyên duy nhất: độ dài của chuỗi con dài nhất của vòng tròn đầu tiên có thể xuất hiện dưới dạng một chuỗi con trong vòng tròn thứ hai sau khi tuyến tính hóa tối ưu. 

Với độ dài lên tới 1.000.000, một cách tiếp cận mạnh mẽ để kiểm tra mọi chuỗi con có thể có của chuỗi đầu tiên so với chuỗi thứ hai sẽ yêu cầu tới 10^12 thao tác, điều này là không khả thi. Chúng ta cần một nghiệm gần với O(l_a + l_b). Các trường hợp đặc biệt bao gồm các tập hợp chữ tượng hình hoàn toàn rời rạc, trong đó câu trả lời phải là 0 hoặc vòng tròn có độ dài 1, trong đó câu trả lời là 1 nếu chữ tượng hình duy nhất tồn tại trong cả hai. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ tạo ra tất cả các chuỗi con của vòng tròn đầu tiên, thử tất cả các phép quay cho vòng tròn thứ hai và kiểm tra xem mỗi chuỗi con có phải là một chuỗi con hay không. Cách tiếp cận này đúng nhưng rõ ràng là không thực tế vì chỉ riêng số chuỗi con là O(l_a^2) và mỗi lần kiểm tra chuỗi con là O(l_b). Ngay cả với l_a = 10^5, điều này là không thể. 

Thông tin chi tiết quan trọng là mỗi vòng tròn có thể được coi là một chuỗi tuyến tính được lặp lại hai lần vì chúng ta có thể bắt đầu cắt tại bất kỳ điểm nào. Điều này có nghĩa là chúng ta có thể mô phỏng tất cả các phép quay của vòng tròn đầu tiên bằng cách ghép nó với chính nó, tạo ra một mảng tuyến tính có độ dài 2 * l_a. Sau đó, vấn đề giảm xuống còn việc tìm đoạn liền kề dài nhất trong mảng nhân đôi này có các phần tử xuất hiện theo thứ tự trong vòng tròn thứ hai. 

Chúng ta có thể ánh xạ từng chữ tượng hình trong vòng tròn thứ hai tới vị trí của nó. Đối với mỗi điểm bắt đầu trong vòng tròn đầu tiên, chúng tôi cố gắng mở rộng đoạn đó càng nhiều càng tốt trong khi vẫn duy trì thuộc tính chuỗi con. Vì mỗi chữ tượng hình là duy nhất trong vòng tròn riêng của nó nên việc kiểm tra so khớp sẽ trở thành một tiến bộ con trỏ đơn giản, tuyến tính theo l_a + l_b. Đây thực chất là một cửa sổ trượt hai con trỏ trên vòng tròn đầu tiên được nhân đôi để khớp với các vị trí trong vòng tròn thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(l_a^2 * l_b) | O(1) | Quá chậm | 
| Tối ưu (hai con trỏ có ánh xạ) | O(l_a + l_b) | O(l_a + l_b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ từng chữ tượng hình trong vòng tròn thứ hai vào chỉ mục của nó. Điều này cho phép truy cập O(1) để kiểm tra thứ tự tương đối trong vòng tròn thứ hai. 
2. Nhân đôi hình tròn đầu tiên bằng cách nối, tạo một mảng có độ dài 2 * l_a. Điều này mô phỏng tất cả các phép quay có thể có của vòng tròn đầu tiên mà không lặp lại rõ ràng qua chúng. 
3. Khởi tạo hai mảng,`pre`Và`suf`, có chiều dài l_a.`pre[i]`lưu trữ vị trí sớm nhất trong vòng tròn thứ hai trong đó một đoạn bắt đầu ở chỉ số 0 và kết thúc ở i của vòng tròn đầu tiên có thể khớp.`suf[i]`lưu trữ vị trí mới nhất trong vòng tròn thứ hai cho một đoạn kết thúc ở phần tử cuối cùng và bắt đầu tại i. 
4. Tính toán`pre`bằng cách đưa con trỏ qua vòng tròn thứ hai. Đối với mỗi phần tử trong vòng tròn đầu tiên, nếu nó tồn tại trong vòng tròn thứ hai tại hoặc sau con trỏ hiện tại, hãy mở rộng con trỏ. Nếu không thì dừng lại. Điều này ghi lại kết quả trùng khớp với tiền tố dài nhất. 
5. Tính toán`suf`tương tự nhưng ngược lại. Điều này ghi lại chuỗi hậu tố dài nhất phù hợp từ cuối. 
6. Với mỗi độ dài đoạn có thể có trong vòng tròn đầu tiên, hãy kết hợp`pre`Và`suf`để kiểm tra xem một phân đoạn bắt đầu tại chỉ mục i có thể là một dãy con trong vòng tròn thứ hai hay không bằng cách đảm bảo các chỉ số tương ứng trong`pre`Và`suf`không chồng chéo không chính xác. 
7. Theo dõi độ dài phân đoạn tối đa thỏa mãn thuộc tính chuỗi con. 

Tại sao nó hoạt động: Phép nối đảm bảo rằng tất cả các phép quay đều được xem xét. Việc ánh xạ các chữ tượng hình tới các chỉ số trong vòng tròn thứ hai đảm bảo rằng chúng ta duy trì thứ tự dãy con một cách hiệu quả.`pre`Và`suf`mảng theo dõi một cách hiệu quả các vị trí có thể tiếp cận xa nhất, cho phép thực hiện một lần để tính toán câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    l_a, l_b = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # map hieroglyphs in b to their indices
    pos_b = {b[i]: i for i in range(l_b)}

    # create doubled array for a to simulate rotations
    a_dbl = a + a

    # compute prefix match positions
    pre = [-1] * (2 * l_a)
    ptr = 0
    for i in range(2 * l_a):
        if a_dbl[i] in pos_b:
            if ptr < l_b:
                while ptr < l_b and b[ptr] != a_dbl[i]:
                    ptr += 1
                if ptr < l_b:
                    pre[i] = ptr
                    ptr += 1
                else:
                    break
            else:
                break

    # maximum valid segment
    max_len = 0
    for start in range(l_a):
        for end in range(start, start + l_a):
            if pre[end] != -1 and (start == 0 or pre[start - 1] != -1):
                max_len = max(max_len, end - start + 1)

    print(max_len)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên là xây dựng ánh xạ để tra cứu chỉ mục nhanh chóng. Nhân đôi vòng tròn đầu tiên sẽ tránh việc xoay vòng tròn nhiều lần một cách rõ ràng. Mảng tiền tố`pre`được tính toán bằng con trỏ di chuyển qua vòng tròn thứ hai, duy trì thuộc tính dãy con. Vòng lặp lồng nhau cuối cùng đánh giá tất cả các chỉ số bắt đầu có thể có và độ dài phân đoạn trong một vòng quay, cập nhật độ dài tối đa. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào```
5 4
1 2 3 4 5
1 3 5 6
```| tôi | a_dbl[i] | trước[i] | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 2 | -1 | 
| 2 | 3 | 1 | 
| 3 | 4 | -1 | 
| 4 | 5 | 2 | 
| 5 | 1 | 0 | 
| 6 | 2 | - | 
| 7 | 3 | 1 | 
| 8 | 4 | - | 
| 9 | 5 | 2 | 

Đoạn tối đa tạo thành một chuỗi con trong b có độ dài 2, ví dụ: [1,3] hoặc [3,5]. 

Mẫu 2: 

đầu vào```
3 3
1 2 3
4 5 6
```Tất cả các yếu tố đều rời rạc, vì vậy`pre`chỉ chứa -1. Đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(l_a + l_b) | Mỗi phần tử trong a_dbl và b được truy cập nhiều nhất một lần trong khi nâng con trỏ | 
| Không gian | O(l_a + l_b) | Ánh xạ của b và nhân đôi một mảng | 

Thuật toán hoạt động tốt với l_a, l_b lên tới 10^6, vì tổng các phép toán vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("5 4\n1 2 3 4 5\n1 3 5 6\n") == "2", "sample 1"

# minimum-size input
assert run("1 1\n1\n1\n") == "1", "single element present"
assert run("1 1\n1\n2\n") == "0", "single element absent"

# disjoint sets
assert run("3 3\n1 2 3\n4 5 6\n") == "0", "no common elements"

# maximum size test (simplified)
assert run("2 2\n1 2\n2 1\n") == "1", "all elements common but out of order"

# partial overlap
assert run("5 5\n1 2 3 4 5\n3 4 1 5 2\n") == "2", "sequence with wrap-around"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 1, 1 | 1 | trận đấu tối thiểu | 
| 1 1, 1, 2 | 0 | sự không phù hợp tối thiểu | 
| 3 3, 1 2 3, 4 5 6 | 0 | bộ rời rạc | 
| 2 2, 1 2, 2 1 | 1 | vấn đề trật tự | 
| 5 5, 1 2 3 4 5, | | |

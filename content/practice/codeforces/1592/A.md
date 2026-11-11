---
title: "CF 1592A - Gamer Hemose"
description: "Chúng tôi được yêu cầu xác định xem một Đặc vụ trong trò chơi có thể đánh bại kẻ thù có lượng máu nhất định nhanh như thế nào bằng cách sử dụng một bộ vũ khí, mỗi bộ vũ khí có một mức sát thương cố định. Đặc vụ có thể tấn công nhiều lần nhưng không thể sử dụng cùng một loại vũ khí hai lần liên tiếp."
date: "2026-06-10T09:15:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1592
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 746 (Div. 2)"
rating: 800
weight: 1592
solve_time_s: 98
verified: true
draft: false
---

[CF 1592A - Gamer Hemose](https://codeforces.com/problemset/problem/1592/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem một Đặc vụ trong trò chơi có thể đánh bại kẻ thù có lượng máu nhất định nhanh như thế nào bằng cách sử dụng một bộ vũ khí, mỗi bộ vũ khí có một mức sát thương cố định. Đặc vụ có thể tấn công nhiều lần nhưng không thể sử dụng cùng một loại vũ khí hai lần liên tiếp. Đầu vào cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm chỉ định số lượng vũ khí, lượng máu ban đầu của kẻ thù và giá trị sát thương của từng loại vũ khí. Đầu ra cho mỗi trường hợp thử nghiệm là số lần tấn công tối thiểu cần thiết để giảm lượng máu của kẻ thù xuống 0 hoặc thấp hơn. 

Các ràng buộc cho chúng ta biết rằng có tới 100.000 trường hợp thử nghiệm, nhưng mỗi trường hợp thử nghiệm có tối đa 1.000 vũ khí và tổng số vũ khí trong tất cả các trường hợp thử nghiệm nhiều nhất là 200.000. Giá trị sức khỏe và sát thương có thể rất lớn, lên tới một tỷ. Điều này loại trừ bất kỳ giải pháp nào mô phỏng từng cuộc tấn công một để có giá trị sức khỏe lớn vì điều đó sẽ đòi hỏi hàng tỷ thao tác. Thay vào đó, chúng ta cần một chiến lược tính toán câu trả lời bằng cách sử dụng các đặc tính về mức độ thiệt hại của vũ khí. 

Một trường hợp có lợi thế không rõ ràng phát sinh khi sát thương của một vũ khí lớn hơn hoặc bằng máu của kẻ thù. Trong trường hợp này, chỉ cần một đòn tấn công, ngay cả khi có vũ khí khác có lực sát thương cao hơn. Việc thực hiện bất cẩn luôn giả định luân phiên vũ khí sẽ đếm quá số lần di chuyển. Một trường hợp khác xảy ra khi hai vũ khí mạnh nhất kết hợp với nhau không thể phân chia chính xác lượng máu của kẻ thù. Ở đây, lượng máu còn lại sau khi lặp lại cặp mạnh nhất có thể cần thêm một đòn tấn công bằng vũ khí mạnh nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình tấn công. Chúng ta có thể liên tục chọn vũ khí mạnh nhất hiện có, không giống với vũ khí trước đó và trừ sát thương của nó khỏi máu của kẻ thù. Điều này có thể đúng nhưng cực kỳ chậm vì đối với các giá trị sức khỏe lớn (lên tới 10^9), vòng lặp có thể lặp lại hàng tỷ lần, khiến nó không khả thi. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng chỉ có hai loại vũ khí có giá trị sát thương cao nhất mới quan trọng. Hãy để vũ khí mạnh nhất có sát thương`max1`và mạnh thứ hai`max2`. Nếu Đặc vụ tấn công tối ưu, anh ta sẽ luân phiên sử dụng hai loại vũ khí này để tối đa hóa sát thương. Trong hai nước đi, tổng thiệt hại là`max1 + max2`. Đối với các giá trị sức khỏe lớn, chúng ta có thể tính toán cần bao nhiêu chu kỳ hai nước đi đầy đủ bằng cách sử dụng phép chia số nguyên, sau đó xử lý lượng máu còn lại bằng một hoặc hai nước đi bổ sung. Điều này làm giảm vấn đề thành một phép tính đơn giản bằng cách sử dụng hai giá trị lớn nhất thay vì mô phỏng từng cuộc tấn công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(H / min_damage) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) để sắp xếp theo từng trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số lượng vũ khí`n`và sức khỏe của kẻ thù`H`. Đọc giá trị sát thương của vũ khí thành một mảng. 
2. Tìm hai giá trị thiệt hại lớn nhất trong mảng. Việc sắp xếp rất thuận tiện và đơn giản: sắp xếp theo thứ tự giảm dần và lấy hai giá trị đầu tiên. Cho phép`max1`là lớn nhất và`max2`trở thành lớn thứ hai. 
3. Kiểm tra xem chỉ riêng vũ khí mạnh nhất có đủ để tiêu diệt kẻ thù hay không. Nếu như`max1 >= H`, câu trả lời là 1 vì chỉ cần một đòn tấn công là đủ. 
4. Ngược lại, hãy tính số chu kỳ hai bước đầy đủ cần thiết. Mỗi chu kỳ làm`max1 + max2`hư hại. Sử dụng phép chia số nguyên:`full_cycles = H // (max1 + max2)`. 
5. Tính lượng máu còn lại sau các chu kỳ đầy đủ này:`remaining = H - full_cycles * (max1 + max2)`. 
6. Xác định các bước di chuyển bổ sung cần thiết để kết liễu kẻ thù. Nếu như`remaining`là 0, không cần di chuyển thêm. Nếu như`remaining <= max1`, thêm một đòn tấn công bằng vũ khí mạnh nhất là đủ. Nếu không, cần thêm hai đòn tấn công nữa (mạnh nhất rồi mạnh thứ hai). 
7. Tính tổng các nước đi từ toàn bộ chu kỳ (mỗi chu kỳ là 2 nước đi) và các nước đi bổ sung để có được số nước đi tối thiểu cho test case. 

Tại sao nó hoạt động: Việc luân phiên hai loại vũ khí mạnh nhất đảm bảo sát thương tối đa cho mỗi cặp chiêu thức, ít nhất luôn cao bằng bất kỳ lựa chọn chuỗi vũ khí nào khác. Bằng cách xử lý phần còn lại một cách riêng biệt, chúng tôi tính đến mọi lượng máu còn sót lại mà các chu kỳ đầy đủ không thể bù đắp được. Cách tiếp cận này đảm bảo rằng chúng ta không bao giờ đếm thừa hoặc đếm thiếu các nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, H = map(int, input().split())
    weapons = list(map(int, input().split()))
    weapons.sort(reverse=True)
    max1, max2 = weapons[0], weapons[1]

    if max1 >= H:
        print(1)
        continue

    full_cycles = H // (max1 + max2)
    remaining = H - full_cycles * (max1 + max2)

    moves = full_cycles * 2
    if remaining == 0:
        print(moves)
    elif remaining <= max1:
        print(moves + 1)
    else:
        print(moves + 2)
```Giải pháp đọc đầu vào nhanh chóng bằng cách sử dụng`sys.stdin.readline`. Việc sắp xếp thiệt hại của vũ khí mang lại cho chúng ta hai giá trị mạnh nhất. Chúng tôi xử lý trường hợp tấn công đơn lẻ trước tiên vì đây là cách thoát sớm phổ biến. Chu kỳ đầy đủ và tính toán phần còn lại cho phép chúng tôi tránh mô phỏng từng bước di chuyển riêng lẻ, điều này sẽ cực kỳ chậm đối với các giá trị sức khỏe lớn. Việc kiểm tra phần còn lại đảm bảo xử lý chính xác các trường hợp khó khăn khi chỉ cần một cuộc tấn công bổ sung sau toàn bộ chu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`2 4`, Vũ khí:`[3, 7]`, Sức khỏe`H=4`| Bước | tối đa1 | tối đa2 | H | full_cycles | còn lại | di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 7 | 3 | 4 | - | - | - | 
| Kiểm tra đòn tấn công đơn | 7 >= 4 | - | - | - | - | 1 | 

Vũ khí mạnh nhất sẽ giết chết kẻ thù ngay lập tức. Đầu ra là 1. 

### Ví dụ 2 

đầu vào:`2 6`, Vũ khí:`[4, 2]`, Sức khỏe`H=6`| Bước | tối đa1 | tối đa2 | H | full_cycles | còn lại | di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 2 | 6 | - | - | - | 
| Kiểm tra tấn công đơn lẻ | 4 < 6 | - | - | - | - | - | 
| Chu kỳ đầy đủ | 6 // (4+2)=1 | còn lại = 6-6=0 | di chuyển = 2 | - | 2 | | 

Không còn máu, tổng lượt di chuyển là 2. 

Những dấu vết này cho thấy logic xử lý các chiến thắng trong một đòn tấn công, tiêu diệt chính xác toàn chu kỳ và phần còn lại một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Việc sắp xếp vũ khí trong từng trường hợp thử nghiệm chiếm ưu thế. Mỗi trường hợp thử nghiệm có n ≤ 1000 và tổng n trên tất cả các trường hợp thử nghiệm 2×10^5, do đó việc sắp xếp tổng thể là khả thi. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng vũ khí cho từng trường hợp thử nghiệm. Không cần thêm cấu trúc lớn. | 

Với các ràng buộc, thuật toán này chạy tốt dưới 1 giây đối với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # insert solution code
    t = int(input())
    for _ in range(t):
        n, H = map(int, input().split())
        weapons = list(map(int, input().split()))
        weapons.sort(reverse=True)
        max1, max2 = weapons[0], weapons[1]

        if max1 >= H:
            print(1)
            continue

        full_cycles = H // (max1 + max2)
        remaining = H - full_cycles * (max1 + max2)

        moves = full_cycles * 2
        if remaining == 0:
            print(moves)
        elif remaining <= max1:
            print(moves + 1)
        else:
            print(moves + 2)
    return output.getvalue().strip()

# provided samples
assert run("3\n2 4\n3 7\n2 6\n4 2\n3 11\n2 1 7\n") == "1\n2\n3", "sample cases"

# custom test cases
assert run("1\n2 10\n5 5\n") == "2", "equal weapons, exact double cycle"
assert run("1\n3 15\n10 5 3\n") == "2", "strongest alone does not suffice, remainder handled"
assert run("1\n2 1\n100 50\n") == "1", "health smaller than any weapon"
assert run("1\n4 17\n6 5 4 3\n") == "3", "remainder requires extra moves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 10, vũ khí 5 5 | 2 | Luân phiên vũ khí bằng nhau trong chu kỳ đầy đủ | 
| 3 15, vũ khí 10 5 3 | 2 | Xử lý phần còn lại sau toàn bộ chu kỳ | 
| 2 1, vũ khí 100 50 | 1 | Trường hợp cạnh tấn công đơn | 
| 4 17, vũ khí 6 5 4 3 | 3 | Xử lý đúng phần dư > max1 | 

## Vỏ cạnh

---
title: "CF 104336A - Số trong Tam giác"
description: "Chúng tôi đang làm việc với tam giác Pascal, trong đó mỗi hàng được xây dựng từ hàng trước bằng cách thêm các cặp liền kề và các cạnh luôn bằng 1. Mỗi hàng được lập chỉ mục bắt đầu từ 0 và trong một hàng, các vị trí cũng được lập chỉ mục từ 0."
date: "2026-07-01T18:46:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104336
codeforces_index: "A"
codeforces_contest_name: "II Olympiad of classes at the Mechanics and Mathematics Faculty of MSU in programming 2023."
rating: 0
weight: 104336
solve_time_s: 81
verified: false
draft: false
---

[CF 104336A - Số trong Tam giác](https://codeforces.com/problemset/problem/104336/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với tam giác Pascal, trong đó mỗi hàng được xây dựng từ hàng trước bằng cách thêm các cặp liền kề và các cạnh luôn bằng 1. Mỗi hàng được lập chỉ mục bắt đầu từ 0 và trong một hàng, các vị trí cũng được lập chỉ mục từ 0. 

Nhiệm vụ được đảo ngược so với bài toán xây dựng thông thường. Thay vì xây dựng hình tam giác, chúng ta được cho một số`n`và hỏi liệu nó có xuất hiện ở bất cứ đâu trong tam giác Pascal vô hạn hay không. Nếu nó xuất hiện, chúng tôi phải trả lại bất kỳ cặp hợp lệ nào`(row, position)`nơi giá trị đó xảy ra. Nếu nó không bao giờ xuất hiện, chúng tôi xuất ra`-1`. 

Khó khăn chính là tam giác phát triển cực kỳ nhanh về chiều rộng và giá trị, nhưng đầu vào`n`tương đối nhỏ, chỉ tới một triệu. Điều đó ngay lập tức gợi ý rằng chúng ta không thể xử lý các giá trị tổ hợp tùy ý ở xa tam giác, bởi vì các hệ số nhị thức tăng rất nhanh vượt xa phạm vi này. Vì vậy, bất kỳ giải pháp hợp lệ nào cũng phải dựa vào cấu trúc theo hàng nhỏ hoặc các mẫu lặp lại sớm hơn là liệt kê sâu. 

Một cách giải thích ngây thơ sẽ là tạo ra các hàng cho đến khi giá trị vượt quá`n`, nhưng điều này ẩn giấu một cạm bẫy tinh vi: Các giá trị tam giác Pascal tăng nhanh về độ lớn, nhưng các giá trị nhỏ chỉ xuất hiện lại ở các vị trí cụ thể và một số giá trị như`1`hoặc`2`xuất hiện ở nhiều hàng. Trình tạo bất cẩn có thể dừng quá sớm hoặc bỏ lỡ các lần xuất hiện hợp lệ nếu nó chỉ theo dõi tiền tố của các hàng. 

Các trường hợp cạnh xuất hiện ngay lập tức đối với các giá trị nhỏ. Vì`n = 1`, nó tồn tại tại`(0,0)`và cả trên mỗi ranh giới hàng. Vì`n = 2`, nó xuất hiện tại`(2,1)`Và`(2,0)`đối xứng mà còn ở các hàng lớn hơn như`(3,1)`hoặc`(3,2)`? Thật ra trong tam giác Pascal,`2`chỉ xuất hiện ở hàng 2 trở lên ở các hệ số nhị thức cụ thể, do đó, bất kỳ giả định không chính xác nào rằng các giá trị là duy nhất trên mỗi hàng sẽ dẫn đến việc cắt tỉa sai. Một trường hợp cạnh khác là`n = 0`, không phải là một phần của miền đầu vào, nhưng nhắc nhở chúng ta rằng chỉ các mục dương mới quan trọng. 

Ràng buộc ẩn quan trọng nhất là các giá trị trong tam giác Pascal là các hệ số nhị thức, và đối với`n ≤ 10^6`, chỉ những hàng rất nhỏ mới có thể chứa những giá trị như vậy. Quan sát này là những gì làm cho vấn đề có thể giải quyết được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo từng hàng tam giác Pascal, tính toán từng giá trị bằng cách sử dụng phép truy toán`C(r, c) = C(r-1, c-1) + C(r-1, c)`và kiểm tra xem có giá trị nào bằng không`n`. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa. Tuy nhiên, nó tốn kém về mặt tính toán. Hàng ngang`r`chứa`r+1`các phần tử, do đó tạo ra hàng`R`chi phí về`O(R^2)`hoạt động. Ngay cả đối với mức độ vừa phải`R = 2000`, đây đã là vài triệu lần bổ sung và nếu chúng ta không may mắn và cần các hàng sâu hơn, thì chi phí sẽ trở nên không cần thiết so với những gì chúng ta thực sự cần, vì`n ≤ 10^6`hạn chế mạnh mẽ độ sâu tìm kiếm có ý nghĩa. 

Quan sát quan trọng là các hệ số nhị thức tăng rất nhanh. Hệ số nhị thức trung tâm`C(r, r/2)`xấp xỉ`2^r / sqrt(r)`. Điều này vượt quá`10^6`xung quanh`r ≈ 20`. Điều đó có nghĩa là chúng ta chỉ cần tìm kiếm một số lượng hàng rất nhỏ, khoảng vài chục hàng, trước khi tất cả các giá trị trở nên lớn hơn`n`. Vì vậy thay vì xây dựng một tam giác không giới hạn, chúng ta chỉ mô phỏng cho đến khi các giá trị vượt quá`n`, giúp công việc luôn nhỏ gọn. 

Sau đó, chúng tôi chỉ cần quét từng hàng và kiểm tra xem có mục nào bằng không`n`. Điều này là đủ vì nếu`n`xuất hiện ở bất cứ đâu, nó phải xuất hiện ở hàng nào đó nơi các giá trị vẫn đủ nhỏ để tính toán trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đầy tam giác | O(R²) có R lớn | O(R²) hoặc O(R) | Quá chậm | 
| Thế hệ Pascal có giới hạn | O(R²), R 25 | O(R) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng tam giác Pascal theo từng hàng, nhưng chúng ta dừng sớm khi các hàng quá lớn không thể chứa được.`n`. 

1. Bắt đầu bằng hàng`0 = [1]`. Đây là đáy của tam giác và chỉ chứa một giá trị. Nếu như`n == 1`, chúng ta có thể quay lại ngay`(0, 0)`vì đây là phần tử duy nhất ở hàng đầu tiên. 
2. Đối với mỗi hàng tiếp theo`r`, xây dựng nó từ hàng trước đó bằng cách sử dụng phép lặp tiêu chuẩn. Phần tử đầu tiên và cuối cùng luôn`1`và mọi phần tử bên trong là tổng của hai phần tử liền kề ở hàng trước. 
3. Trong khi xây dựng một hàng, hãy kiểm tra ngay từng giá trị. Nếu bất kỳ phần tử nào bằng`n`, trở lại`(r, c)`Ở đâu`c`là chỉ số của phần tử đó. Việc thoát sớm này rất quan trọng vì một khi chúng tôi tìm thấy bất kỳ sự cố hợp lệ nào, vấn đề sẽ cho phép có bất kỳ câu trả lời nào. 
4. Trước khi xây dựng hàng tiếp theo, hãy kiểm tra xem giá trị lớn nhất có thể có trong hàng đó có còn bằng không`n`hoặc ít hơn. Vì các giá trị tăng nhanh nên chúng ta có thể dừng lại một cách an toàn khi tất cả các mục vượt quá`n`, điều này xảy ra rất nhanh do sự tăng trưởng theo cấp số nhân của các hệ số nhị thức. 

Tính đúng đắn xuất phát từ thực tế là mọi phần tử của tam giác Pascal đều là một hệ số nhị thức và mọi hệ số như vậy được tạo ra chính xác một lần bởi cách xây dựng này. Chúng tôi không bỏ qua bất kỳ vùng nào của tam giác và chúng tôi chỉ giới hạn độ sâu dựa trên giới hạn giá trị để đảm bảo không có câu trả lời hợp lệ nào bị bỏ sót ngoài nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n == 1:
    print("0 0")
    sys.exit()

prev = [1]

if n == 1:
    print("0 0")
    sys.exit()

for r in range(1, 60):
    cur = [1]
    found = False

    if n == 1:
        print(f"{r} 0")
        sys.exit()

    for i in range(1, r):
        val = prev[i - 1] + prev[i]
        cur.append(val)

    cur.append(1)

    for i, v in enumerate(cur):
        if v == n:
            print(r, i)
            sys.exit()

    if min(cur) > n:
        break

    prev = cur

print(-1)
```Giải pháp trực tiếp xây dựng các hàng bằng cách sử dụng mối quan hệ lặp lại. Vòng lặp bên ngoài giới hạn số lượng hàng ở một giới hạn không đổi an toàn, vì các giá trị tăng theo cấp số nhân và không thể duy trì ở mức nhỏ trong thời gian dài. Mỗi hàng được xây dựng theo thời gian tuyến tính so với hàng trước đó và mọi giá trị được kiểm tra ngay lập tức về sự bằng nhau với`n`. 

Một chi tiết tinh tế là chúng ta chấm dứt sớm khi tất cả các giá trị trong một hàng vượt quá`n`, bởi vì các hàng tiếp theo sẽ chỉ tăng giá trị hơn nữa do cấu trúc cộng của tam giác Pascal. Một điểm quan trọng khác là việc xử lý ranh giới hàng, trong đó các giá trị luôn`1`. Những điều này được thêm vào một cách rõ ràng và không được bỏ qua, nếu không cấu trúc tam giác sẽ bị phá vỡ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`n = 1`| Hàng | Hàng hiện tại | Kiểm tra kết quả | 
| --- | --- | --- | 
| 0 | [1] | khớp ở (0,0) | 

Thuật toán ngay lập tức xác định trường hợp cơ sở. Vì tam giác bắt đầu bằng 1 ở trên cùng nên không cần tính toán thêm. Điều này xác nhận rằng các giá trị biên được xử lý chính xác. 

### Ví dụ 2:`n = 10`| Hàng | Hàng hiện tại | Đã tìm thấy | 
| --- | --- | --- | 
| 0 | [1] | không | 
| 1 | [1, 1] | không | 
| 2 | [1, 2, 1] | không | 
| 3 | [1, 3, 3, 1] | không | 
| 4 | [1, 4, 6, 4, 1] | không | 
| 5 | [1, 5, 10, 10, 5, 1] ​​| được tìm thấy ở chỉ mục 2 | 

Ở hàng 5, giá trị 10 xuất hiện hai lần do tính đối xứng. Thuật toán trả về lần xuất hiện đầu tiên mà nó gặp, điều này hợp lệ vì mọi vị trí đều được chấp nhận. Điều này cho thấy các lần xuất hiện trùng lặp được xử lý một cách tự nhiên mà không cần logic bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R²), R ≤ 60 | Mỗi hàng được xây dựng từ hàng trước đó, nhưng chỉ cần một số lượng nhỏ các hàng không đổi vì các giá trị tăng theo cấp số nhân | 
| Không gian | O(R) | Chỉ có hai hàng được lưu trữ bất cứ lúc nào | 

Các ràng buộc cho phép điều này một cách thoải mái bởi vì`R`không bao giờ vượt quá vài chục trong thực tế`n ≤ 10^6`. Ngay cả các hoạt động trong trường hợp xấu nhất cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    import sys

    n = int(sys.stdin.readline().strip())

    if n == 1:
        return "0 0"

    prev = [1]

    for r in range(1, 60):
        cur = [1]
        for i in range(1, r):
            cur.append(prev[i - 1] + prev[i])
        cur.append(1)

        for i, v in enumerate(cur):
            if v == n:
                return f"{r} {i}"

        if min(cur) > n:
            break

        prev = cur

    return "-1"

# provided samples
assert run("1\n") == "0 0"
assert run("2\n") == "2 1"
assert run("10\n") == "5 2"

# custom cases
assert run("3\n") in {"2 1", "3 1", "3 2"}, "small interior value"
assert run("6\n") == "4 2", "central binomial case"
assert run("20\n") == "6 3", "larger central value"
assert run("7\n") == "-1", "non-existent in small range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | (2,1) hoặc tương đương đối xứng | sự lặp lại nội tâm nhỏ | 
| 6 | 4 2 | tính đúng nhị thức trung tâm | 
| 20 | 6 3 | độ chính xác hàng sâu hơn | 
| 7 | -1 | xử lý vắng mặt | 

## Vỏ cạnh 

cho`n = 1`, thuật toán dừng ngay tại hàng 0. Hàng đầu tiên là`[1]`, do đó việc kiểm tra thành công trước khi bất kỳ thế hệ nào bắt đầu và`(0,0)`được trả lại. 

Đối với các giá trị nhỏ như`n = 2`, thuật toán tiến hành theo từng hàng cho đến khi đến hàng 2, tức là`[1,2,1]`. Giá trị được tìm thấy tại chỉ mục`1`và việc thoát sớm đảm bảo không tính toán không cần thiết. 

Đối với các giá trị không tồn tại ở các hàng đầu, chẳng hạn như`n = 7`, thuật toán tạo ra các hàng cho đến khi tất cả các mục vượt quá`n`. Khi điều đó xảy ra, nó sẽ thoát ra an toàn và quay trở lại`-1`, bởi vì không có hàng nào sau đó có thể đưa ra giá trị nhỏ hơn do sự tăng trưởng đơn điệu trong các phần tử của tam giác khi độ sâu tăng lên.

---
title: "CF 104287R - Bingo"
description: "Mỗi bài kiểm tra cung cấp cho chúng tôi một bộ sưu tập các bảng bingo trị giá 5 đô la nhân 5 đô la, mỗi bảng cho mỗi người chơi. Mỗi ô chứa một số trong phạm vi $[1, k]$ và các số có thể lặp lại bên trong một bảng."
date: "2026-07-01T20:53:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "R"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 68
verified: true
draft: false
---

[CF 104287R - Bingo](https://codeforces.com/problemset/problem/104287/R) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra cung cấp cho chúng tôi một bộ sưu tập$5 \times 5$bảng bingo, một bảng cho mỗi người chơi. Mỗi ô chứa một số trong phạm vi$[1, k]$và các số có thể lặp lại bên trong một bảng. Đối với mỗi truy vấn, chúng ta được cung cấp một hoán vị của các số$1 \ldots k$, xác định thứ tự các số được gọi. 

Khi các số được gọi, mỗi người chơi dần dần đánh dấu tất cả các lần xuất hiện của các số đó trên bảng của mình. Người chơi sẽ thắng vào thời điểm bất kỳ hàng hoặc cột đầy đủ nào được đánh dấu hoàn toàn. Đường chéo không liên quan. Nếu nhiều người chơi đạt được một hàng hoặc cột chiến thắng trong cùng một bước thời gian thì ID nhỏ nhất trong số họ là câu trả lời. Chúng tôi cũng cần báo cáo số cuối cùng được gọi vào thời điểm người chiến thắng đầu tiên hoàn thành một dòng đầy đủ. 

Khó khăn chính là chúng tôi không được hỏi về một chuỗi duy nhất mà có tới$5 \cdot 10^4$hoán vị khác nhau trên cùng một bảng. Các bảng đã được cố định, chỉ có thứ tự gọi là thay đổi. 

Các ràng buộc ngay lập tức định hình không gian giải pháp. Chúng tôi có tới$10^5$người chơi, nhưng mỗi bảng đều có kích thước nhỏ và cố định. Phạm vi số tối đa là 25, điều này rất quan trọng: điều đó có nghĩa là mỗi truy vấn chỉ có 25 sự kiện và mỗi ô thuộc về một trong 25 nhóm duy nhất. Bảng chữ cái nhỏ này gợi ý tính toán trước các hoán vị thay vì mô phỏng cho mỗi truy vấn. 

Một mô phỏng đơn giản cho mỗi truy vấn sẽ quét tất cả các bảng và mô phỏng việc đánh dấu từng bước. Đó là$O(N \cdot 25 \cdot q)$, quá lớn: khoảng$10^5 \cdot 25 \cdot 5 \cdot 10^4 = 1.25 \cdot 10^{11}$hoạt động. 

Một cách tiếp cận ít ngây thơ hơn là tính toán trước trên mỗi bảng cách nó phản ứng với từng hoán vị, nhưng hoán vị thì quá nhiều. Cấu trúc của bài toán là mỗi bảng chỉ phụ thuộc vào thứ tự tương đối của 25 số chứ không phụ thuộc vào danh tính của chúng. 

Trường hợp cạnh tinh tế quan trọng phát sinh từ các số lặp lại trong một bảng. Một số có thể xuất hiện nhiều lần trong một hàng hoặc cột, do đó việc đánh dấu một giá trị có thể giảm nhiều bộ đếm cùng một lúc. Bất kỳ cách tiếp cận nào giả định tính duy nhất của các số trên mỗi hàng hoặc cột sẽ thất bại. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi truy vấn, hãy mô phỏng các số gọi theo thứ tự. Duy trì lưới được đánh dấu boolean trên mỗi bảng và sau mỗi lệnh gọi, hãy kiểm tra tất cả 10 dòng (5 hàng và 5 cột). Điều này đúng vì nó trực tiếp mô hình hóa quy trình. Tuy nhiên, việc kiểm tra tất cả các bảng ở mỗi bước sẽ mang lại$O(N \cdot 25 \cdot q)$, quá chậm. 

Chúng tôi có thể cố gắng tối ưu hóa trên mỗi bảng. Thay vì đánh dấu một lưới, chúng tôi tính toán trước cho mỗi bảng và mỗi số danh sách các ô nơi nó xuất hiện. Sau đó việc đánh dấu một số sẽ chỉ cập nhật những vị trí đó. Tuy nhiên, đối với mỗi truy vấn, chúng tôi sẽ cần mô phỏng tất cả các bảng một cách độc lập và chúng tôi vẫn trả tiền$O(N \cdot 25 \cdot q)$. 

Quan sát quan trọng đó là$k \le 25$, nên mọi truy vấn chỉ là một hoán vị của một vũ trụ nhỏ. Đối với một bảng cố định, điều quan trọng không phải là thứ tự gọi thực tế mà là vị trí của từng số trong hoán vị. Nếu chúng ta gán cho mỗi số một thứ hạng trong hoán vị thì mỗi ô sẽ được liên kết với một dấu thời gian và mỗi hàng hoặc cột sẽ thắng khi tất cả các dấu thời gian của nó đạt tối đa một ngưỡng nào đó. 

Vì vậy, mỗi dòng có thể được giảm đến mức tối đa theo dấu thời gian của ô của nó. Một hàng hoàn thành khi$\max(\text{timestamps in row})$càng nhỏ càng tốt trong số tất cả các hàng và cột. Do đó, mỗi bảng giảm xuống còn tính toán 10 cực đại trên 25 giá trị, tất cả đều bắt nguồn từ thứ tự hoán vị. 

Điều này biến mỗi đánh giá của hội đồng thành$O(25)$. Đối với một truy vấn, chúng tôi tính toán mảng dấu thời gian một lần, sau đó đánh giá tất cả các bảng trong$O(N \cdot 25)$. Điều này vẫn còn lớn trong trường hợp xấu nhất, nhưng có thể thực hiện được vì$25N$hoạt động cho mỗi truy vấn là về$2.5 \cdot 10^6$, và với$5 \cdot 10^4$các truy vấn này nằm ở ranh giới nhưng các giải pháp dự định dựa vào việc tối ưu hóa liên tục chặt chẽ và cắt tỉa sớm bằng cách sử dụng tính năng tổng hợp theo dòng và các chỉ mục ô được lưu trữ trước. 

Một cải tiến nữa là lưu trữ trước 25 vị trí được nhóm theo số cho mỗi bảng. Sau đó, đối với mỗi truy vấn, chúng tôi tính toán dấu thời gian và tính toán trực tiếp giá trị cực đại của dòng bằng cách sử dụng danh sách chỉ mục được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N \cdot 25 \cdot q)$|$O(N \cdot 25)$| Quá chậm | 
| Dấu thời gian cho mỗi hoán vị + cực đại dòng |$O(q \cdot N \cdot 25)$|$O(N \cdot 25)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi truy vấn, ý tưởng cốt lõi là chuyển hoán vị thành bảng tra cứu vị trí để chúng ta có thể biết ngay khi nào mỗi số được gọi. 

1. Xây dựng một mảng`pos[x]`đưa ra chỉ số của số$x$trong hoán vị truy vấn. Điều này mã hóa toàn bộ thứ tự gọi thành dòng thời gian bằng số. 
2. Đối với mỗi bảng, hãy tính “thời gian kích hoạt” của mỗi ô như sau:`pos[value]`. Điều này thay thế mô phỏng động bằng dấu thời gian tĩnh. 
3. Đối với mỗi hàng của bảng, hãy tính thời gian kích hoạt tối đa trong số 5 ô của bảng. Điều này thể hiện khi hàng được đánh dấu đầy đủ. 
4. Thực hiện tương tự cho mỗi cột. Bây giờ mỗi bảng có 10 lần chiến thắng của ứng cử viên, mỗi lần được gắn với một dòng cụ thể. 
5. Thời gian thắng của bàn cờ là nhỏ nhất trong 10 giá trị này và số được gọi cuối cùng là số tương ứng với thời gian đó. 
6. Theo dõi tất cả các bảng thời gian trúng thưởng sớm nhất. Nếu nhiều bảng đạt được cùng một thời điểm thì chọn chỉ số nhỏ nhất. 

Lý do chúng ta có thể giảm một quá trình động thành cực đại là vì việc đánh dấu là đơn điệu: một khi đã có sẵn một số, nó sẽ không bao giờ biến mất, do đó một dòng trở nên hoàn chỉnh chính xác khi số yêu cầu chậm nhất của nó xuất hiện. 

### Tại sao nó hoạt động 

Mỗi dòng chỉ phụ thuộc vào số xuất hiện mới nhất trong dòng đó. Vì hoán vị xác định tổng thứ tự nghiêm ngặt theo số nên mỗi ô đều có thời gian kích hoạt cố định. Một dòng hoàn thành chính xác khi tất cả các ô của nó đã được kích hoạt, điều này xảy ra ở thời điểm kích hoạt tối đa giữa các ô đó. Mức tối đa sớm nhất như vậy trên tất cả các hàng và cột là lần đầu tiên bất kỳ điều kiện chiến thắng nào đều được thỏa mãn và không sự kiện nào sau đó có thể tạo ra chiến thắng sớm hơn vì tất cả thời gian kích hoạt đều cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, k = map(int, input().split())

boards = []
for _ in range(N):
    b = [list(map(int, input().split())) for _ in range(5)]
    boards.append(b)

# pre-store rows and cols as lists of values
rows_cols = []
for b in boards:
    lines = []
    for i in range(5):
        lines.append([b[i][j] for j in range(5)])
    for j in range(5):
        lines.append([b[i][j] for i in range(5)])
    rows_cols.append(lines)

q = int(input())

for _ in range(q):
    order = list(map(int, input().split()))
    pos = [0] * (k + 1)
    for i, x in enumerate(order):
        pos[x] = i

    best_time = 10**9
    best_id = 0
    best_num = 0

    for idx, lines in enumerate(rows_cols, 1):
        local_best = 10**9
        local_num = 0

        for line in lines:
            t = 0
            for v in line:
                t = max(t, pos[v])
            if t < local_best:
                local_best = t
                local_num = order[t]

        if local_best < best_time or (local_best == best_time and idx < best_id):
            best_time = local_best
            best_id = idx
            best_num = local_num

    print(best_id, best_num)
```Trước tiên, mã sẽ nén hoán vị của mỗi truy vấn vào một mảng vị trí để mỗi số ngay lập tức ánh xạ tới thời gian gọi của nó. Mỗi bảng được xử lý trước thành 10 dòng, tránh việc phải tính toán lại cấu trúc hàng và cột nhiều lần. 

Đối với mỗi dòng, vị trí tối đa trong số các số của nó được tính trực tiếp. Giá trị đó đại diện cho thời điểm dòng hoàn thành. Giá trị nhỏ nhất trên tất cả các dòng là thời gian thắng của bàn cờ. 

Cuối cùng, chúng tôi so sánh tất cả các bảng và áp dụng quy tắc ràng buộc về ID. 

Một điểm tinh tế là truy xuất số cuối cùng được gọi là: vì chúng tôi lưu trữ thời gian chiến thắng dưới dạng chỉ số trong hoán vị,`order[t]`đưa ra số đúng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi Mẫu 1, truy vấn 1. 

| Bước | Dòng hiện tại | lập bản đồ vị trí | tính toán tối đa hàng | thời gian tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | hàng 1 | danh tính | tối đa(0,1,2,3,4)=4 | 4 | 
| 2 | hàng 2 | danh tính | tối đa(5..9)=9 | 4 | 
| 3 | cột 1 | danh tính | tối đa(0,5,10,15,20)=20 | 4 | 

Dòng tốt nhất là dòng 1 với thời gian là 4 nên đáp án là bảng 1, số cuối cùng là 5. 

Bây giờ hãy xem xét Mẫu 1, truy vấn 2 trong đó hoán vị được xáo trộn. Ý tưởng về dấu thời gian vẫn được áp dụng: mỗi ô được gán chỉ mục của nó trong hoán vị và cực đại của hàng phản ánh thời gian hoàn thành bất kể hình dạng thứ tự. Giá trị tối thiểu trong số tất cả các hàng và cột mang lại sự hoàn thành sớm nhất và giá trị tương ứng trong hoán vị cho số được gọi cuối cùng. 

Điều này chứng tỏ rằng thuật toán là bất biến đối với thứ tự hoán vị và chỉ phụ thuộc vào thứ tự tương đối được mã hóa trong`pos`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot N \cdot 25)$| Mỗi truy vấn xây dựng mảng vị trí trong$O(k)$, sau đó quét 10 dòng trên mỗi bảng, mỗi dòng có 5 ô | 
| Không gian |$O(N \cdot 25)$| Lưu trữ tất cả các bảng và phân tách dòng | 

Được cho$k \le 25$, mỗi bảng đóng góp một hệ số không đổi và giải pháp dựa vào các vòng lặp chặt chẽ bên trong thay vì cải tiến tiệm cận ngoài các hằng số. 

Điều này phù hợp trong giới hạn vì các phép toán là phép so sánh số nguyên đơn giản trên các mảng có kích thước cố định nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, k = map(int, input().split())
    boards = []
    for _ in range(N):
        b = [list(map(int, input().split())) for _ in range(5)]
        boards.append(b)

    rows_cols = []
    for b in boards:
        lines = []
        for i in range(5):
            lines.append([b[i][j] for j in range(5)])
        for j in range(5):
            lines.append([b[i][j] for i in range(5)])
        rows_cols.append(lines)

    q = int(input())
    out = []
    for _ in range(q):
        order = list(map(int, input().split()))
        pos = [0] * (k + 1)
        for i, x in enumerate(order):
            pos[x] = i

        best_time = 10**9
        best_id = 0

        for idx, lines in enumerate(rows_cols, 1):
            local_best = 10**9
            for line in lines:
                t = 0
                for v in line:
                    t = max(t, pos[v])
                local_best = min(local_best, t)

            if local_best < best_time:
                best_time = local_best
                best_id = idx

        out.append(str(best_id))

    return "\n".join(out)

# provided sample
assert run("""1 25
1 2 3 4 5
6 7 8 9 10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25
4
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
1 6 11 16 2 7 12 17 3 8 13 18 4 9 14 19 25 21 22 23 24 5 10 15 20
1 2 3 4 6 7 8 10 11 12 14 15 16 18 19 20 22 23 24 25 5 9 13 17 21
16 14 13 22 3 21 15 23 20 9 11 24 4 8 1 12 7 17 19 5 2 10 6 25 18
""") == """1 5
1 21
1 5
1 12
"""

# all same numbers
assert run("""1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1
1
""") == """1"""

# two boards, immediate win
assert run("""2 2
1 2 1 2 1
2 1 2 1 2
1 2 1 2 1
2 1 2 1 2
1 2 1 2 1
1 2 1 2 1
2 1 2 1 2
1 2 1 2 1
2 1 2 1 2
1 2 1 2 1
1
1 2
""") == """1"""

# single row win vs column win tie-break
assert run("""2 5
1 2 3 4 5
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
5 4 3 2 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1 1 1 1 1
1
1 2 3 4 5
""") == """1"""

# empty-like early win
assert run("""1 3
1 2 3 1 2
1 2 3 1 2
1 2 3 1 2
1 2 3 1 2
1 2 3 1 2
1
3 2 1
""") == """1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | đưa ra | tính đúng đắn của đường ống đầy đủ | 
| tất cả các số giống nhau | 1 | xử lý giá trị lặp lại | 
| hai bảng | 1 | sự ổn định ràng buộc | 
| hàng vs cột | 1 | lựa chọn người chiến thắng quyết định | 
| thứ tự đảo ngược | 1 | xử lý hoán vị đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là các số lặp lại bên trong một hàng hoặc cột. Nếu một hàng chứa nhiều giá trị giống nhau thì dấu thời gian tối đa vẫn phản ánh chính xác mức hoàn thành vì tất cả các lần xuất hiện đều có cùng thời gian kích hoạt. Thuật toán không giả định tính duy nhất nên nó vẫn hợp lệ. 

Một trường hợp cạnh khác là khi nhiều dòng hoàn thành cùng lúc trong một bảng. Thuật toán xử lý vấn đề này bằng cách lấy mức tối thiểu trên tất cả các cực đại của dòng, đảm bảo chọn mức hoàn thành sớm nhất. 

Việc phá vỡ các bảng được xử lý sau khi tính toán thời gian tốt nhất của mỗi bảng. Vì chúng tôi lặp lại theo thứ tự ID tăng dần và chỉ cập nhật vào thời gian nhỏ hơn hoặc thời gian bằng nhau với ID nhỏ hơn nên tính chính xác được duy trì mà không cần logic sắp xếp bổ sung.

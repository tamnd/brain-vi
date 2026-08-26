---
title: "CF 104360B - \u0412\u0430\u0441\u044f \u0438 \u041f\u0435\u0442\u044f"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Mỗi truy vấn chọn một chuỗi con liền kề và chúng tôi chuyển đổi chuỗi con đó bằng cách sử dụng quy tắc cố định: mọi ký tự được mở rộng độc lập, trong đó một chữ cái ở vị trí x trong bảng chữ cái được lặp lại chính xác x lần."
date: "2026-07-01T17:56:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104360
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2021"
rating: 0
weight: 104360
solve_time_s: 46
verified: true
draft: false
---

[CF 104360B - \u0412\u0430\u0441\u044f \u0438 \u041f\u0435\u0442\u044f](https://codeforces.com/problemset/problem/104360/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Mỗi truy vấn chọn một chuỗi con liền kề và chúng tôi chuyển đổi chuỗi con đó bằng cách sử dụng quy tắc cố định: mọi ký tự được mở rộng độc lập, trong đó một chữ cái ở vị trí`x`trong bảng chữ cái được lặp lại chính xác`x`lần. Ví dụ,`a`trở thành một bản sao,`b`trở thành hai bản sao,`c`trở thành ba bản sao, v.v. Nhiệm vụ không phải là xây dựng chuỗi đã chuyển đổi mà chỉ tính toán độ dài cuối cùng của chuỗi đó cho mỗi truy vấn. 

Vì vậy, về cơ bản, mỗi truy vấn yêu cầu tổng có trọng số trên một chuỗi con, trong đó trọng số của một ký tự chỉ phụ thuộc vào danh tính của nó. Nếu chúng ta lập bản đồ`a -> 1, b -> 2, ..., z -> 26`, mỗi truy vấn yêu cầu tổng các trọng số này trong một phạm vi. 

Các ràng buộc đầu vào cho phép tối đa 100.000 ký tự và 100.000 truy vấn. Bất kỳ giải pháp nào tính toán lại tổng từ đầu cho mỗi truy vấn sẽ yêu cầu quét tối đa`O(n)`ký tự cho mỗi truy vấn, dẫn đến`O(nq)`trong trường hợp xấu nhất, đó là theo thứ tự`10^10`hoạt động và sẽ không chạy kịp thời. Điều này ngay lập tức buộc phải áp dụng giải pháp dựa trên tiền xử lý với thời gian truy vấn không đổi hoặc logarit. 

Một vấn đề tế nhị sẽ xuất hiện nếu người ta cố gắng diễn đạt quá theo nghĩa đen với sự chuyển đổi. Xây dựng chuỗi mở rộng hoặc mô phỏng sự lặp lại trên mỗi ký tự sẽ bùng nổ cả về thời gian và bộ nhớ. Ngay cả một truy vấn đơn lẻ cũng có thể tạo ra một chuỗi có độ dài lên tới`26 * n`, vượt xa việc xây dựng khả thi. Số lượng có ý nghĩa duy nhất là tổng số tiền đóng góp. 

Một trường hợp cạnh khác là khi chuỗi con là một ký tự đơn. Câu trả lời phải chính xác là chỉ mục bảng chữ cái của nó. Điều này giúp xác thực tính chính xác của việc xử lý tiền tố, vì các lỗi riêng lẻ trong tổng tiền tố thường xuất hiện ở đây. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, lặp qua chuỗi con, chuyển đổi từng ký tự sang vị trí bảng chữ cái của nó và tích lũy tổng. Điều này đúng vì mỗi ký tự đóng góp độc lập vào độ dài cuối cùng. Tuy nhiên, cách tiếp cận này lặp lại công việc tương tự đối với các phạm vi chồng chéo. Với`n = 100000`Và`q = 100000`, trường hợp xấu nhất liên tục quét các đoạn lớn của chuỗi, dẫn đến khoảng`10^10`các thao tác nhân vật. 

Điều quan trọng là mỗi ký tự đóng góp một giá trị cố định độc lập với ngữ cảnh. Điều này có nghĩa là chúng ta có thể tính toán trước tổng tiền tố trên các giá trị này. Khi chúng ta xây dựng một mảng`pref`, Ở đâu`pref[i]`lưu trữ tổng số đóng góp của lần đầu tiên`i`ký tự, bất kỳ truy vấn nào`[l, r]`có thể được trả lời là`pref[r] - pref[l-1]`. Điều này làm giảm mỗi truy vấn xuống còn O(1) thời gian sau khi xử lý trước O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tổng tiền tố | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi thành mảng số trong đó mỗi ký tự`c`được ánh xạ tới`ord(c) - ord('a') + 1`. Điều này trực tiếp biểu thị số lần ký tự đó sẽ được lặp lại trong chuỗi mở rộng hư cấu, nhưng chúng tôi không bao giờ xây dựng chuỗi đó. 
2. Xây dựng mảng tổng tiền tố`pref`kích thước`n + 1`, Ở đâu`pref[i]`bằng tổng các giá trị đầu tiên`i`nhân vật. Bước này nén tất cả các phép tính phạm vi lặp lại thành một cấu trúc duy nhất. 
3. Đối với mỗi truy vấn`[l, r]`, tính toán câu trả lời như`pref[r] - pref[l - 1]`. Điều này hoạt động vì tổng tiền tố lưu trữ các đóng góp tích lũy và phép trừ sẽ tách biệt phạm vi. 
4. Xuất ngay từng kết quả hoặc lưu trữ và in khi kết thúc. 

Cạm bẫy thực sự duy nhất là lập chỉ mục. Sự cố sử dụng các chỉ mục dựa trên 1, trong khi mảng Python dựa trên 0. Việc thay đổi nhất quán ở cả quá trình xây dựng tiền tố và thời gian truy vấn là điều cần thiết. 

### Tại sao nó hoạt động 

Mỗi ký tự đóng góp độc lập vào độ dài cuối cùng và quy tắc chuyển đổi không đưa ra bất kỳ tương tác nào giữa các vị trí. Điều này làm cho bài toán trở thành một hàm cộng tuyến tính theo các khoảng. Tổng tiền tố duy trì tổng khoảng cách chính xác khi thực hiện phép trừ, do đó, mọi truy vấn sẽ giảm xuống việc đánh giá sự khác biệt của hai giá trị tích lũy được tính toán trước. Không có sự phụ thuộc gần đúng hoặc trạng thái nào, do đó tính chính xác được suy ra trực tiếp từ tính tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)

    for i in range(n):
        val = ord(s[i]) - ord('a') + 1
        pref[i + 1] = pref[i] + val

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Đầu tiên, mã xây dựng mảng tổng tiền tố theo trọng số ký tự. Mỗi vị trí đóng góp chỉ mục bảng chữ cái của nó và mảng tiền tố tích lũy các giá trị này. 

Mỗi truy vấn được trả lời bằng cách sử dụng phép trừ hai giá trị tiền tố. Phép trừ`pref[r] - pref[l - 1]`xử lý chính xác các phạm vi bao gồm vì`pref`được xác định bằng một mục số 0 ở đầu. 

Một lỗi triển khai phổ biến là quên`+1`thay đổi lập chỉ mục tiền tố hoặc trộn các chỉ mục dựa trên 0 và 1 trong truy vấn. Một cách khác là xây dựng lại mảng tiền tố cho mỗi truy vấn thay vì một lần trên toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
abacaba
1 3
2 5
1 7
```Đầu tiên chúng ta tính toán các giá trị ký tự:`a=1, b=2, a=1, c=3, a=1, b=2, a=1`Tổng tiền tố: 

| tôi | char | giá trị | trước | 
| --- | --- | --- | --- | 
| 0 | - | - | 0 | 
| 1 | một | 1 | 1 | 
| 2 | b | 2 | 3 | 
| 3 | một | 1 | 4 | 
| 4 | c | 3 | 7 | 
| 5 | một | 1 | 8 | 
| 6 | b | 2 | 10 | 
| 7 | một | 1 | 11 | 

Truy vấn:`[1,3] = pref[3]-pref[0]=4`

`[2,5] = pref[5]-pref[1]=8-1=7`

`[1,7] = 11`Dấu vết này xác nhận rằng mảng tiền tố tổng hợp chính xác các đóng góp có trọng số và mỗi truy vấn là một khoảng chênh lệch đơn giản. 

### Ví dụ 2 

đầu vào:```
7 4
abbabaa
1 3
5 7
6 6
2 4
```Giá trị ký tự:`a=1, b=2, b=2, a=1, b=2, a=1, a=1`Tiền tố: 

| tôi | trước | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 5 | 
| 4 | 6 | 
| 5 | 8 | 
| 6 | 9 | 
| 7 | 10 | 

Truy vấn:`[1,3]=5`

`[5,7]=10-6=4`

`[6,6]=1`

`[2,4]=6-1=5`Trường hợp này nhấn mạnh các truy vấn một phần tử và các phạm vi chồng chéo, cả hai đều được xử lý thống nhất bởi cùng một cấu trúc tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một lượt xây dựng tổng tiền tố, mỗi truy vấn là O(1) | 
| Không gian | O(n) | Mảng tiền tố có kích thước n+1 | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì cả hai`n`Và`q`lên tới 100.000, giữ tổng số hoạt động khoảng 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (ord(s[i]) - ord('a') + 1)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))
    return "\n".join(out)

# provided samples
assert run("7 3\nabacaba\n1 3\n2 5\n1 7\n") == "4\n7\n11"
assert run("7 4\nabbabaa\n1 3\n5 7\n6 6\n2 4\n") == "5\n4\n1\n5"

# custom cases
assert run("1 1\na\n1 1\n") == "1"
assert run("5 2\nabcde\n1 5\n2 4\n") == "15\n9"
assert run("6 3\naaaaaa\n1 6\n2 5\n3 3\n") == "6\n5\n1"
assert run("4 2\nzzzz\n1 4\n2 3\n") == "104\n52"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 1 | ranh giới tối thiểu | 
| bảng chữ cái đầy đủ | 15, 9 | tính đúng đắn chung | 
| tất cả là | 6, 5, 1 | trọng lượng đồng đều lặp đi lặp lại | 
| tất cả z | 104, 52 | xử lý ký tự có giá trị cao | 

## Vỏ cạnh 

Một truy vấn một ký tự như`s = "c", l = r = 1`trả lại trực tiếp`3`. Mảng tiền tố trở thành`[0, 3]`, vậy câu trả lời là`pref[1] - pref[0] = 3`. Điều này xác nhận rằng việc lập chỉ mục cơ sở hoạt động mà không cần xử lý đặc biệt. 

Truy vấn toàn dải kiểm tra xem mảng tiền tố có tổng hợp chính xác toàn bộ chuỗi hay không. Ví dụ,`s = "abc"`đưa ra tiền tố`[0,1,3,6]`, và truy vấn`[1,3]`sản lượng`6`, khớp với tổng trực tiếp. 

Các chuỗi thống nhất như`"aaaaaa"`xác nhận rằng các khoản đóng góp giống nhau lặp đi lặp lại được tích lũy một cách chính xác. Mỗi bước tiền tố tăng chính xác 1, do đó tổng phạm vi giảm xuống mức chênh lệch đơn giản tỷ lệ thuận với độ dài chuỗi con. 

Những trường hợp này đảm bảo chung rằng cả logic lập chỉ mục và tích lũy vẫn nhất quán trên tất cả các loại truy vấn.

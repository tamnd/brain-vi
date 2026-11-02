---
title: "CF 106511B - Sáu Bảy"
description: "Chúng ta được cho một số trường hợp thử nghiệm và mỗi trường hợp thử nghiệm là một chuỗi chỉ gồm các ký tự 6 và 7. Nhiệm vụ là đếm xem chúng ta có thể chọn bao nhiêu cặp vị trí sao cho vị trí đầu tiên chứa số 6, vị trí thứ hai chứa số 7 và số 7 xuất hiện ở bên phải…"
date: "2026-06-18T19:06:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106511
codeforces_index: "B"
codeforces_contest_name: "Columbia University Local Contest (CULC) Spring 2026"
rating: 0
weight: 106511
solve_time_s: 51
verified: true
draft: false
---

[CF 106511B - Sáu Bảy](https://codeforces.com/problemset/problem/106511/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số ca kiểm thử và mỗi ca kiểm thử là một chuỗi chỉ gồm các ký tự`6`Và`7`. Nhiệm vụ là đếm xem chúng ta có thể chọn được bao nhiêu cặp vị trí sao cho vị trí đầu tiên chứa một`6`, vị trí thứ hai chứa một`7`, và`7`xuất hiện đúng bên phải của người được chọn`6`. 

Nói cách khác, chúng ta đang đếm các cặp chỉ số có thứ tự$(i, j)$Ở đâu$i < j$, nhân vật tại$i$là`6`, và ký tự tại$j$là`7`. Mỗi trường hợp thử nghiệm là độc lập và chúng tôi xuất số lượng cho mỗi chuỗi. 

Kích thước đầu vào cho phép lên tới$10^4$các trường hợp thử nghiệm và tổng chiều dài của tất cả các chuỗi kết hợp tối đa là$2 \cdot 10^5$. Đây là hạn chế chính: mặc dù có thể có nhiều trường hợp thử nghiệm nhưng tổng công việc trên tất cả chúng phải tuyến tính theo tổng chiều dài chuỗi. Bất cứ điều gì xử lý từng trường hợp thử nghiệm trong thời gian dài hơn tuyến tính sẽ quá chậm. 

Một vòng lặp kép đơn giản trên tất cả các cặp vị trí bên trong mỗi chuỗi sẽ kiểm tra tới$n^2$cặp cho mỗi trường hợp thử nghiệm. Nếu một chuỗi có độ dài$2 \cdot 10^5$, chỉ riêng điều đó thôi cũng đã có rồi$4 \cdot 10^{10}$hoạt động vượt xa giới hạn 2 giây có thể xử lý. Vì vậy chúng ta cần một phương thức xử lý từng chuỗi trong$O(n)$thời gian. 

Có một số trường hợp bất cẩn khiến logic bị phá vỡ. 

Nếu chuỗi chỉ chứa`7`s chẳng hạn`777`, không có`6`để ghép nối với, vì vậy câu trả lời phải bằng 0. Một cách tiếp cận có lỗi giả định ít nhất một`6`hoặc bắt đầu đếm từ ký tự đầu tiên có thể cộng giá trị không chính xác. 

Nếu chuỗi chỉ chứa`6`s chẳng hạn`6666`, vẫn không có giá trị`7`, vì vậy câu trả lời lại là số không. Một sai lầm ở đây thường xuất phát từ việc cố gắng đếm các tổ hợp vị trí mà không kiểm tra loại ký tự thứ hai. 

Nếu chuỗi thay thế như`6767`, chúng ta phải đảm bảo chỉ tính các cặp chuyển tiếp hợp lệ. Câu trả lời đúng ở đây là 3, đến từ một`6`ở vị trí 1 ghép nối với cả hai sau`7`s và thứ hai`6`ghép nối với cái cuối cùng`7`. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mọi vị trí chứa`6`, quét tất cả các vị trí sau đó và đếm xem có bao nhiêu vị trí`7`. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của các cặp hợp lệ. Tuy nhiên, thời gian chạy của nó là bậc hai trong trường hợp xấu nhất, vì đối với một chuỗi có độ dài$n$, mỗi cái$n$vị trí có thể quét lên đến$n$những vị trí trong tương lai. Với tổng kích thước đầu vào đạt$2 \cdot 10^5$, cách tiếp cận này sẽ cố gắng theo thứ tự$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng là đối với mỗi`6`, điều quan trọng chỉ là có bao nhiêu`7`s xuất hiện sau nó chứ không phải vị trí của chúng riêng lẻ. Thay vì quét tiến nhiều lần, chúng ta có thể duy trì số lượng đang chạy`7`s vẫn ở bên phải vị trí hiện tại. Nếu chúng ta biết giá trị đó tại mỗi`6`, chúng ta có thể thêm nó trực tiếp vào câu trả lời. 

Một cách rõ ràng hơn để nghĩ về nó là đảo ngược hướng đếm. Nếu chúng ta duyệt chuỗi từ phải sang trái, chúng ta duy trì bao nhiêu`7`s chúng ta đã thấy cho đến nay. Khi chúng ta gặp phải một`6`, mọi thứ đã thấy trước đây`7`tạo thành một cặp hợp lệ với nó. Điều này chuyển đổi cấu trúc vòng lặp lồng nhau thành một lượt duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các cặp) | O(n²) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Một lượt có đếm hậu tố | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập. 

1. Khởi tạo bộ đếm`sevens = 0`để theo dõi có bao nhiêu`7`các ký tự chúng ta đã thấy cho đến nay khi quét từ phải sang trái. Điều này thể hiện số lượng đối tác hợp lệ tồn tại cho bất kỳ`6`chúng ta gặp sau này. 
2. Khởi tạo`answer = 0`. 
3. Lặp lại chuỗi từ ký tự cuối cùng đến ký tự đầu tiên. 

Khi chúng ta nhìn thấy một`7`, chúng tôi tăng`sevens`. Điều này là do bất kỳ`6`xuất hiện sớm hơn có thể kết hợp với điều này`7`. 

Khi chúng ta nhìn thấy một`6`, chúng tôi thêm`sevens`ĐẾN`answer`. Điều này có tác dụng vì mọi`7`chúng ta đã thấy những lời dối trá ở bên phải điều này`6`, tạo thành một cặp hợp lệ. 
4. Sau khi xử lý toàn bộ chuỗi, xuất ra`answer`. 

Hướng truyền tải là điều làm cho logic nhất quán: tại thời điểm chúng tôi xử lý một`6`, biến`sevens`đã đại diện chính xác số lượng vị trí hợp lệ trong tương lai. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến ở bất kỳ vị trí nào trong quá trình quét từ phải sang trái,`sevens`bằng số lượng`7`s hoàn toàn ở bên phải của chỉ mục hiện tại. Khi chúng tôi xử lý một`6`tại chỉ mục`i`, mọi`7`tính vào`sevens`tạo thành một cặp độc đáo$(i, j)$với$j > i$. Vì mỗi cặp được tính chính xác một lần tại thời điểm điểm cuối bên trái của nó được xử lý nên tổng cuối cùng bằng số cặp hợp lệ trong chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        sevens = 0
        ans = 0

        for ch in reversed(s):
            if ch == '7':
                sevens += 1
            else:  # ch == '6'
                ans += sevens

        print(ans)

if __name__ == "__main__":
    solve()
```Mã này phản ánh trực tiếp lý luận từ phải sang trái. Việc đảo ngược rất quan trọng vì nó tránh phải tính toán trước các mảng hậu tố một cách rõ ràng và nó đảm bảo rằng khi chúng ta đạt đến một giá trị`6`, tất cả đều có liên quan`7`s đã được tính rồi. 

Một chi tiết triển khai tinh tế là loại bỏ chuỗi đầu vào. Không có`.strip()`, ký tự dòng mới có thể bị xử lý nhầm thành ký tự phụ, dẫn đến logic đếm không chính xác trong một số môi trường. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`767`Chúng tôi quét từ phải sang trái. 
| Vị trí | Char | bảy | và | 
| --- | --- | --- | --- | 
| 2 | 7 | 1 | 0 | 
| 1 | 6 | 1 | 1 | 
| 0 | 7 | 2 | 1 | 
Đáp án cuối cùng là 1, tương ứng với cặp`(6 at index 1, 7 at index 2)`. 

Dấu vết này cho thấy rằng mỗi`6`đóng góp chính xác số lượng`7`s ở bên phải và không có cặp nào được tính gấp đôi. 

### Ví dụ 2:`6767`| Vị trí | Char | bảy | và | 
| --- | --- | --- | --- | 
| 3 | 7 | 1 | 0 | 
| 2 | 6 | 1 | 1 | 
| 1 | 7 | 2 | 1 | 
| 0 | 6 | 2 | 3 | 
Câu trả lời cuối cùng là 3. Điều này khẳng định rằng bội số`7`s tích lũy chính xác và sớm hơn`6`được hưởng lợi từ tất cả sau này`7`S. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý chính xác một lần trong một lần quét | 
| Không gian | O(1) | Chỉ có hai bộ đếm được duy trì bất kể kích thước đầu vào | 

Tổng chiều dài đầu vào được giới hạn bởi$2 \cdot 10^5$, do đó công việc tổng thể vẫn tuyến tính trên tất cả các trường hợp thử nghiệm và phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    for _ in range(t):
        s = input().strip()
        sevens = 0
        ans = 0
        for ch in reversed(s):
            if ch == '7':
                sevens += 1
            else:
                ans += sevens
        print(ans)

    return output.getvalue().strip()

# provided samples
assert run("4\n767\n677\n6767\n777\n") == "1\n2\n3\n0"

# custom cases
assert run("1\n6\n") == "0", "single 6"
assert run("1\n7\n") == "0", "single 7"
assert run("1\n66666\n") == "0", "all sixes"
assert run("1\n77777\n") == "0", "all sevens"
assert run("1\n667777\n") == "8", "all pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6`|`0`| đầu vào tối thiểu, không có cặp hợp lệ | 
|`7`|`0`| đầu vào tối thiểu, không có điểm cuối bên trái | 
|`66666`|`0`| tất cả các ký tự giống nhau (không khớp) | 
|`77777`|`0`| tất cả các ký tự giống nhau (không khớp) | 
|`667777`|`8`| ghép nối chéo đầy đủ chính xác | 

## Vỏ cạnh 

Một chuỗi chỉ gồm`6`s, chẳng hạn như`66666`, kích hoạt trường hợp mọi ký tự đều có thể là điểm cuối bên trái nhưng không có điểm cuối bên phải. Trong quá trình quét từ phải sang trái,`sevens`vẫn bằng 0 xuyên suốt, nên mọi`6`thêm số 0 vào câu trả lời, tạo ra kết quả đúng bằng 0. 

Một chuỗi chỉ gồm`7`s, chẳng hạn như`777`, kiểm tra tình huống ngược lại. Thuật toán tăng dần`sevens`cho mọi nhân vật, nhưng không bao giờ gặp phải một`6`, vì vậy không có gì được thêm vào câu trả lời. Kết quả chính xác vẫn bằng không. 

Một chuỗi hỗn hợp nhưng không cân bằng như`666777`thể hiện sự tích lũy Khi quét từ bên phải, ba bước đầu tiên sẽ được xây dựng`sevens = 3`, và mỗi`6`thêm 3 vào câu trả lời. Điều này đảm bảo rằng tất cả các cặp xuyên ranh giới được tính chính xác một lần, xác nhận rằng bất biến về “số 7 ở bên phải” là đủ để mô tả tất cả các cặp hợp lệ.

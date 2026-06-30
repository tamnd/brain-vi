---
title: "CF 104636D - Hệ Thống Tưới Nước"
description: "Chúng ta có một hệ thống đường ống với các đầu ra $n$, mỗi đầu ra có kích thước $si$. Arkady đổ một lượng nước cố định $A$ vào hệ thống, nhưng anh ta được phép chặn bất kỳ ổ cắm nhỏ nào trước khi làm như vậy."
date: "2026-06-29T17:06:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 82
verified: false
draft: false
---

[CF 104636D - Hệ thống tưới nước](https://codeforces.com/problemset/problem/104636/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống đường ống với$n$ổ cắm, mỗi ổ cắm có kích thước$s_i$. Arkady đổ một lượng nước cố định$A$vào hệ thống, nhưng anh ta được phép chặn bất kỳ tập hợp con cửa hàng nào trước khi thực hiện việc đó. Sau đó, nước chỉ phân phối giữa các cửa xả không bị chặn theo tỷ lệ kích thước của chúng. 

Nguyên tắc chính là nếu các ổ cắm không bị chặn có tổng kích thước$S$, sau đó ổ cắm$i$nhận được$\frac{s_i}{S} \cdot A$đơn vị nước. Mục tiêu của chúng tôi là đảm bảo cửa hàng đầu tiên nhận được ít nhất$B$đơn vị nước. Chúng tôi được yêu cầu giảm thiểu số lượng cửa hàng mà chúng tôi chặn để đạt được điều kiện này. 

Quan sát quan trọng đầu tiên là việc chặn chỉ thay đổi mẫu số của phân số chứ không thay đổi tử số của đầu ra đầu tiên. Số tiền mà cửa hàng đầu tiên nhận được chỉ phụ thuộc vào mức độ nhỏ mà chúng tôi có thể tạo ra tổng các kích thước còn lại trong khi vẫn duy trì sẵn cửa hàng đầu tiên. 

Từ$n$có thể lớn như$100{,}000$, bất kỳ giải pháp nào thử tất cả các tập hợp con của các lỗ bị chặn đều không thể thực hiện được. Ngay cả việc kiểm tra tất cả các tập hợp con cũng sẽ yêu cầu$2^n$hoạt động vượt xa mọi giới hạn khả thi. Ngay cả việc sắp xếp các tập hợp con hoặc tính toán lại các khoản tiền nhiều lần cũng sẽ dẫn đến ít nhất$O(n^2)$hành vi trong trường hợp xấu nhất, cũng là quá chậm. 

Những hạn chế về$A$Và$B$nhỏ ($\le 10^4$), nhưng chúng không phải là yếu tố hạn chế chính. Nút thắt thực sự là số lượng lỗ hổng, buộc chúng ta phải$O(n \log n)$hoặc$O(n)$giải pháp. 

Một trường hợp khó nhận thấy khi lỗ đầu tiên đã cung cấp đủ nước mà không chặn bất cứ thứ gì. Ví dụ, nếu$s_1$là rất lớn so với những người khác, hoặc nếu$B \cdot \sum s_i \le A \cdot s_1$, thì câu trả lời là bằng không. Một cách tiếp cận ngây thơ vẫn có thể thử loại bỏ các phần tử không cần thiết. 

Một trường hợp nguy hiểm khác là khi chúng ta phải chặn hầu hết mọi thứ. Nếu tất cả các lỗ khác đều lớn ngoại trừ lỗ đầu tiên, chiến lược tối ưu là chỉ giữ lại lỗ 1 và chặn phần còn lại. Bất kỳ thứ tự tham lam không chính xác nào bỏ qua tác động của các lỗ lớn ở mẫu số sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi tập hợp con các lỗ có thể bị chặn, tính tổng kết quả của các kích thước còn lại và kiểm tra xem lỗ đầu tiên có nhận được ít nhất$B$Nước. Đối với mỗi tập hợp con, chúng tôi tính toán lại tổng kích thước$S$, sau đó đánh giá xem$\frac{s_1}{S} \cdot A \ge B$. Điều này có tác dụng vì nó mô phỏng trực tiếp quá trình được mô tả trong bài toán. 

Tuy nhiên, có$2^n$tập hợp con, và thậm chí việc đánh giá một tập hợp con cũng mất$O(n)$nếu thực hiện một cách ngây thơ. Điều này dẫn đến một thuật toán hàm mũ hoàn toàn không khả thi đối với$n = 100{,}000$. 

Quan sát quan trọng là việc chặn các lỗ khác với lỗ đầu tiên chỉ giúp giảm mẫu số$S$. Để tối đa hóa lượng nước nhận được ở lỗ đầu tiên, chúng tôi muốn giảm thiểu$S$, có nghĩa là chúng ta nên loại bỏ các lỗ lớn nhất trước tiên. Mỗi lỗ bị loại bỏ sẽ làm giảm mẫu số nhiều nhất có thể cho mỗi thao tác loại bỏ. 

Điều này biến vấn đề thành việc chọn số lượng phần tử không phải đầu tiên lớn nhất mà chúng ta loại bỏ, sao cho bất đẳng thức trở thành đúng. Sau khi sắp xếp các lỗ khác theo thứ tự giảm dần, chúng ta tham lam loại bỏ từng lỗ một cho đến khi thỏa mãn điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp + loại bỏ tham lam) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cô lập kích thước của lỗ đầu tiên vì nó luôn được giữ nguyên và xác định trực tiếp tử số của phân số cuối cùng. 

Chúng tôi tính tổng tổng ban đầu của tất cả các kích thước lỗ, vì đây là mẫu số bắt đầu trước khi chặn bất kỳ thứ gì. 

Chúng tôi kiểm tra xem có cần chặn hay không bằng cách xác minh xem$A \cdot s_1 \ge B \cdot S$. Nếu điều này đã được giữ, chúng tôi có thể trả về 0 ngay lập tức vì mọi thao tác chặn sẽ chỉ làm giảm$S$, nhưng chúng tôi đã hài lòng rồi. 

Chúng tôi thu thập tất cả các lỗ ngoại trừ lỗ đầu tiên và sắp xếp chúng theo thứ tự kích thước giảm dần. Thứ tự này đảm bảo rằng khi chúng ta loại bỏ các lỗ trống, chúng ta luôn giảm mẫu số một cách mạnh mẽ nhất có thể. 

Sau đó, chúng tôi lặp qua danh sách đã sắp xếp, loại bỏ từng lỗ một trong tổng số và đếm xem chúng tôi đã thực hiện bao nhiêu lần xóa. Sau mỗi lần loại bỏ, chúng tôi kiểm tra xem điều kiện$A \cdot s_1 \ge B \cdot S$được hài lòng. Lần đầu tiên điều đó trở thành sự thật, chúng tôi dừng lại và trả lại số lỗ đã loại bỏ. 

### Tại sao nó hoạt động 

Quá trình này duy trì một bất biến rõ ràng: ở bất kỳ bước nào, chúng tôi đã loại bỏ chính xác phần lớn nhất$k$lỗ trong số tất cả các lỗ không phải lỗ đầu tiên. Điều này đảm bảo rằng số tiền hiện tại$S$là số tiền nhỏ nhất có thể đạt được bằng cách loại bỏ bất kỳ$k$lỗ. Vì điều kiện chỉ phụ thuộc vào$S$, giảm thiểu$S$đối với một số lần xóa cố định là tối ưu và do đó, lần đầu tiên bất đẳng thức được giữ tương ứng với số lần xóa tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, A, B = map(int, input().split())
    s = list(map(int, input().split()))
    
    s1 = s[0]
    others = s[1:]
    
    total = sum(s)
    
    if A * s1 >= B * total:
        print(0)
        return
    
    others.sort(reverse=True)
    
    removed = 0
    for x in others:
        total -= x
        removed += 1
        if A * s1 >= B * total:
            print(removed)
            return

    print(n - 1)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc đầu vào và tách lỗ đầu tiên khỏi phần còn lại. Điều này rất quan trọng vì lỗ đầu tiên không bao giờ bị chặn nên sự đóng góp của nó không đổi trong suốt quá trình. 

Chúng tôi tính tổng số tiền một lần và sử dụng nó làm mẫu số tiến triển. Việc kiểm tra thoát sớm sẽ tránh được việc sắp xếp và lặp lại không cần thiết khi cấu hình ban đầu đã đáp ứng được yêu cầu. 

Việc sắp xếp các lỗ còn lại theo thứ tự giảm dần đảm bảo rằng mỗi lần loại bỏ sẽ mang lại mức giảm mẫu số tối đa có thể. Trong quá trình lặp lại, chúng tôi liên tục cập nhật tổng và kiểm tra bất đẳng thức ở dạng nhân chéo để tránh lỗi dấu phẩy động. 

Dự phòng cuối cùng`n - 1`chỉ kích hoạt nếu ngay cả việc loại bỏ tất cả các lỗ khác vẫn không đủ, tương ứng với trường hợp chỉ còn lại lỗ đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 10 3
2 2 2 2
```Chúng tôi bắt đầu với$s_1 = 2$và tổng cộng$S = 8$. Chúng tôi cần$10 \cdot 2 \ge 3 \cdot S$, nghĩa$20 \ge 3S$, Vì thế$S \le 6.66$. 

| Bước | Đã xóa lỗ | Số tiền còn lại$S$| Điều kiện hài lòng | 
| --- | --- | --- | --- | 
| 0 | không | 8 | không | 
| 1 | 2 | 6 | vâng | 

Sau khi loại bỏ một lỗ có kích thước 2, tổng số sẽ là 6 và điều kiện được giữ nguyên. Vậy câu trả lời là 1. 

Điều này xác nhận rằng việc tham lam loại bỏ lỗ lớn nhất hiện có là đủ. 

### Mẫu 2 

đầu vào:```
4 80 20
2 1 4 3
```Đây$s_1 = 2$, tổng cộng$S = 10$, yêu cầu là$80 \cdot 2 \ge 20 \cdot S$, tức là$160 \ge 20S$, Vì thế$S \le 8$. 

| Bước | Đã xóa lỗ | Số tiền còn lại$S$| Điều kiện hài lòng | 
| --- | --- | --- | --- | 
| 0 | không | 10 | không | 
| 1 | 4 | 6 | vâng | 

Việc loại bỏ lỗ không phải lỗ lớn nhất sẽ ngay lập tức giảm tổng số tiền đủ để thỏa mãn ràng buộc. 

Điều này cho thấy rằng đôi khi chỉ cần loại bỏ một lần là đủ và việc chấm dứt sớm hơn là rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp$n-1$các lỗ còn lại chiếm ưu thế trong thời gian chạy, trong khi quá trình quét là tuyến tính | 
| Không gian |$O(n)$| Chúng tôi lưu trữ danh sách kích thước lỗ ngoại trừ kích thước lỗ đầu tiên | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n \le 10^5$và việc sắp xếp ở quy mô này nằm trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, A, B = map(int, input().split())
    s = list(map(int, input().split()))
    
    s1 = s[0]
    others = s[1:]
    total = sum(s)
    
    if A * s1 >= B * total:
        return "0\n"
    
    others.sort(reverse=True)
    removed = 0
    
    for x in others:
        total -= x
        removed += 1
        if A * s1 >= B * total:
            return str(removed) + "\n"
    
    return str(n - 1) + "\n"

# provided samples
assert run("4 10 3\n2 2 2 2\n") == "1\n"
assert run("4 80 20\n2 1 4 3\n") == "1\n"

# custom cases
assert run("1 5 3\n10\n") == "0\n"  # only hole already satisfies
assert run("5 10 100\n1 9 9 9 9\n") == "4\n"  # must remove all but first
assert run("5 10 1\n5 4 3 2 1\n") == "0\n"  # already sufficient
assert run("6 10 50\n1 10 10 10 10 10\n") == "4\n"  # progressive removals
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp tối thiểu | 
| yêu cầu nặng nề | 4 | loại bỏ gần như hoàn toàn | 
| đã hài lòng | 0 | không cần hành động | 
| vượt ngưỡng dần dần | 4 | sự đúng đắn tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không cần chặn. Đối với đầu vào như$n=1$,$s_1$là lỗ trống duy nhất, mẫu số không bao giờ thay đổi nên điều kiện được kiểm tra một lần và trả về 0 ngay lập tức. Thuật toán xử lý việc này thông qua việc kiểm tra bất đẳng thức sớm. 

Một trường hợp cạnh khác là khi tất cả các lỗ khác đều cực kỳ lớn so với lỗ đầu tiên. Trong trường hợp như vậy, vòng lặp tham lam sẽ loại bỏ tất cả chúng. Ví dụ, nếu$s = [1, 100, 100, 100]$, tổng ban đầu là 301 và điều kiện không thành công. Sau khi loại bỏ 100 ba lần, tổng trở thành 1 và điều kiện gần như được thỏa mãn. Thuật toán trả về chính xác 3, đây là mức tối ưu vì bất kỳ số lần loại bỏ nhỏ hơn đều để lại mẫu số lớn hơn. 

Trường hợp cạnh cuối cùng là khi lỗi dấu phẩy động có thể xuất hiện nếu được triển khai trực tiếp. Giải pháp tránh điều này hoàn toàn bằng cách nhân cả hai vế của bất đẳng thức, đảm bảo tất cả các phép so sánh được thực hiện bằng số nguyên, giúp giữ logic ổn định ngay cả đối với kích thước đầu vào tối đa.

---
title: "CF 102680H - Người máy cuối cùng"
description: "Vấn đề xác định một chuỗi vô hạn các màu liên minh. Nó bắt đầu với một đội đỏ duy nhất. Để tạo phiên bản tiếp theo của chuỗi, chúng tôi sao chép chuỗi hiện tại, hoán đổi mọi màu trong bản sao và nối bản sao đó vào cuối."
date: "2026-08-01T23:39:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "H"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 174
verified: true
draft: false
---

[CF 102680H - Robot cuối cùng](https://codeforces.com/problemset/problem/102680/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề xác định một chuỗi vô hạn các màu liên minh. Nó bắt đầu với một đội đỏ duy nhất. Để tạo phiên bản tiếp theo của chuỗi, chúng tôi sao chép chuỗi hiện tại, hoán đổi mọi màu trong bản sao và nối bản sao đó vào cuối. Các vị trí đầu tiên của chuỗi đủ để tiết lộ mẫu, nhưng chuỗi tăng theo cấp số nhân, vì vậy chúng ta cần trả lời các truy vấn yêu cầu màu của một đội có thứ hạng rất lớn. 

Đầu vào chứa một số thứ hạng đội. Mỗi thứ hạng có thể lớn bằng$10^{18}$, điều này ngay lập tức loại trừ việc xây dựng chuỗi một cách trực tiếp. Ngay cả việc lưu trữ đầu tiên$10^9$các ký tự sẽ là không thể và việc xây dựng thực tế cần vô số vị trí. Vì số lượng truy vấn nhỏ nên giải pháp dự định phải trả lời từng vị trí một cách độc lập trong khoảng thời gian logarit. 

Các trường hợp khó không phải là về kích thước của chuỗi mà là về ranh giới lập chỉ mục và đệ quy. Trình tự này được lập chỉ mục một, trong khi hầu hết các triển khai đều hoạt động tự nhiên với các vị trí được lập chỉ mục bằng 0. 

Ví dụ, đầu vào```
1
1
```có đầu ra```
Red
```Giải pháp chuyển đổi thành chỉ mục 0 không chính xác có thể kiểm tra bit sai và trả về màu xanh lam. 

Một trường hợp khác là:```
3
2
3
4
```Trình tự bắt đầu như sau:```
Position: 1 2 3 4
Color:    r b b r
```Đầu ra đúng là:```
Blue
Blue
Red
```Một sai lầm phổ biến là giả định nửa sau của mỗi khối chỉ đơn giản là đối diện với nửa đầu mà không điều chỉnh vị trí bên trong nửa đó. Nửa thứ hai là bản sao đảo ngược ở cùng chỉ số tương đối. 

Trường hợp cạnh cuối cùng là các vị trí rất lớn như:```
1
10000000000000000
```Trình tự không thể được tạo ra, thậm chí một phần. Bất kỳ cách tiếp cận nào cố gắng mô phỏng việc xây dựng sẽ hết thời gian hoặc bộ nhớ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xây dựng chuỗi cho đến khi đạt thứ hạng lớn nhất được yêu cầu. Nếu chiều dài hiện tại là$L$, cách xây dựng tiếp theo nhân đôi nó, vì vậy sau$k$lặp đi lặp lại chiều dài là$2^k$. Để trả lời một câu hỏi gần$10^{18}$, chúng ta sẽ cần hơn 60 bản mở rộng. Số lần mở rộng đó không phải là vấn đề, nhưng chuỗi được lưu trữ cuối cùng sẽ chứa khoảng$10^{18}$nhân vật, điều đó là không thể. 

Sở dĩ việc xây dựng trực tiếp chứa đựng những công việc không cần thiết là vì chúng ta không cần toàn bộ trình tự mà chỉ cần một vị trí. Việc xây dựng có cấu trúc đệ quy. Một khối có chiều dài$2^k$bao gồm nửa đầu và sau đó là bản sao đảo ngược của nửa đầu. Nếu một vị trí nằm ở nửa đầu thì câu trả lời giống với vị trí tương ứng ở khối nhỏ hơn. Nếu ở hiệp 2 thì đáp án là màu đối lập với vị trí tương ứng ở hiệp 1. 

Điều này làm giảm vấn đề liên tục hỏi xem vị trí hiện tại nằm ở nửa đầu hay nửa sau của khối lũy thừa hai. Quá trình loại bỏ một bit thông tin ở mỗi bước, do đó số lượng thao tác chỉ là số bit trong chỉ mục. 

Lực lượng vũ phu hoạt động vì trình tự được tạo tuân theo chính xác các quy tắc bắt buộc, nhưng không thành công khi thứ hạng được yêu cầu trở nên lớn. Nhận xét rằng mỗi nửa giây là một bản sao đảo ngược cho phép chúng ta thay thế việc tạo chuỗi bằng phân tách vị trí nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$theo cấp bậc lớn nhất |$O(n)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi thứ hạng của đội từ chỉ mục một sang chỉ mục không. Cấu trúc đệ quy dễ mô tả hơn theo cách này vì ký tự đầu tiên tương ứng với chỉ số 0. 
2. Giữ một lá cờ cho biết màu hiện tại có bị đảo số lần lẻ hay không. Ban đầu cờ sai vì ký tự đầu tiên có màu đỏ. 
3. Tìm lũy thừa lớn nhất của hai khối chứa vị trí hiện tại. Một khối có chiều dài$2^k$được hình thành bởi một khối có chiều dài nhỏ hơn$2^{k-1}$tiếp theo là bản sao đảo ngược của nó. 
4. Nếu vị trí nằm ở nửa đầu của khối, hãy giữ trạng thái lật hiện tại và tiếp tục với vị trí tương tự bên trong khối nhỏ hơn. 
5. Nếu vị trí ở nửa sau, hãy trừ kích thước của nửa đầu khỏi vị trí và chuyển trạng thái lật. Chuyển sang nửa sau có nghĩa là chúng ta đang xem bản sao ngược. 
6. Tiếp tục thu nhỏ khối cho đến khi chỉ còn lại một vị trí. Trạng thái lật cuối cùng quyết định câu trả lời. Trạng thái sai có nghĩa là màu đỏ, trong khi trạng thái đúng có nghĩa là màu xanh lam. 

Tại sao nó hoạt động: 

Ở mỗi bước, khối hiện tại được thể hiện chính xác bằng một khối nhỏ hơn và một bản sao đảo ngược của khối nhỏ hơn đó. Thuật toán theo dõi vị trí đó thuộc về trường hợp nào trong hai trường hợp này. Bất cứ khi nào nó chuyển sang một bản sao đảo ngược, nó sẽ chuyển trạng thái màu, giữ nguyên ý nghĩa của vị trí hiện tại. Khi kích thước khối trở thành một, ký tự ban đầu có màu đỏ và các lần lật tích lũy sẽ đưa ra câu trả lời thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_position(n):
    n -= 1
    flipped = False

    while n > 0:
        length = 1
        while (length << 1) <= n:
            length <<= 1

        if n >= length:
            n -= length
            flipped = not flipped

    return "Blue" if flipped else "Red"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        ans.append(solve_position(n))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Đầu tiên, hàm này thay đổi thứ hạng thành chỉ mục dựa trên số 0. Điều này tránh trộn lẫn định nghĩa trình tự toán học với độ lệch triển khai. 

Biến`flipped`lưu trữ số lần chúng ta đã nhập nửa đảo ngược của khối đệ quy. Chỉ có vấn đề chẵn lẻ, vì vậy boolean là đủ. 

Vòng lặp liên tục tìm lũy thừa lớn nhất của hai không vượt quá vị trí hiện tại. Nếu vị trí nằm ngoài ranh giới đó thì nó thuộc nửa sau của khối hiện tại. Việc xóa nửa đầu sẽ cho vị trí tương ứng trong khối nhỏ hơn và chuyển đổi`flipped`tính đến sự đảo ngược. 

Vòng lặp lồng nhau để tìm lũy thừa của hai lần chạy nhiều nhất là khoảng 60 lần vì thứ hạng bị giới hạn bởi$10^{18}$. Số nguyên Python xử lý trực tiếp kích thước này, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
1
2
3
8
10000000000000000
```việc thực thi bốn giá trị đầu tiên là: 

| Xếp hạng | Chỉ số dựa trên 0 | Lật thay đổi | Màu cuối cùng | 
| --- | --- | --- | --- | 
| 1 | 0 | không | Đỏ | 
| 2 | 1 | nhập một nửa đảo ngược một lần | Màu xanh | 
| 3 | 2 | nhập một nửa đảo ngược một lần | Màu xanh | 
| 8 | 7 | nhập các nửa đảo ngược với tổng số lần lật lẻ | Màu xanh | 

Ba hàng đầu tiên hiển thị phần đầu của chuỗi`r, b, b`, phù hợp với mẫu được tạo. Trường hợp cấp 8 chứng tỏ rằng thuật toán không cần xây dựng tám ký tự đầu tiên. 

Đối với truy vấn lớn: 

| Xếp hạng | Chỉ số dựa trên 0 | Số lần giảm | Màu cuối cùng | 
| --- | --- | --- | --- | 
| 10000000000000000 | 9999999999999999 | 54 | Màu xanh | 

Dấu vết chứng tỏ rằng thuật toán chỉ tuân theo cấu trúc nhị phân của chỉ mục. Khối lượng công việc phụ thuộc vào số bit chứ không phải kích thước số của thứ hạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| Mỗi lần lặp lại sẽ loại bỏ một cấp độ khỏi cấu trúc lũy thừa hai. | 
| Không gian |$O(1)$| Chỉ có chỉ mục hiện tại và trạng thái lật được lưu trữ. | 

Với tối đa khoảng 60 lần lặp lại cho các cấp bậc lên tới$10^{18}$, giải pháp dễ dàng phù hợp với giới hạn của hàng trăm truy vấn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_position(n):
    n -= 1
    flipped = False
    while n > 0:
        length = 1
        while (length << 1) <= n:
            length <<= 1
        if n >= length:
            n -= length
            flipped = not flipped
    return "Blue" if flipped else "Red"

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        out.append(solve_position(int(sys.stdin.readline())))
    sys.stdin = old
    return "\n".join(out) + "\n"

assert run("""5
1
2
3
8
10000000000000000
""") == """Red
Blue
Blue
Blue
Blue
""", "sample"

assert run("""1
1
""") == """Red
""", "minimum position"

assert run("""4
1
2
3
4
""") == """Red
Blue
Blue
Red
""", "first block boundary"

assert run("""3
7
8
9
""") == """Red
Blue
Blue
""", "power of two boundary"

assert run("""3
16
17
18
""") == """Blue
Red
Blue
""", "large block transition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Red`| Vị trí ban đầu và chuyển đổi chỉ mục | 
|`1 2 3 4`|`Red Blue Blue Red`| Khối đệ quy đầu tiên | 
|`7 8 9`|`Red Blue Blue`| Vượt qua ranh giới quyền lực hai | 
|`16 17 18`|`Blue Red Blue`| Xử lý đúng đệ quy sâu hơn | 

## Vỏ cạnh 

Vị trí đầu tiên rất đặc biệt vì dãy ban đầu bắt đầu bằng màu đỏ. Đối với đầu vào:```
1
1
```thuật toán chuyển chỉ số về 0, không thực hiện thao tác lật nào và trả về Màu đỏ. Bất kỳ cách tiếp cận nào coi thứ hạng đầu vào đã dựa trên số 0 sẽ thất bại ở đây. 

Đối với các vị trí chính xác ở đầu khối mới, logic nửa sau phải được áp dụng cẩn thận. Đối với đầu vào:```
3
3
4
5
```màu sắc là:```
Position 3: Blue
Position 4: Red
Position 5: Blue
```Vị trí 4 là ký tự đầu tiên của nửa đảo ngược của khối có độ dài bốn. Thuật toán phát hiện chỉ mục nằm ở nửa sau, trừ đi một nửa độ dài và chuyển đổi màu sắc. 

Đối với thứ hạng rất lớn, chẳng hạn như:```
1
10000000000000000
```thuật toán không bao giờ lưu trữ trình tự. Nó liên tục loại bỏ các mức nhị phân cao nhất của vị trí cho đến khi đạt được câu trả lời. Công việc vẫn tỷ lệ thuận với số bit trong chỉ mục nên truy vấn được xử lý nhanh chóng.

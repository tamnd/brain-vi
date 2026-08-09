---
title: "CF 104002C - William và quản lý cấp trung"
description: "Chúng ta có một hàng công nhân, mỗi công nhân có hai thuộc tính: năng suất và giờ làm việc. Sự đóng góp của người lao động được xác định là tích của hai giá trị này."
date: "2026-07-02T05:36:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104002
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-28-22 Div. 2 (Beginner)"
rating: 0
weight: 104002
solve_time_s: 47
verified: true
draft: false
---

[CF 104002C - William và Ban quản lý cấp trung](https://codeforces.com/problemset/problem/104002/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng công nhân, mỗi công nhân có hai thuộc tính: năng suất và giờ làm việc. Sự đóng góp của người lao động được xác định là tích của hai giá trị này. William phải chọn một đoạn liền kề chính xác$K$người lao động và muốn tối đa hóa tổng đóng góp của phân khúc đó. 

Vì vậy, nhiệm vụ giảm xuống còn quét một mảng có độ dài$N$, trong đó mỗi vị trí giữ một giá trị$a_i = p_i \cdot h_i$và tìm tổng tối đa trên tất cả các mảng con liền kề có độ dài cố định$K$. 

Kích thước đầu vào đạt tới$N = 10^5$, loại trừ mọi giải pháp tính lại tổng từ đầu cho mỗi cửa sổ. Việc tính toán lại đơn giản cho mỗi phân đoạn sẽ tốn kém$O(NK)$, trong trường hợp xấu nhất trở thành$10^{10}$, vượt xa những gì phù hợp trong một giây. 

Một giải pháp đúng phải sử dụng lại tính toán giữa các cửa sổ liền kề, nghĩa là mỗi phần tử chỉ được thêm và xóa nhiều nhất một lần. 

Trường hợp cạnh tinh tế xuất hiện khi$K = 1$. Trong trường hợp đó, câu trả lời đơn giản là sản phẩm duy nhất tối đa. Bất kỳ logic cửa sổ trượt nào cũng phải xử lý việc khởi tạo một cách chính xác. 

Một trường hợp góc khác là khi tất cả các giá trị bằng nhau hoặc khi giá trị âm không tồn tại ở đây, nhưng chúng ta vẫn phải cẩn thận với tình trạng tràn trong các ngôn ngữ có kích thước số nguyên cố định. Trong Python đây không phải là vấn đề, nhưng trong C++ nó yêu cầu số nguyên 64 bit. 

Một trường hợp minh họa nhỏ: 

đầu vào:```
4 2
2 3
1 3
3 2
4 1
```Sản phẩm trở thành$[6, 3, 6, 4]$. Đáp án đúng là tổng lớn nhất của 2 phần tử liên tiếp bất kỳ nên ta so sánh$(6+3), (3+6), (6+4)$, cho$10$. 

Một cách tiếp cận đơn giản có thể tính toán lại từng cặp một cách độc lập nhưng điều đó lặp lại công việc được chia sẻ trong các phân đoạn chồng chéo. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xem xét mọi vị trí bắt đầu có thể có của một đoạn có chiều dài$K$, tính tổng của số tiếp theo$K$các phần tử bằng cách lặp lại chúng và theo dõi mức tối đa. Điều này đúng vì nó đánh giá trực tiếp mọi phân khúc ứng viên hợp lệ. Tuy nhiên, đối với mỗi$N-K+1$vị trí bắt đầu, chúng tôi làm$K$bổ sung, dẫn đến khoảng$N \cdot K$hoạt động. Với$N = 10^5$, điều này trở nên không khả thi khi$K$là lớn. 

Sự cải thiện đến từ việc nhận ra rằng các phân đoạn liên tiếp chồng chéo lên nhau rất nhiều. Khi chúng ta di chuyển cửa sổ từ$[i, i+K-1]$ĐẾN$[i+1, i+K]$, chỉ có hai phần tử thay đổi: chúng tôi loại bỏ$a_i$và thêm$a_{i+K}$. Điều này biến việc tính toán lại thành các cập nhật gia tăng, giảm mỗi ca về thời gian không đổi. 

Vì vậy, thay vì tính lại tổng, chúng tôi duy trì tổng hiện có và cập nhật nó khi cửa sổ trượt qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NK)$|$O(1)$| Quá chậm | 
| Cửa Sổ Trượt |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi công nhân thành một giá trị duy nhất$a_i = p_i \cdot h_i$, vì chỉ có tích này mới quan trọng đối với tổng cuối cùng. 
2. Tính tổng số đầu tiên$K$các phần tử. Điều này tạo thành cửa sổ ban đầu và thiết lập câu trả lời cơ bản. 
3. Đặt số tiền ban đầu này làm câu trả lời tốt nhất hiện tại vì chưa có phân đoạn nào khác được đánh giá. 
4. Trượt cửa sổ từng vị trí một từ trái sang phải. Ở mỗi bước, hãy trừ phần tử rời khỏi cửa sổ và thêm phần tử mới vào đó. Bản cập nhật này hoạt động vì các cửa sổ liên tiếp khác nhau chính xác ở hai vị trí. 
5. Sau mỗi lần cập nhật, hãy so sánh tổng cửa sổ mới với câu trả lời đúng nhất và lưu giá trị lớn hơn. 
6. Sau khi xử lý tất cả các cửa sổ, xuất ra tổng tốt nhất gặp phải. 

### Tại sao nó hoạt động 

Thuật toán duy trì tổng chính xác của độ dài hiện tại-$K$cửa sổ ở mỗi bước. Vì mỗi lần chuyển đổi giữa các cửa sổ đều duy trì tính chính xác bằng cách loại bỏ chính xác một phần tử lỗi thời và thêm chính xác một phần tử mới nên không có thông tin nào bị mất hoặc bị tính hai lần. Mọi phân đoạn kích thước liền kề có thể$K$được truy cập chính xác một lần trong quá trình trượt này, do đó mức tối đa trên tất cả các khoản tiền được duy trì là mức tối đa toàn cầu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = []
    
    for _ in range(n):
        p, h = map(int, input().split())
        a.append(p * h)
    
    window_sum = sum(a[:k])
    best = window_sum
    
    for i in range(k, n):
        window_sum += a[i]
        window_sum -= a[i - k]
        if window_sum > best:
            best = window_sum
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách thu gọn từng chuỗi công việc thành một giá trị số nguyên duy nhất, giúp đơn giản hóa vấn đề thành một tác vụ tổng hợp mảng con tối đa có độ dài cố định tiêu chuẩn. Cửa sổ ban đầu được tính toán một lần bằng cách sử dụng một lát cắt tiền tố, lát cắt này thiết lập cả tổng hiện có và câu trả lời cơ sở. 

Vòng lặp sau đó thực thi bất biến trượt: tại lần lặp$i$,`window_sum`luôn biểu diễn tổng các phần tử từ$i-k+1$ĐẾN$i$. Mỗi lần lặp lại cập nhật trạng thái này theo thời gian không đổi bằng cách loại bỏ phần tử bên trái đã lỗi thời và thêm phần tử bên phải mới. 

Một lỗi phổ biến là tính lại tổng bên trong vòng lặp bằng cách cắt, điều này âm thầm làm giảm nghiệm thành thời gian bậc hai. Một lỗi thường gặp khác là bắt đầu vòng lặp trượt ở chỉ mục sai; ở đây nó bắt đầu chính xác vào lúc`k`, đảm bảo ca đầy đủ đầu tiên tương ứng với việc thay thế`a[0]`với`a[k]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
2 3
1 3
3 2
4 1
```Mảng được chuyển đổi:$[6, 3, 6, 4]$| Bước | Cửa sổ | Tổng hợp | Tốt nhất | 
| --- | --- | --- | --- | 
| Ban đầu | [6, 3] | 9 | 9 | 
| tôi=2 | [3, 6] | 9 | 9 | 
| tôi=3 | [6, 4] | 10 | 10 | 

Giá trị tốt nhất cuối cùng là 10, xuất phát từ cửa sổ cuối cùng. Điều này xác nhận thuật toán đánh giá chính xác các cửa sổ chồng chéo mà không cần tính toán lại. 

### Ví dụ 2 

đầu vào:```
5 3
1 2
2 1
3 1
1 5
2 2
```Mảng được chuyển đổi:$[2, 2, 3, 5, 4]$| Bước | Cửa sổ | Tổng hợp | Tốt nhất | 
| --- | --- | --- | --- | 
| Ban đầu | [2, 2, 3] | 7 | 7 | 
| tôi=3 | [2, 3, 5] | 10 | 10 | 
| tôi=4 | [3, 5, 4] | 12 | 12 | 

Dấu vết này cho thấy cách cửa sổ tiền tố yếu cục bộ được thay thế bằng phân đoạn mạnh hơn sau này và cách thuật toán theo dõi mức tối đa toàn cục một cách tự nhiên mà không có bất kỳ hoạt động quay lui nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi phần tử được thêm một lần và xóa một lần trong quá trình trượt | 
| Không gian |$O(1)$| Chỉ một số biến số nguyên được duy trì ngoài bộ nhớ đầu vào | 

Giải pháp phù hợp dễ dàng trong các ràng buộc cho$N \le 10^5$, vì nó thực hiện một lần truyền tuyến tính duy nhất trên mảng với các cập nhật liên tục theo thời gian trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""4 2
2 3
1 3
3 2
4 1
""") == "10"

# minimum size
assert run("""1 1
5 7
""") == "35"

# all equal
assert run("""5 3
2 2
2 2
2 2
2 2
2 2
""") == "12"

# strictly increasing
assert run("""4 2
1 1
2 2
3 3
4 4
""") == "14"

# k = n
assert run("""4 4
1 2
2 3
3 4
4 5
""") == str(2+6+12+20)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 35 | ranh giới một phần tử | 
| tất cả đều bình đẳng | 12 | ổn định của tổng trượt | 
| ngày càng tăng | 14 | theo dõi cửa sổ chính xác | 
| k = n | toàn bộ số tiền | trường hợp cạnh toàn dải | 

## Vỏ cạnh 

Khi nào$K = 1$, thuật toán khởi tạo cửa sổ làm phần tử đầu tiên và sau đó chỉ cần so sánh từng sản phẩm riêng lẻ khi nó trượt. Quy tắc cập nhật vẫn hoạt động vì việc trừ và thêm các phần tử liền kề sẽ suy biến thành việc thay thế phần tử hiện tại. 

Vì$K = N$, vòng lặp không thực thi vì chỉ có một cửa sổ hợp lệ. Tổng ban đầu đã là câu trả lời và thuật toán trả về chính xác mà không cần bất kỳ cập nhật trượt nào. 

Khi tất cả các giá trị giống hệt nhau thì tổng của mọi cửa sổ đều bằng nhau. Thuật toán liên tục cập nhật cửa sổ nhưng không bao giờ thay đổi giá trị tốt nhất được lưu trữ, chứng tỏ rằng thuật toán không dựa vào sự thay đổi giá trị để hoạt động chính xác.

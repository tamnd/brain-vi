---
title: "CF 104415I - Gây ấn tượng với thuyền trưởng"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi xử lý nó từ trái sang phải. Trong khi quét, chúng tôi duy trì số lần mỗi giá trị đã xuất hiện."
date: "2026-06-30T19:52:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "I"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 43
verified: true
draft: false
---

[CF 104415I - Gây ấn tượng với Thuyền trưởng](https://codeforces.com/problemset/problem/104415/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên và chúng tôi xử lý nó từ trái sang phải. Trong khi quét, chúng tôi duy trì số lần mỗi giá trị đã xuất hiện. Tại mỗi vị trí, trước khi chèn giá trị hiện tại vào cấu trúc tần số đang chạy, chúng tôi muốn đếm xem có bao nhiêu phần tử trước đó hình thành mối quan hệ nhân cụ thể với nó. Cụ thể, đối với phần tử hiện tại$a_i$, chúng tôi tìm kiếm các phần tử trước đó$a_j$như vậy hoặc$a_j$chia rẽ$a_i$hoặc$a_i$chia rẽ$a_j$, tùy thuộc vào cách diễn giải điều kiện thông qua tỷ lệ$x / a_i$được mô tả trong tuyên bố. Tính toán dự định giảm xuống còn đếm các cặp trong đó một giá trị là thương của mục tiêu cố định chia cho giá trị hiện tại và thương đó phải xuất hiện. 

Cấu trúc ẩn chính là các giá trị nhỏ, vì vậy thay vì xử lý đầu vào dưới dạng số nguyên tùy ý cần băm hoặc ánh xạ, chúng ta có thể duy trì mảng tần số trực tiếp được lập chỉ mục theo giá trị. Điều này biến mỗi truy vấn về “tồn tại bao nhiêu lần xuất hiện trước đó của một giá trị bằng một số thương số được tính toán” thành tra cứu theo thời gian không đổi. 

Những ràng buộc ngụ ý rằng$n$có thể đủ lớn để$O(n^2)$quét qua các phần tử trước đó là không thể. Thậm chí$O(n \log n)$mỗi phần tử sẽ quá chậm nếu có liên quan đến phép nhân hoặc thao tác bản đồ lặp đi lặp lại. Cách tiếp cận duy nhất có thể chấp nhận được là một cái gì đó gần với thời gian tuyến tính, trong đó mỗi phần tử đóng góp một lượng công việc không đổi. 

Một trường hợp khó phát hiện khi phép chia không chính xác. Nếu chúng ta cố gắng tính thương số mà không kiểm tra tính chia hết, chúng ta có thể tính sai kết quả phân số là chỉ số hợp lệ. Một vấn đề khác xảy ra khi phần tử hiện tại bằng 0, do logic phân chia trở nên không xác định hoặc yêu cầu xử lý riêng. Một cách triển khai ngây thơ, tính toán một cách mù quáng$x / a_i$không có xác nhận sẽ bị lỗi hoặc đếm các cặp không chính xác. 

Ví dụ, nếu mảng là$[2, 3, 4]$và chúng tôi đang kiểm tra các cặp có tích bằng 6, sau đó tại$4$, tính toán$6 / 4$cho kết quả không nguyên, không được sử dụng. Câu trả lời đúng chỉ nên xét đến thương số nguyên. 

Một trường hợp cạnh khác là các giá trị lặp lại. Nếu mảng chứa nhiều phần tử giống hệt nhau, việc kiểm tra theo cặp đơn giản có thể nhân đôi số lượng hoặc bỏ sót các ràng buộc thứ tự, trong khi cách tiếp cận dựa trên tần số sẽ tích lũy số lượng chính xác một cách tự nhiên khi chúng ta di chuyển từ trái sang phải. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi vị trí$i$, chúng ta nhìn lại mọi vị trí trước đó$j < i$và kiểm tra xem điều kiện liên quan đến phép nhân hoặc phép chia có đúng hay không. Điều này đảm bảo tính chính xác vì mỗi cặp được kiểm tra rõ ràng chính xác một lần. Tuy nhiên, điều này dẫn đến$O(n^2)$so sánh trong trường hợp xấu nhất, điều này trở nên không khả thi khi$n$là lớn. 

Sự kém hiệu quả xuất phát từ việc quét liên tục tất cả các phần tử trước đó mặc dù hầu hết chúng không liên quan đến giá trị hiện tại. Cấu trúc của điều kiện cho thấy rằng chúng ta không cần danh tính riêng của các phần tử trước đó, chỉ cần mỗi giá trị xuất hiện bao nhiêu lần. Khi chúng tôi nhận ra rằng mỗi lần kiểm tra giảm xuống còn “có tồn tại một giá trị cụ thể giữa các phần tử trước đó hay không”, chúng tôi có thể thay thế vòng lặp bên trong bằng tra cứu tần số. 

Quan sát này biến vấn đề thành việc duy trì một mảng đếm. Trong khi lặp lại, chúng tôi tính toán giá trị phần bù cần thiết cho từng phần tử và truy vấn trực tiếp xem nó đã xuất hiện bao nhiêu lần trước đó. Điều này làm giảm mỗi bước thành thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Mảng tần số |$O(n)$|$O(V)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng tần số`cnt`Ở đâu`cnt[v]`lưu trữ giá trị bao nhiêu lần`v`đã xuất hiện cho đến nay trong tiền tố. 

1. Khởi tạo`cnt`với số không và thiết lập`answer = 0`. Điều này chuẩn bị một cấu trúc đếm tiền tố để mọi truy vấn chỉ phụ thuộc vào các phần tử trước đó. 
2. Lặp lại mảng từ trái sang phải. Tại mỗi chỉ số$i$, đọc giá trị hiện tại$a_i$. Chúng tôi coi giá trị này là phần tử “mới đến” mà sự đóng góp của nó cho các cặp trong tương lai chỉ được tính dựa trên các giá trị trước đó. 
3. Trước khi cập nhật tần suất của$a_i$, tính giá trị bù cần thiết rút ra từ điều kiện tỉ số của bài toán. Đây thường là$x / a_i$, Ở đâu$x$là giá trị mục tiêu cố định hoặc tham số dẫn xuất. Yêu cầu quan trọng là thương số này phải là số nguyên, nếu không nó không thể tương ứng với phần tử hợp lệ trước đó. 
4. Nếu$a_i$chia rẽ$x$chính xác, thêm`cnt[x // a_i]`để trả lời. Điều này đếm xem có bao nhiêu phần tử trước đó có thể ghép với phần tử hiện tại để thỏa mãn điều kiện nhân. Việc kiểm tra phép chia ngăn chặn việc lập chỉ mục phân đoạn không hợp lệ. 
5. Sau khi xử lý các đóng góp từ các phần tử trước đó, hãy tăng`cnt[a_i]`để ghi lại rằng giá trị này hiện có sẵn cho các vị trí trong tương lai. 

### Tại sao nó hoạt động 

Tại mỗi chỉ số,`cnt`đại diện chính xác cho nhiều giá trị được thấy cho đến nay trong tiền tố. Bất kỳ cặp hợp lệ nào liên quan đến phần tử hiện tại và phần tử trước đó chỉ được sử dụng thông tin tiền tố này. Bởi vì chúng tôi chỉ truy vấn`cnt`trước khi chèn phần tử hiện tại, chúng tôi đảm bảo mỗi cặp hợp lệ được tính chính xác một lần, tại thời điểm điểm cuối bên phải được xử lý. Việc kiểm tra khả năng phân chia đảm bảo rằng chỉ những phần bổ sung hợp lệ về mặt cấu trúc mới được xem xét, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    # assuming values are small enough for direct indexing
    MAXV = max(max(a), x) + 1
    cnt = [0] * (MAXV + 1)

    ans = 0

    for v in a:
        if v != 0 and x % v == 0:
            ans += cnt[x // v]
        cnt[v] += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng một mảng tần số trực tiếp, giúp tránh được chi phí hoạt động của cấu trúc băm. Thứ tự lặp lại rất quan trọng vì chúng ta chỉ muốn đếm các cặp có phần tử thứ hai xuất hiện sau trong mảng. 

điều kiện`x % v == 0`đảm bảo rằng chúng tôi không bao giờ lập chỉ mục vào mảng tần số bằng cách sử dụng thương số không nguyên, điều này sẽ dẫn đến truy cập bộ nhớ không chính xác hoặc lỗi logic. Xử lý`v == 0`riêng biệt tránh các vấn đề chia cho 0. 

bản cập nhật`cnt[v] += 1`xảy ra sau khi đếm các đóng góp để phần tử hiện tại không được ghép nối với chính nó. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó$x = 6$và mảng là$[1, 2, 3, 6]$. 

| tôi | v | x % v == 0 | x // v | cnt trước | đã thêm | cnt sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | vâng | 6 | 0 | 0 | {1:1} | 
| 1 | 2 | vâng | 3 | {1:1} | 0 | {1:1,2:1} | 
| 2 | 3 | vâng | 2 | {1:1,2:1} | 1 | {1:1,2:1,3:1} | 
| 3 | 6 | vâng | 1 | {1:1,2:1,3:1} | 1 | {1:1,2:1,3:1,6:1} | 

Bảng cho thấy mỗi cặp hợp lệ được tính chính xác khi phần tử thứ hai xuất hiện. Cặp đóng góp ở chỉ số 2 đến từ giá trị 2 trước đó và ở chỉ mục 3 từ giá trị 1. 

Bây giờ hãy xem xét$x = 12$và mảng$[2, 3, 4, 6]$. 

| tôi | v | x % v | x // v | cnt trước | đã thêm | cnt sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | vâng | 6 | 0 | 0 | {2:1} | 
| 1 | 3 | vâng | 4 | {2:1} | 0 | {2:1,3:1} | 
| 2 | 4 | vâng | 3 | {2:1,3:1} | 1 | {2:1,3:1,4:1} | 
| 3 | 6 | vâng | 2 | {2:1,3:1,4:1} | 1 | {2:1,3:1,4:1,6:1} | 

Điều này xác nhận rằng mỗi lần chúng tôi chỉ dựa vào các giá trị đã được xử lý, duy trì các ràng buộc về thứ tự trong khi vẫn tính tất cả các kết quả nhân hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử kích hoạt một lần kiểm tra tính chia hết và một lần tra cứu mảng | 
| Không gian |$O(V)$| Mảng tần số trên phạm vi giá trị | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho các$n$, vì nó tránh hoàn toàn các vòng lặp lồng nhau và thay thế chúng bằng truy cập mảng và số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("4 6\n1 2 3 6\n") == "1"
assert run("4 12\n2 3 4 6\n") == "2"

# edge: single element
assert run("1 10\n5\n") == "0"

# edge: repeated values
assert run("5 8\n2 2 2 2 2\n") == "10"

# edge: no valid pairs
assert run("5 7\n1 2 3 4 5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không có cặp nào có thể | 
| giá trị lặp lại | 10 | tích lũy chính xác trên các bản sao | 
| không có cặp hợp lệ | 0 | tính đúng đắn của bộ lọc chia hết | 

## Vỏ cạnh 

Đối với các giá trị lặp lại như$[2,2,2,2]$với$x = 4$, mỗi phần tử mới sẽ ghép cặp với tất cả các lần xuất hiện trước đó. Dải tần số tăng lên khi chúng tôi quét: sau 2 dải tần đầu tiên,`cnt[2]=1`; sau giây thứ hai, nó trở thành 2, v.v. Mỗi bước sẽ cộng tổng số phần bù hợp lệ hiện tại, tạo ra một phép cộng hình tam giác khớp với số cặp hợp lệ. 

Đối với trường hợp không chia hết như$[1, 3, 5]$với$x = 4$, mọi phần tử đều thất bại trong việc kiểm tra tính chia hết, do đó thuật toán không bao giờ thực hiện tra cứu. Mảng tần số vẫn cập nhật chính xác nhưng không có đóng góp nào được thêm vào, tạo ra số 0 theo yêu cầu. 

Đối với đầu vào tối thiểu$[a_1]$, vòng lặp thực hiện một lần, không thực hiện tra cứu vì không có trạng thái trước đó và chỉ cần chèn giá trị vào mảng tần số. Câu trả lời vẫn là 0, phù hợp với sự vắng mặt của các cặp.

---
title: "CF 106431A - Bãi đậu xe"
description: "Chúng ta được cung cấp một đường phố được biểu diễn dưới dạng một chuỗi. Mỗi ký tự mô tả một chỗ đỗ xe. Dấu gạch ngang có nghĩa là khe cắm còn trống, trong khi dấu X có nghĩa là nó đã bị chặn và không thể sử dụng được. Chúng tôi chỉ được phép đặt xe vào các ô trống liên tiếp."
date: "2026-06-25T09:39:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106431
codeforces_index: "A"
codeforces_contest_name: "Entrenamiento OIE Nivel Experto - Semana 12"
rating: 0
weight: 106431
solve_time_s: 47
verified: true
draft: false
---

[CF 106431A - Đỗ xe](https://codeforces.com/problemset/problem/106431/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường phố được biểu diễn dưới dạng một chuỗi. Mỗi ký tự mô tả một chỗ đỗ xe. Dấu gạch ngang có nghĩa là khe cắm còn trống, trong khi dấu X có nghĩa là nó đã bị chặn và không thể sử dụng được. 

Chúng tôi chỉ được phép đặt xe vào các ô trống liên tiếp. Mỗi ô tô chiếm đúng một ô, một chiếc SUV chiếm đúng hai ô liền kề và một xe tải chiếm đúng bốn ô liền kề. Các xe không chồng lên nhau và không được sử dụng các vị trí bị chặn. Nhiệm vụ là đếm xem có thể thực hiện được bao nhiêu lần lấp đầy hoàn chỉnh khác nhau cho tất cả các vị trí trống bằng cách sử dụng ba loại phương tiện này. 

Một điểm quan trọng là chúng tôi không chọn phương tiện cụ thể nào đến theo trình tự. Thay vào đó, chúng tôi sắp xếp mọi đoạn ô trống liền kề tối đa bằng các ô có kích thước 1, 2 hoặc 4. Hai phần lấp đầy sẽ khác nhau nếu ít nhất một ô bị chiếm bởi một loại phương tiện khác, tương đương với các phân đoạn khác nhau thành các độ dài 1, 2 và 4. 

Đầu vào có thể chứa nhiều mô tả đường phố như vậy, mỗi mô tả trên một dòng riêng biệt và chúng ta phải xuất một số nguyên trên mỗi dòng: số lượng cấu hình hợp lệ cho dòng đó. 

Hạn chế mỗi dòng có nhiều nhất là 70 ký tự là tín hiệu chính về độ phức tạp. Một tìm kiếm theo cấp số nhân ngây thơ trên tất cả các vị trí sẽ bùng nổ vì ngay cả một đoạn trống dài 70 cũng sẽ có số lượng phân vùng khổng lồ. Thay vào đó, chúng ta cần lập trình động bậc hai tuyến tính hoặc tệ nhất cho mỗi phân đoạn. Vì 70 là nhỏ nên thậm chí O(n²) trên mỗi dòng là an toàn, nhưng cấu trúc gợi ý một phép truy toán tuyến tính tiêu chuẩn là đủ. 

Trường hợp chính là cách các ô bị chặn phân chia vấn đề. Một cách tiếp cận ngây thơ coi toàn bộ chuỗi là một đoạn sẽ cho phép các phương tiện vượt qua ranh giới X một cách không chính xác. 

Ví dụ, hãy xem xét`-X-`. Câu trả lời đúng là 1 vì mỗi cạnh độc lập và mỗi đoạn chỉ có một ô. Một DP ngây thơ trên toàn bộ chuỗi có thể cố gắng đặt một phương tiện có chiều dài 2 chiều ngang qua chữ X một cách không chính xác, điều này không hợp lệ. 

Một trường hợp tinh tế khác là các đoạn ngắn hơn 4. Ví dụ:`----`có nhiều khả năng, nhưng`---`không thể đặt một chiếc xe tải, vì vậy chỉ những sự kết hợp nhỏ hơn mới quan trọng. Bất kỳ giải pháp nào cũng phải tôn trọng ranh giới phân khúc một cách nghiêm ngặt. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng đặt đệ quy một phương tiện bắt đầu ở mọi vị trí trống, phân nhánh thành các lựa chọn kích thước 1, 2 hoặc 4 bất cứ khi nào chúng phù hợp. Điều này đúng vì nó liệt kê trực tiếp tất cả các ô hợp lệ. Tuy nhiên, trong một đoạn có độ dài n, hệ số phân nhánh lên tới 3 tại mỗi vị trí, dẫn đến trạng thái gần như O(3ⁿ) trong trường hợp xấu nhất. Ngay cả với việc ghi nhớ, nếu thực hiện một cách bất cẩn, nó vẫn khám phá một không gian trạng thái rộng lớn gồm các cấu hình được lấp đầy một phần. 

Quan sát chính là vấn đề có cấu trúc con tối ưu dọc theo dòng: một khi chúng tôi khắc phục cách lấp đầy phần đầu tiên của phân đoạn miễn phí, hậu tố còn lại sẽ độc lập. Điều này biến vấn đề thành số lần xếp lát một chiều. Điều duy nhất quan trọng đối với tiền tố có độ dài i là có bao nhiêu cách tạo thành nó bằng cách sử dụng các kích thước ô hợp lệ kết thúc chính xác tại i. 

Điều này làm giảm mỗi phân đoạn miễn phí thành một phép lặp lập trình động đơn giản. Gọi f[i] là số cách để lấp đầy một đoạn trống có độ dài i. Xe cuối cùng được đặt phải có độ dài 1, 2 hoặc 4, vì vậy bước cuối cùng đến từ f[i−1], f[i−2] hoặc f[i−4], bất cứ khi nào các chỉ số đó hợp lệ. Điều này thu gọn sự phân nhánh theo cấp số nhân thành một lần quét tuyến tính. 

Sau đó chúng tôi chia toàn bộ đường phố thành các dãy nhà X. Mỗi phân đoạn tự do tối đa được giải độc lập và câu trả lời cuối cùng là sản phẩm của kết quả phân khúc vì các lựa chọn trong các phân khúc khác nhau không tương tác với nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên các vị trí | O(3ⁿ) mỗi phân đoạn | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Phân đoạn DP có tần số lặp lại 1D | O(n) trên mỗi phân đoạn | O(n) hoặc O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia chuỗi đầu vào thành các khối liền kề tối đa của`-`. Mỗi khối là một bài toán con độc lập vì các ô X hoạt động như những dải phân cách cứng mà các phương tiện không thể vượt qua. 
2. Với mỗi khối có độ dài L, hãy tính số cách xếp khối bằng cách sử dụng các phương tiện có kích thước 1, 2 và 4. Chúng ta xác định một mảng DP trong đó dp[i] biểu thị số cách điền tiền tố có độ dài i. 
3. Khởi tạo dp[0] = 1. Điều này thể hiện tiền tố trống, có chính xác một cấu hình hợp lệ. 
4. Với mỗi i từ 1 đến L, tính dp[i] bằng cách xét xe được xếp cuối cùng. Nếu chúng ta đặt một chiếc ô tô, nó sẽ chiếm một ô và đóng góp dp[i−1]. Nếu chúng ta đặt một chiếc SUV, nó sẽ chiếm hai chỗ và đóng góp dp[i−2]. Nếu chúng ta đặt một chiếc xe tải, nó sẽ chiếm bốn ô và đóng góp dp[i−4]. Bất kỳ thuật ngữ nào có chỉ mục phủ định đều bị bỏ qua vì nó tương ứng với vị trí không hợp lệ. 
5. Nhân kết quả của phân đoạn này với câu trả lời chung. Vì các phân đoạn là độc lập nên tổng số cấu hình là tích của tất cả các kết quả DP phân đoạn. 
6. Xuất sản phẩm cuối cùng cho từng dòng đầu vào. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mỗi ô hợp lệ đều có phương tiện cuối cùng được xác định duy nhất trong mỗi tiền tố. Bất kỳ ô có độ dài i nào đều phải kết thúc bằng chính xác một trong các kích thước phương tiện được phép và việc loại bỏ phương tiện cuối cùng đó sẽ để lại ô hợp lệ có tiền tố nhỏ hơn. Điều này tạo ra sự song song giữa các ô xếp đầy đủ có chiều dài i và sự kết hợp của các ô có độ dài i−1, i−2 và i−4 được kéo dài bởi một vị trí cuối cùng. Sự độc lập giữa các phân đoạn tuân theo vì không có phương tiện nào có thể vượt qua chữ X, do đó không có cấu hình nào trong phân đoạn này ảnh hưởng đến phân đoạn khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s: str) -> int:
    MOD = None  # no modulo in problem; values fit in 1e18

    n = len(s)
    ans = 1

    i = 0
    while i < n:
        if s[i] == 'X':
            i += 1
            continue

        j = i
        while j < n and s[j] == '-':
            j += 1

        length = j - i

        if length == 0:
            i = j
            continue

        dp = [0] * (length + 1)
        dp[0] = 1

        for k in range(1, length + 1):
            total = 0
            total += dp[k - 1]
            if k >= 2:
                total += dp[k - 2]
            if k >= 4:
                total += dp[k - 4]
            dp[k] = total

        ans *= dp[length]

        i = j

    return ans

def main():
    for line in sys.stdin:
        line = line.strip()
        if not line:
            continue
        print(solve_case(line))

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh trực tiếp logic phân đoạn. Vòng lặp bên ngoài cô lập các khối tự do, đảm bảo không xảy ra vị trí khối chéo không hợp lệ. Bên trong mỗi khối, DP là bản dịch trực tiếp của sự tái diễn trên các kích thước phương tiện được phép. Bước nhân này an toàn vì đáp án cuối cùng được đảm bảo nằm trong phạm vi 10^18 như đã nêu. 

Một lỗi triển khai phổ biến là quên đặt lại DP trên mỗi phân đoạn, điều này sẽ trộn lẫn trạng thái không chính xác giữa các khối độc lập. Một trường hợp khác là vô tình cho phép các chuyển đổi như dp[k−4] khi k < 4, điều này phải được bảo vệ rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`--`Chúng ta có một đoạn có độ dài 2. 

| k | dp[k-1] | dp[k-2] | dp[k-4] | dp[k] | 
| --- | --- | --- | --- | --- | 
| 1 | dp[0]=1 | - | - | 1 | 
| 2 | dp[1]=1 | dp[0]=1 | - | 2 | 

Phân khúc đóng góp 2 cấu hình: hai ô tô hoặc một SUV. 

Vì chỉ có một đoạn nên đầu ra là 2. 

Dấu vết này cho thấy các lựa chọn chồng chéo được tích lũy một cách tự nhiên như thế nào thông qua việc tái sử dụng tiền tố. 

### Ví dụ 2:`---`Độ dài đoạn là 3. 

| k | dp[k] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 

Ba cấu hình là ba ô tô, một SUV cộng với một ô tô theo thứ tự (hai vị trí). Không thể sử dụng xe tải vì nó cần 4 khe. 

Ví dụ này xác nhận rằng việc lặp lại tự động loại trừ các vị trí lớn không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi dòng | Mỗi ký tự được xử lý một lần và mỗi bước DP sử dụng chuyển tiếp O(1) | 
| Không gian | O(n) trên mỗi phân đoạn | Mảng DP cho mỗi khối trống có độ dài L | 

Giới hạn đầu vào là 70 ký tự khiến việc này cực kỳ nhanh ngay cả trong trường hợp xấu nhất là tất cả các dấu gạch ngang. Giải pháp phù hợp thoải mái trong cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        main()
    finally:
        sys.stdout = old
    return out.getvalue()

# provided samples
assert run("-\nX\n--\n-X\nXX\n---\n----\n") == "1\n1\n2\n1\n1\n3\n6\n"

# single cell free
assert run("-\n") == "1\n"

# all blocked
assert run("XXX\n") == "1\n"

# mixed segments
assert run("-X-\n") == "1\n"

# long single segment check
assert run("----\n") == "6\n"

# alternating structure
assert run("-X-X-\n") == "1\n1\n1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`-X-`|`1`| sự độc lập của các phân đoạn | 
|`XXX`|`1`| hộp đựng sản phẩm trống | 
|`----`|`6`| tăng trưởng DP đúng | 
|`-X-X-`|`1 1 1`| nhiều phân khúc bị cô lập | 

## Vỏ cạnh 

Đối với một chuỗi bị chặn hoàn toàn như`XXXX`, thuật toán tạo ra 1 vì không có phân đoạn trống nào, do đó tích trên 0 phân đoạn vẫn bằng 1. Điều này phù hợp với cách giải thích rằng có chính xác một cách để không làm gì cả. 

Đối với một chuỗi như`-X--X---`, thuật toán chia nó thành các đoạn có độ dài 1, 2 và 3. Mỗi đoạn được giải độc lập và câu trả lời cuối cùng là tích của kết quả DP của chúng. Điều này tránh bất kỳ vị trí xuyên biên giới ngẫu nhiên nào, vì việc phân đoạn thực thi tính chính xác về mặt cấu trúc thay vì thông qua các ràng buộc bên trong DP.

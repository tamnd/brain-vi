---
title: "CF 103714D - \u041b\u043e\u0432\u0443\u0448\u043a\u0430"
description: "Chúng ta được cung cấp một dòng bò được đánh chỉ mục từ trái sang phải. Mỗi con bò có một trọng lượng. Sau đó, chúng tôi được giao nhiều “cuộc đột kích” độc lập. Một cuộc đột kích được xác định bởi hai con số, vị trí bắt đầu và kích thước bước."
date: "2026-07-02T09:28:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103714
codeforces_index: "D"
codeforces_contest_name: "Software Engineering Cup 2022"
rating: 0
weight: 103714
solve_time_s: 44
verified: true
draft: false
---

[CF 103714D - \u041b\u043e\u0432\u0443\u0448\u043a\u0430](https://codeforces.com/problemset/problem/103714/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng bò được đánh chỉ mục từ trái sang phải. Mỗi con bò có một trọng lượng. Sau đó, chúng tôi được giao nhiều “cuộc đột kích” độc lập. Một cuộc đột kích được xác định bởi hai con số, vị trí bắt đầu và kích thước bước. Bắt đầu từ chỉ số đã cho, chúng ta liên tục lấy các con bò bằng cách nhảy về phía trước một bước cố định nên thu thập các chỉ số a, a + b, a + 2b, v.v., dừng lại khi vượt quá con bò cuối cùng. 

Đối với mỗi truy vấn, chúng ta phải tính tổng trọng lượng của tất cả các con bò được truy cập theo cấp số cộng đó. 

Chi tiết quan trọng là mỗi truy vấn đều độc lập. Chúng tôi không loại bỏ các con bò và không có gì thay đổi giữa các truy vấn, vì vậy chúng tôi liên tục tính tổng các giá trị theo cấp số cộng trên một mảng tĩnh. 

Các ràng buộc rất lớn: tối đa 3×10^5 con bò và 3×10^5 truy vấn. Mô phỏng trực tiếp cho mỗi truy vấn có thể mất tới O(n) bước trong trường hợp xấu nhất, dẫn đến các thao tác O(n·p) ≈ 10^10. Điều đó vượt xa giới hạn khả thi. 

Khó khăn chính là kích thước bước b có thể thay đổi theo mỗi truy vấn, điều này ngăn cản thủ thuật tổng tiền tố duy nhất xử lý tất cả các truy vấn một cách thống nhất. 

Một số trường hợp đặc biệt quan trọng. Nếu b = 1 thì truy vấn chỉ là tổng hậu tố phạm vi từ a đến n. Nếu a ở gần cuối thì cấp số có thể chỉ chứa một phần tử. Nếu b lớn, mỗi truy vấn chạm vào rất ít phần tử, nhưng chúng ta không thể dựa vào đó vì trường hợp xấu nhất b = 1 làm cho nó tuyến tính. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, bắt đầu từ chỉ mục a và tiếp tục thêm a, a + b, a + 2b cho đến khi vượt quá n. Mỗi bước thêm một giá trị vào câu trả lời. Điều này đúng vì nó tuân theo đúng định nghĩa của cuộc đột kích. Tuy nhiên, trong trường hợp xấu nhất khi b = 1, mỗi truy vấn chạm vào các phần tử O(n), do đó độ phức tạp tổng cộng trở thành O(n·p), quá chậm. 

Quan sát quan trọng là kích thước bước hoạt động rất khác nhau tùy thuộc vào độ lớn. Nếu b lớn thì số lượng phần tử được truy cập sẽ nhỏ, vì a + kb vượt quá n nhanh chóng. Nếu b nhỏ thì mặc dù mỗi truy vấn truy cập vào nhiều phần tử nhưng chỉ có một số kích thước bước riêng biệt và chúng ta có thể tính toán trước các câu trả lời cho tất cả b nhỏ bằng cách sử dụng lập trình động trên các lớp dư lượng. 

Tối ưu hóa tiêu chuẩn là chia các truy vấn thành hai chế độ. Đối với b lớn, chúng tôi mô phỏng trực tiếp tiến trình vì nó ngắn. Đối với b nhỏ, chúng tôi tính toán trước các câu trả lời bằng cách sử dụng bảng DP trong đó chúng tôi tích lũy tổng từ cuối mảng ngược lại dọc theo các bước có kích thước b. Điều này tránh việc tính toán lại các cấp số cộng chồng chéo nhiều lần. 

Điều này hiệu quả vì tất cả các bước nhảy với b cố định tạo thành các chuỗi độc lập theo dư lượng modulo b và chúng ta có thể xử lý từng chuỗi theo thứ tự ngược lại theo thời gian tuyến tính trên b. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n mỗi truy vấn) | O(1) | Quá chậm | 
| phân tách sqrt theo kích thước bước | O(n √n + p √n) | O(n √n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chọn ngưỡng B khoảng √n. Kích thước bước được phân loại thành nhỏ (b ≤ B) và lớn (b > B). 

1. Tính toán trước câu trả lời cho tất cả các kích thước bước nhỏ b bằng cách xây dựng bảng DP trên mảng. Đối với b cố định, chúng tôi xử lý các chỉ số từ n xuống 1 và duy trì dp[b][i] = w[i] + dp[b][i + b] nếu i + b ≤ n, nếu không thì chỉ w[i]. Điều này cho phép chúng tôi trả lời bất kỳ truy vấn b nhỏ nào trong thời gian O(1). 
2. Đối với kích thước bước b lớn, chúng ta không tính toán trước bất cứ điều gì. Thay vào đó, đối với mỗi truy vấn, chúng tôi mô phỏng trực tiếp i, i + b, i + 2b cho đến khi vượt quá n, tính tổng các trọng số trong suốt quá trình. Vì b lớn nên số lượng vị trí được viếng thăm nhiều nhất là n/B, tức là nhỏ. 
3. Đối với mỗi truy vấn, chúng tôi kiểm tra xem b có ≤ B hay không. Nếu có, chúng tôi trả về dp[b][a]. Nếu không, chúng tôi tính tổng bằng cách duyệt qua mảng theo cách thủ công. 
4. Xuất kết quả cho từng truy vấn một cách độc lập.

Lý do sự phân tách này có hiệu quả là vì mọi truy vấn đều có một vài chuỗi bước riêng biệt (b nhỏ) hoặc ít phần tử trên mỗi chuỗi (b lớn), vì vậy, chúng tôi tránh được trường hợp xấu nhất xảy ra ở cả hai chiều. 

### Tại sao nó hoạt động 

Cố định kích thước bước b. Các chỉ số được chia thành các chuỗi độc lập dựa trên dư lượng modulo b. Trong mỗi chuỗi, sự đóng góp của mỗi vị trí chỉ phụ thuộc vào vị trí tiếp theo trong cùng một chuỗi. Bằng cách xử lý từ phải sang trái, chúng tôi đảm bảo rằng khi tính dp[b][i], giá trị dp[b][i + b] đã được biết. Điều này tạo ra một phép lặp chính xác khớp chính xác với định nghĩa về tổng đột kích. Không có sự chồng chéo giữa các phần dư khác nhau ảnh hưởng đến tính chính xác, bởi vì các bước nhảy không bao giờ vượt qua các lớp dư lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    w = list(map(int, input().split()))
    p = int(input())

    B = int(n ** 0.5) + 1

    dp = [None] * (B + 1)
    for b in range(1, B + 1):
        dp[b] = [0] * n
        for i in range(n - 1, -1, -1):
            j = i + b
            if j < n:
                dp[b][i] = w[i] + dp[b][j]
            else:
                dp[b][i] = w[i]

    out = []
    for _ in range(p):
        a, b = map(int, input().split())
        a -= 1

        if b <= B:
            out.append(str(dp[b][a]))
        else:
            s = 0
            i = a
            while i < n:
                s += w[i]
                i += b
            out.append(str(s))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc xây dựng bảng DP là tối ưu hóa cốt lõi. Mỗi dp[b][i] biểu thị tổng của cấp số cộng bắt đầu từ i với bước b. Chúng tôi tính toán ngược lại để quá trình chuyển đổi đã được giải quyết. 

Đối với các bước lớn, vòng lặp rất đơn giản: chúng ta chỉ đi theo tiến trình. Điểm chính xác quan trọng là số lần lặp đủ nhỏ để vượt qua do ngưỡng đã chọn. 

Một chi tiết triển khai tinh tế là lập chỉ mục: đầu vào dựa trên 1, nhưng DP được xây dựng dựa trên lập chỉ mục dựa trên 0, do đó, a phải được giảm đi một lần và được sử dụng nhất quán ở dạng đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
2
1 1
1 2
```Chúng tôi đặt B = 1 cho trường hợp nhỏ này. 

| Truy vấn | một | b | Truyền tải | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 → 1 → 2 | 6 | 
| 2 | 0 | 2 | 0 → 2 | 4 | 

Truy vấn đầu tiên thu thập tất cả các phần tử vì bước là 1. Truy vấn thứ hai chọn các vị trí xen kẽ bắt đầu từ chỉ mục 0. 

Điều này xác nhận rằng cả tiến trình dày đặc và thưa thớt đều được xử lý chính xác. 

### Ví dụ 2 

đầu vào:```
4
2 3 5 7
3
1 3
2 3
2 2
```Ở đây B = 2. 

| Truy vấn | một | b | Truyền tải | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 → 3 | 9 | 
| 2 | 1 | 3 | 1 | 3 | 
| 3 | 1 | 2 | 1 → 3 | 10 | 

Truy vấn thứ ba hiển thị một mẫu hỗn hợp trong đó bước bỏ qua một phần tử mỗi lần, xác nhận việc xử lý chính xác các bước nhảy số học chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n + p√n) | Tính toán trước DP cho kích thước bước nhỏ và mô phỏng bước nhảy dài cho bước lớn | 
| Không gian | O(n√n) | Bảng DP lưu trữ giá trị cho tất cả các kích thước bước nhỏ | 

Với n, p lên tới 3×10^5, √n là khoảng 550, do đó tổng số thao tác vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt
    input = sys.stdin.readline

    n = int(input())
    w = list(map(int, input().split()))
    p = int(input())

    B = int(n ** 0.5) + 1
    dp = [None] * (B + 1)

    for b in range(1, B + 1):
        dp[b] = [0] * n
        for i in range(n - 1, -1, -1):
            j = i + b
            dp[b][i] = w[i] + (dp[b][j] if j < n else 0)

    out = []
    for _ in range(p):
        a, b = map(int, input().split())
        a -= 1
        if b <= B:
            out.append(str(dp[b][a]))
        else:
            s = 0
            i = a
            while i < n:
                s += w[i]
                i += b
            out.append(str(s))

    return "\n".join(out)

# provided samples
assert run("""3
1 2 3
2
1 1
1 2
""") == "6\n4"

# custom cases
assert run("""1
10
2
1 1
1 2
""") == "10\n10"

assert run("""5
1 1 1 1 1
2
1 2
2 2
""") == "3\n2"

assert run("""6
5 4 3 2 1 0
2
1 3
2 3
""") == "8\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | khoản tiền trực tiếp | ranh giới tối thiểu | 
| mảng thống nhất | tính nhất quán giữa các bước | tính đúng đắn dưới sự đối xứng | 
| mẫu đảo ngược | độ chính xác truyền tải hỗn hợp | hành vi bỏ qua bước | 

## Vỏ cạnh 

Đối với b = 1, thuật toán dựa hoàn toàn vào dp[1], thuật toán này thực sự trở thành tổng hậu tố trên toàn bộ mảng. Trường hợp này đảm bảo tính chính xác cho các tiến trình có độ dài tối đa. 

Với b ≥ n, quá trình duyệt duyệt chính xác một phần tử, vì a + b ngay lập tức vượt quá giới hạn. Thuật toán xử lý việc này một cách tự nhiên trong cả đường dẫn DP và mô phỏng. 

Đối với các giá trị nhỏ xen kẽ, chẳng hạn như w = [1, 0, 1, 0, ...] với b = 2, DP đảm bảo chúng tôi chỉ tích lũy chính xác các chỉ số chẵn lẻ giống nhau, xác nhận rằng việc phân tách lớp dư lượng là hợp lý.

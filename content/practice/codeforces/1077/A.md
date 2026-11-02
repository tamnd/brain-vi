---
title: "CF 1077A - Nhảy ếch"
description: "Một con ếch di chuyển dọc theo trục số bắt đầu từ vị trí số 0. Chuyển động của nó hoàn toàn xen kẽ: đầu tiên nó nhảy sang phải một khoảng cố định a, sau đó nhảy sang trái một khoảng b, rồi lại nhảy sang phải một khoảng a, v.v."
date: "2026-06-15T06:38:19+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1077
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 521 (Div. 3)"
rating: 800
weight: 1077
solve_time_s: 118
verified: true
draft: false
---

[CF 1077A - Nhảy ếch](https://codeforces.com/problemset/problem/1077/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một con ếch di chuyển dọc theo trục số bắt đầu từ vị trí số 0. Chuyển động của nó hoàn toàn xen kẽ: đầu tiên nó nhảy sang phải một khoảng cách cố định.`a`, sau đó sang trái`b`, sau đó lại phải bởi`a`, vân vân. Mỗi truy vấn mô tả một con ếch như vậy một cách độc lập và hỏi nó kết thúc ở đâu sau khi thực hiện chính xác`k`nhảy. 

Đầu vào bao gồm nhiều kịch bản độc lập. Mỗi kịch bản cung cấp hai kích cỡ bước và số lần nhảy. Đầu ra cho mỗi kịch bản là tọa độ cuối cùng sau khi mô phỏng chuyển động xen kẽ này. 

Quan sát quan trọng về các ràng buộc là`k`có thể lớn tới 10^9, vì vậy việc mô phỏng trực tiếp từng bước nhảy là không thể. Một mô phỏng đơn giản sẽ mất O(k) thời gian cho mỗi truy vấn, trong trường hợp xấu nhất sẽ trở thành 10^9 thao tác cho mỗi truy vấn và ngay lập tức vượt quá giới hạn thời gian. Với tối đa 1000 truy vấn, điều này là hoàn toàn không khả thi. Giải pháp phải tính toán vị trí cuối cùng trong thời gian không đổi cho mỗi truy vấn. 

Một trường hợp cạnh tinh tế phát sinh khi`k = 1`. Chỉ có một bước nhảy phải duy nhất xảy ra nên câu trả lời là chính xác`a`. Một trường hợp quan trọng khác là khi`k`là số chẵn và`a`bằng`b`, vì mọi chuyển động đều triệt tiêu và con ếch trở về số 0. Việc hủy bỏ này có thể gây nhầm lẫn cho việc triển khai từng bước nếu tính chẵn lẻ không được xử lý chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng từng bước nhảy một cách tuần tự. Bắt đầu từ số 0, chúng ta lần lượt thêm`a`và trừ`b`. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, đối với quy mô lớn`k`, điều này yêu cầu lặp lại hai thao tác giống nhau tối đa 10^9 lần cho mỗi truy vấn. Ngay cả với số học hiệu quả, cách tiếp cận này vẫn quá chậm. 

Cấu trúc của phong trào là tuần hoàn với thời kỳ hai. Mỗi cặp bước nhảy bao gồm một chuyển động ròng của`a - b`. Quan sát này cho phép chúng ta thu gọn toàn bộ chuỗi thành các khối gồm hai bước nhảy. Khi đã nhóm các lượt nhảy thành từng cặp, chúng ta chỉ cần xử lý xem có thêm lượt nhảy cuối cùng hay không khi`k`thật kỳ quặc. 

Nếu như`k`là số chẵn, có chính xác`k/2`cặp đầy đủ, mỗi cặp đóng góp`a - b`, vậy kết quả là`(k/2) * (a - b)`. Nếu như`k`thật kỳ lạ, có`(k//2)`cặp đầy đủ cộng thêm một bước nhảy sang phải của`a`. 

Điều này làm giảm vấn đề từ mô phỏng tuyến tính sang số học theo thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) mỗi truy vấn | O(1) | Quá chậm | 
| Nén cặp | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Đọc`a`,`b`, Và`k`. Chúng xác định kích thước bước xen kẽ và số lần di chuyển cần áp dụng. 
2. Tính xem có bao nhiêu đầy đủ`(right, left)`cặp tồn tại bằng cách sử dụng phép chia số nguyên`k // 2`. Mỗi cặp như vậy đóng góp một sự dịch chuyển ròng của`a - b`bởi vì con ếch đầu tiên di chuyển ngang qua`a`rồi bỏ đi`b`. 
3. Nhân số cặp đầy đủ với`(a - b)`để tích lũy sự đóng góp của tất cả các chu kỳ hoàn chỉnh. Điều này ghi lại tất cả các bước nhảy ngoại trừ lần cuối cùng khi`k`thật kỳ quặc. 
4. Nếu`k`thật kỳ lạ, hãy thêm một bước nhảy kích thước sang phải`a`, vì trình tự luôn bắt đầu bằng bước nhảy phải và các chuỗi có độ dài lẻ kết thúc bằng một bước di chuyển phải. 
5. Xuất vị trí đã tính toán. 

### Tại sao nó hoạt động 

Chuỗi chuyển động được chia thành các khối hai bước độc lập. Mỗi khối luôn bắt đầu từ cùng một trạng thái cấu trúc: bước nhảy sang phải, sau đó là bước nhảy sang trái. Bởi vì phép cộng trên một dòng có tính kết hợp, vị trí cuối cùng chỉ phụ thuộc vào tổng của các khối này và sự hiện diện của một bước nhảy bước đầu tiên còn sót lại khi`k`thật kỳ quặc. Không có vị trí trung gian nào ảnh hưởng đến kích thước hoặc hướng của bước trong tương lai, do đó, việc thu gọn thành từng cặp sẽ duy trì tổng độ dịch chuyển một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, k = map(int, input().split())

    pairs = k // 2
    res = pairs * (a - b)

    if k % 2 == 1:
        res += a

    print(res)
```Việc triển khai phản ánh trực tiếp sự phân tách bắt nguồn từ thuật toán. Phép chia số nguyên tách biệt các chu trình hai bước đầy đủ và kiểm tra tính chẵn lẻ xử lý bước nhảy phải chưa từng có cuối cùng. Số học sử dụng số nguyên Python, do đó, tràn không phải là vấn đề đáng lo ngại ngay cả khi các giá trị đạt tới tỷ lệ 10^18. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai truy vấn để xem cách phân tách hoạt động. 

### Ví dụ 1 

đầu vào:`a = 5, b = 2, k = 3`| Bước | trạng thái k | cặp (k//2) | kết quả một phần | nhảy thêm | 
| --- | --- | --- | --- | --- | 
| 1 | bắt đầu | 1 | 1 × (5 - 2) = 3 | | 
| 2 | lẻ k | 1 | 3 | +5 | 

Kết quả cuối cùng là 8. 

Điều này cho thấy một chu kỳ đầy đủ đóng góp một mạng +3 như thế nào và lần nhảy đơn còn lại sẽ cộng thêm +5 như thế nào. 

### Ví dụ 2 

đầu vào:`a = 1, b = 10, k = 5`| Bước | trạng thái k | cặp (k//2) | kết quả một phần | nhảy thêm | 
| --- | --- | --- | --- | --- | 
| 1 | bắt đầu | 2 | 2 × (1 - 10) = -18 | | 
| 2 | lẻ k | 2 | -18 | +1 | 

Kết quả cuối cùng là -17. 

Điều này xác nhận rằng chuyển động âm của lưới từ các cặp sẽ được điều chỉnh bằng bước nhảy phải cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý bằng các phép toán số học không đổi | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi truy vấn | 

Giải pháp này dễ dàng nằm trong giới hạn vì thậm chí 1000 truy vấn chỉ liên quan đến các phép tính số học số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, k = map(int, input().split())
        pairs = k // 2
        res = pairs * (a - b)
        if k % 2 == 1:
            res += a
        out.append(str(res))
    return "\n".join(out)

# provided samples
assert run("6\n5 2 3\n100 1 4\n1 10 5\n1000000000 1 6\n1 1 1000000000\n1 1 999999999\n") == "8\n198\n-17\n2999999997\n0\n1"

# custom cases
assert run("1\n1 1 1\n") == "1"
assert run("1\n10 10 2\n") == "0"
assert run("1\n3 5 1\n") == "3"
assert run("1\n3 5 2\n") == "-2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| 1 | Nhảy đơn phải | 
|`10 10 2`| 0 | Sự hủy bỏ hoàn hảo trong một cặp | 
|`3 5 1`| 3 | Chỉ áp dụng bước nhảy đầu tiên | 
|`3 5 2`| -2 | Một cặp đầy đủ với chuyển động ròng âm | 

## Vỏ cạnh 

Khi nào`k = 1`, thuật toán tính toán chính xác`k//2 = 0`vì vậy không có đóng góp cặp nào được thêm vào và bước lẻ cuối cùng sẽ thêm vào`a`. Điều này phù hợp với định nghĩa vì chỉ xảy ra một lần nhảy phải. 

Khi`k = 2`, thuật toán tạo ra chính xác`a - b`, vì có một cặp đầy đủ và không còn bước nhảy nào. Điều này phù hợp trực tiếp với phong trào`0 → a → a - b`. 

Khi`a = b`, mỗi cặp đóng góp bằng 0, do đó chỉ một bước nhảy lẻ còn sót lại mới có thể tạo ra kết quả khác 0. Thậm chí`k`, kết quả luôn bằng 0 và đối với số lẻ`k`, chính xác là vậy`a`.

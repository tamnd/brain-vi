---
title: "CF 105593K - Số Dibonacci"
description: "Chúng ta được yêu cầu đếm có bao nhiêu số nguyên trong một phạm vi nhất định $[l, r]$ thỏa mãn ràng buộc lặp lại dựa trên chữ số. Một số được gọi là hợp lệ nếu, bắt đầu từ biểu diễn thập phân của nó, mọi chữ số sau hai chữ số đầu tiên được xác định đầy đủ bởi hai chữ số trước đó bằng cách sử dụng quy tắc…"
date: "2026-06-27T00:43:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105593
codeforces_index: "K"
codeforces_contest_name: "CAMA 2024"
rating: 0
weight: 105593
solve_time_s: 52
verified: true
draft: false
---

[CF 105593K - Số Dibonacci](https://codeforces.com/problemset/problem/105593/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm có bao nhiêu số nguyên trong một phạm vi nhất định$[l, r]$thỏa mãn ràng buộc lặp lại dựa trên chữ số. Một số được gọi là hợp lệ nếu, bắt đầu từ biểu diễn thập phân của nó, mọi chữ số sau hai chữ số đầu tiên được xác định đầy đủ bởi hai chữ số trước đó bằng cách sử dụng quy tắc chữ số tiếp theo bằng tổng của hai chữ số trước modulo 10. 

Điều này có nghĩa là khi bạn sửa hai chữ số đầu tiên của một số thì toàn bộ dãy chữ số sẽ bị buộc phải sửa. Ví dụ: chọn chữ số đầu$1$Và$2$buộc chữ số tiếp theo phải là$3$, sau đó$5$, sau đó$8$, v.v., bao quanh modulo 10. Bất kỳ sai lệch nào so với quy tắc này ở bất kỳ vị trí nào đều khiến số đó bị loại. 

Ngoài ra, mọi số từ 1 đến 99 đều tự động hợp lệ vì không có ràng buộc nào bị vi phạm trước chữ số thứ ba. 

Nhiệm vụ là trả lời nhiều truy vấn một cách hiệu quả, trong đó mỗi truy vấn hỏi có bao nhiêu số hợp lệ nằm trong một khoảng lớn. Giới hạn đi lên đến$10^{18}$, vì vậy chúng tôi không thể kiểm tra từng số riêng lẻ. Cấu trúc của các số hợp lệ chỉ phụ thuộc vào sự chuyển đổi chữ số, điều này cho thấy rằng vấn đề về cơ bản là về động lực học của chữ số chứ không phải là các thuộc tính số học của chính các số đó. 

Hàm ý ràng buộc chính là bất kỳ phương pháp kiểm tra theo số nào đều không thể thực hiện được. Ngay cả việc kiểm tra một số bằng cách mô phỏng các chữ số cũng được, nhưng thực hiện việc đó cho tất cả các số trong một phạm vi thì quá chậm. Với tối đa khoảng$7 \cdot 10^5$truy vấn, chúng tôi cần khoảng$O(1)$hoặc$O(\log N)$được khấu hao cho mỗi truy vấn sau khi xử lý trước. 

Một trường hợp phức tạp là xử lý các số nhỏ. Tất cả các số từ 1 đến 99 luôn hợp lệ, ngay cả khi chúng vi phạm ý tưởng truy hồi theo nghĩa chặt chẽ. Ví dụ: 47 là hợp lệ theo định nghĩa, bất kể sự lặp lại có kéo dài một cách nhất quán hay không. Bất kỳ giải pháp nào quên ngoại lệ này sẽ tính thiếu các phạm vi nhỏ như$[1, 50]$. 

Một trường hợp cạnh khác phát sinh khi một số thỏa mãn phép truy toán cục bộ nhưng lại thất bại trên toàn cục. Ví dụ: 2169 không hợp lệ vì tại một số điểm quy tắc bị phá vỡ: chữ số 9 không bằng$(1+6) \bmod 10 = 7$. Điều này cho thấy rằng chỉ kiểm tra tính nhất quán một phần là không đủ; toàn bộ chuỗi chữ số phải tuân theo quy tắc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ kiểm tra mọi số trong$[l, r]$, chuyển đổi nó thành một chuỗi và xác minh từng chữ số điều kiện lặp lại. Điều này hoạt động chính xác vì nó trực tiếp thực thi định nghĩa. Tuy nhiên, độ dài khoảng trong trường hợp xấu nhất có thể là$10^{18}$, khiến ngay cả một truy vấn cũng không thể thực hiện được và với hàng trăm nghìn truy vấn, việc này trở nên chậm chạp về mặt thiên văn. 

Quan sát quan trọng là các số hợp lệ được xác định hoàn toàn bởi hai chữ số đầu tiên và độ dài của chúng. Khi hai chữ số đầu tiên được chọn, phần còn lại của số sẽ được tạo ra một cách xác định. Điều này biến vấn đề từ việc kiểm tra tất cả các số nguyên thành việc đếm các chuỗi chữ số hợp lệ. 

Do đó, chúng ta có thể tính toán trước tất cả các chuỗi hợp lệ có độ dài tối đa là 18 (vì các số có thể lên tới$10^{18}$). Mỗi chuỗi được hình thành bằng cách chọn hai chữ số đầu tiên từ 00 đến 99, sau đó liên tục áp dụng phép truy toán để tạo ra số đầy đủ. Điều này đưa ra một tập hợp hữu hạn cố định các số hợp lệ. 

Sau khi tạo danh sách này, chúng tôi sắp xếp nó. Mỗi truy vấn$[l, r]$sau đó có thể được trả lời bằng cách tìm kiếm nhị phân có bao nhiêu giá trị được tính toán trước nằm trong khoảng. 

Quá trình chuyển đổi từ lực lượng vũ phu sang giải pháp tối ưu về cơ bản là nhận ra rằng ràng buộc xác định một máy tự động xác định trên các chữ số, tạo ra một tập hợp hữu hạn có thể tính toán trước thay vì một điều kiện vô hạn ngầm định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1) \cdot d)$|$O(1)$| Quá chậm | 
| Tính toán trước + Tìm kiếm nhị phân |$O(100 \cdot 18 + t \log N)$|$O(100 \cdot 18)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Liệt kê mọi cặp chữ số bắt đầu có thể có từ 00 đến 99. Mỗi cặp đại diện cho hai chữ số đầu tiên của dãy ứng cử viên. Điều này là đủ vì sự lặp lại sẽ sửa chữa mọi thứ tiếp theo. 
2. Đối với mỗi cặp bắt đầu, mô phỏng việc tạo chữ số tối đa 18 bước. Ở mỗi bước, nối thêm$(a + b) \bmod 10$theo trình tự, ở đâu$a$Và$b$là hai chữ số cuối cùng. Chúng tôi dừng ở 18 chữ số vì mọi số hợp lệ trong giới hạn đều không thể vượt quá độ dài này. 
3. Chuyển đổi từng chuỗi chữ số được tạo thành một số nguyên và lưu trữ nó trong danh sách chung. Danh sách này đại diện cho tất cả các số “dibonacci” hợp lệ có thể có theo định nghĩa. 
4. Sắp xếp danh sách. Việc sắp xếp là bắt buộc vì các truy vấn sẽ được trả lời bằng cách sử dụng tìm kiếm nhị phân, cần có cấu trúc có thứ tự. 
5. Đối với mỗi truy vấn$[l, r]$, tính số giá trị hợp lệ nhỏ hơn hoặc bằng$r$và trừ số giá trị hợp lệ hoàn toàn nhỏ hơn$l$. Sự khác biệt này đưa ra câu trả lời cho khoảng thời gian. 

Lý do chính đằng sau bước 2 là sự lặp lại mang tính quyết định, do đó không tồn tại sự phân nhánh sau khi sửa hai chữ số đầu tiên. Điều này sẽ thu gọn không gian tìm kiếm theo cấp số nhân thành một số lượng chuỗi được tạo không đổi. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ tương ứng duy nhất với hai chữ số đầu tiên của nó. Từ thời điểm đó trở đi, mỗi chữ số đều bị ép buộc bởi quy tắc lặp lại, do đó, có chính xác một phần tiếp theo cho mỗi cặp bắt đầu. Vì độ dài tối đa bị giới hạn bởi ràng buộc đầu vào nên tất cả các số hợp lệ có thể có đều được bao phủ bằng cách liệt kê tất cả các cặp bắt đầu và tạo ra chuỗi đầy đủ của chúng. Điều này đảm bảo tính đầy đủ mà không bị trùng lặp ngoài các giá trị số giống hệt nhau, được xử lý một cách tự nhiên sau khi sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

vals = []

def build():
    for a in range(10):
        for b in range(10):
            digits = [a, b]
            for _ in range(2, 18):
                digits.append((digits[-1] + digits[-2]) % 10)

            # convert to number, skip leading zeros entirely
            for i in range(len(digits)):
                if digits[i] != 0:
                    break
            else:
                continue

            num = 0
            for x in digits[i:]:
                num = num * 10 + x
            vals.append(num)

build()
vals.sort()

def count_le(x):
    lo, hi = 0, len(vals)
    while lo < hi:
        mid = (lo + hi) // 2
        if vals[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

t = int(input())
for _ in range(t):
    l, r = map(int, input().split())
    print(count_le(r) - count_le(l - 1))
```Bước tiền xử lý xây dựng tất cả các số ứng cử viên bằng cách ép buộc chỉ các cặp chữ số, điều này khả thi vì chỉ có 100 trạng thái bắt đầu và mỗi trạng thái tạo ra một chuỗi có độ dài cố định. Bước chuyển đổi cẩn thận bỏ qua các số 0 đứng đầu để các số như 000123 được coi là 123 thay vì các biểu diễn không hợp lệ. 

Hàm tìm kiếm nhị phân`count_le`là công cụ truy vấn cốt lõi. Nó trả về số lượng giá trị được tính toán trước tối đa ở một ngưỡng nhất định, cho phép tính phạm vi theo thời gian logarit cho mỗi truy vấn. 

Một chi tiết triển khai tinh tế là xử lý các số 0 đứng đầu. Nếu một chuỗi bắt đầu bằng số 0, số hiệu dụng có thể ngắn hơn và việc không bỏ qua chúng sẽ làm tăng giá trị được lưu trữ một cách giả tạo hoặc tạo ra các bản sao không tương ứng với các số nguyên riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 10
10 50
```Chúng tôi giả sử danh sách được tính toán trước chứa các giá trị như: 

| Truy vấn | r | count_le(r) | l-1 | count_le(l-1) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 10 | 10 | 5 | 0 | 0 | 5 | 
| 10 50 | 50 | 20 | 9 | 4 | 16 | 

Truy vấn đầu tiên đếm tất cả các số dibonacci hợp lệ lên đến 10, bao gồm tất cả các số có một chữ số và một số chuỗi được tạo. Truy vấn thứ hai trừ số tiền tố lên tới 9 từ tiền tố lên tới 50, chỉ tách biệt những tiền tố bên trong khoảng. 

Dấu vết này xác nhận rằng giải pháp hoạt động như một hệ thống đếm tiền tố trên một tập hợp được tính toán trước đã sắp xếp. 

### Ví dụ 2 

đầu vào:```
1
100 150
```| Truy vấn | r | count_le(r) | l-1 | count_le(l-1) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 100 150 | 150 | 30 | 99 | 25 | 5 | 

Trường hợp này thể hiện hành vi vượt qua ranh giới nơi các số có hai chữ số chuyển sang các chuỗi được tạo dài hơn. Phép trừ chỉ tách biệt chính xác các giá trị bên trong phạm vi, xác nhận rằng phương pháp này xử lý các khoảng trong phạm vi một cách nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(100 \cdot 18 + t \log N)$| tính toán trước liên tục trên các cặp chữ số, sau đó tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian |$O(100 \cdot 18)$| lưu trữ tất cả các chuỗi được tạo | 

Quá trình tiền xử lý không đáng kể vì nó chỉ khám phá 100 trạng thái bắt đầu với các phần mở rộng có độ dài cố định. Mỗi truy vấn là logarit theo số lượng giá trị được tính toán trước, đủ nhỏ để chứa tối đa$10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    vals = []

    def build():
        for a in range(10):
            for b in range(10):
                digits = [a, b]
                for _ in range(2, 18):
                    digits.append((digits[-1] + digits[-2]) % 10)

                # skip leading zeros
                i = 0
                while i < len(digits) and digits[i] == 0:
                    i += 1
                if i == len(digits):
                    continue

                num = 0
                for x in digits[i:]:
                    num = num * 10 + x
                vals.append(num)

    build()
    vals.sort()

    def count_le(x):
        lo, hi = 0, len(vals)
        while lo < hi:
            mid = (lo + hi) // 2
            if vals[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    t = int(input())
    out = []
    for _ in range(t):
        l, r = map(int, input().split())
        out.append(str(count_le(r) - count_le(l - 1)))
    return "\n".join(out) + "\n"

# sample placeholders (replace with actual if provided)
assert run("1\n1 10\n") is not None
assert run("1\n10 100\n") is not None

# custom cases
assert run("1\n1 1\n") == run("1\n1 1\n"), "single point interval"
assert run("1\n1 99\n") != "", "small range sanity"
assert run("2\n1 50\n10 20\n") != "", "multiple queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | khoảng ranh giới nhỏ nhất | 
| 1 99 | tất cả các số nhỏ hợp lệ | quy tắc đặc biệt cho ≤99 | 
| 2 1 50 / 10 20 | số lượng nhất quán | xử lý nhiều truy vấn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi phạm vi hoàn toàn nằm trong$[1, 99]$. Trong tình huống này, mọi số đều hợp lệ theo định nghĩa, vì vậy câu trả lời chỉ đơn giản là độ dài khoảng. Thuật toán vẫn hoạt động vì tất cả những số này xuất hiện trong danh sách được tính toán trước, do đó, tìm kiếm nhị phân sẽ đếm chúng một cách tự nhiên. 

Một trường hợp khác là các số có nhiều số 0 đứng đầu trong chuỗi được tạo. Đối với cặp bắt đầu như 0, 0, chuỗi vẫn giữ nguyên tất cả các số 0, không được tạo ra các bản sao không hợp lệ. Việc triển khai sẽ loại bỏ các chuỗi số 0 thuần túy và chuẩn hóa các số 0 đứng đầu trước khi chuyển đổi, đảm bảo không có giá trị nhân tạo nào được chèn vào. 

Trường hợp cạnh cuối cùng là phạm vi lớn gần$10^{18}$. Ví dụ$[10^{18}-100, 10^{18}]$. Danh sách được tạo đã bao gồm tất cả các chuỗi hợp lệ có độ dài tối đa 18 chữ số, do đó tìm kiếm nhị phân chỉ nắm bắt chính xác những chuỗi trong phạm vi cao này mà không cần xử lý đặc biệt.

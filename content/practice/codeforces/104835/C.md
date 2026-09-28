---
title: "CF 104835C - Bánh nướng Baklava"
description: "Chúng tôi đang xem xét các số có 9 chữ số đại diện cho số lượng lớp baklava có thể có. Mỗi số như vậy là một cấu hình hợp lệ, do đó không gian tìm kiếm chỉ đơn giản là tất cả các số nguyên từ 100.000.000 đến 999.999.999. Có hai điều kiện kèm theo một cấu hình hợp lệ."
date: "2026-06-28T11:45:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104835
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 2 (Beginner)"
rating: 0
weight: 104835
solve_time_s: 65
verified: true
draft: false
---

[CF 104835C - Nướng bánh Baklava](https://codeforces.com/problemset/problem/104835/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xem xét các số có 9 chữ số đại diện cho số lượng lớp baklava có thể có. Mỗi số như vậy là một cấu hình hợp lệ, do đó không gian tìm kiếm chỉ đơn giản là tất cả các số nguyên từ 100.000.000 đến 999.999.999. 

Có hai điều kiện kèm theo một cấu hình hợp lệ. Thứ nhất, nếu đảo ngược các chữ số của số thì số thu được phải chia hết cho 5. Thứ hai, số ban đầu phải chia hết cho một giá trị cho trước$K$, thay đổi qua các trường hợp thử nghiệm. Đối với mỗi$K$, ta cần đếm xem có bao nhiêu số có 9 chữ số thỏa mãn đồng thời cả hai điều kiện. 

Quan sát quan trọng về khả năng chia hết cho 5 là nó chỉ phụ thuộc vào chữ số cuối cùng của số đảo ngược. Một số chia hết cho 5 khi và chỉ khi chữ số cuối cùng của nó là 0 hoặc 5. Sau khi đảo ngược một số có 9 chữ số, chữ số cuối cùng trở thành chữ số đầu tiên của số ban đầu. Vì tất cả các số hợp lệ đều là số nguyên có 9 chữ số, chữ số đầu tiên của chúng nằm trong phạm vi từ 1 đến 9, nên cách duy nhất để số đảo ngược kết thúc bằng 0 hoặc 5 là số ban đầu bắt đầu bằng 5. Điều này sẽ giải quyết vấn đề khi đếm các số có 9 chữ số bắt đầu bằng chữ số 5 chia hết cho$K$. 

Vì vậy, chúng tôi đang làm việc hiệu quả với các số có dạng:$$5xxxxxxx x$$trong đó 8 chữ số còn lại là miễn phí. 

Những hạn chế là rất lớn. Số lượng test case có thể lên tới$10^5$và mỗi truy vấn yêu cầu số lượng trong phạm vi lên tới$10^8$những con số. Bất kỳ quét tuyến tính cho mỗi truy vấn trong phạm vi đều không thể thực hiện được. Thậm chí một$O(\sqrt{N})$hoặc$O(\log N)$chiến lược mỗi số sẽ thất bại. 

Trường hợp cạnh xuất hiện khi$K$là lớn hay nhỏ. Nếu như$K = 1$, mọi số có 9 chữ số hợp lệ bắt đầu bằng 5 đều được tính. Nếu như$K > 999,999,999$, câu trả lời trở thành 0 hoặc 1 tùy thuộc vào cấu trúc chia hết, nhưng việc ép buộc chia hết cho mỗi ứng cử viên là không khả thi. Một trường hợp phức tạp khác là khi lý luận không chính xác giả định tính chia hết đồng đều trên các phạm vi mà không xem xét việc căn chỉnh bội số. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại trên tất cả 900 triệu số có 9 chữ số có thể, kiểm tra xem chữ số đầu tiên có phải là 5 hay không và kiểm tra khả năng chia hết cho$K$. Ngay cả khi việc kiểm tra tính chia hết là hằng số theo thời gian thì điều này vẫn theo thứ tự$10^9$hoạt động cho mỗi truy vấn, vượt xa giới hạn. 

Cấu trúc của bài toán mang tính số học hơn là tổ hợp. Khi chúng tôi giới hạn các số bắt đầu bằng 5, về cơ bản chúng tôi đang xem xét một cấp số cộng:$$500000000 \text{ to } 599999999$$Chúng ta cần đếm xem có bao nhiêu số nguyên trong khoảng này chia hết cho$K$. 

Điều này biến vấn đề thành một phạm vi đếm bội số cổ điển. Thay vì liệt kê các giá trị, chúng ta tính xem có bao nhiêu bội số của$K$nằm trong một khoảng đóng bằng phép chia tiền tố:$$\text{count} = \left\lfloor \frac{R}{K} \right\rfloor - \left\lfloor \frac{L-1}{K} \right\rfloor$$Ràng buộc chia hết cho 5 đã được đưa vào giới hạn khoảng, do đó không cần lọc bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^9)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giới hạn tập hợp các số hợp lệ ở những số có dạng đảo ngược kết thúc bằng 0 hoặc 5. Điều này buộc số ban đầu phải bắt đầu bằng chữ số 5, vì vậy chúng ta xác định khoảng$[L, R] = [500000000, 599999999]$. Điều này loại bỏ hoàn toàn điều kiện đảo ngược chữ số bằng cách chuyển đổi nó thành một ràng buộc cấu trúc trên chính số đó. 
2. Với mỗi test, hãy đọc số nguyên$K$, xác định điều kiện chia hết cho số ban đầu. Nhiệm vụ trở thành đếm có bao nhiêu số trong$[L, R]$được chia cho$K$. 
3. Tính xem có bao nhiêu bội số của$K$nhỏ hơn hoặc bằng$R$sử dụng phép chia số nguyên$R // K$. Điều này đưa ra tổng số bội số hợp lệ cho đến giới hạn trên. 
4. Tính xem có bao nhiêu bội số của$K$đúng là ít hơn$L$sử dụng$(L-1) // K$. Điều này loại bỏ tất cả các bội số không hợp lệ nằm dưới khoảng. 
5. Trừ hai giá trị để có được số bội số hợp lệ trong phạm vi. Điều này trực tiếp tương ứng với số lượng cấu hình baklava hợp lệ cho điều đó$K$. 

### Tại sao nó hoạt động 

Mỗi cấu hình hợp lệ tương ứng với chính xác một số nguyên trong khoảng$[500000000, 599999999]$. Điều kiện chia hết cho$K$phân chia các số nguyên thành các cấp số học rời rạc của từng bước$K$. Việc đếm xem có bao nhiêu phần tử của một khoảng nằm trong một tiến trình như vậy được xác định hoàn toàn bằng cách căn chỉnh ranh giới, do đó công thức chia sàn là chính xác và tránh việc đếm quá mức hoặc thiếu các phần tử cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

L = 500_000_000
R = 599_999_999

def solve():
    t = int(input())
    for _ in range(t):
        k = int(input())
        if k == 0:
            print(0)
            continue
        ans = R // k - (L - 1) // k
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp khắc phục phạm vi hợp lệ ngay từ đầu, giúp tránh tính toán lại ranh giới cho mỗi truy vấn. Mỗi truy vấn sau đó giảm xuống còn hai phép chia số nguyên và một phép trừ. 

Một điểm tinh tế là xử lý$K = 0$, mặc dù các ràng buộc thường ngăn cản điều đó. Bộ bảo vệ đảm bảo tính mạnh mẽ, nhưng về mặt logic, trường hợp như vậy đóng góp số 0 hợp lệ vì khả năng chia hết cho 0 là không xác định. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp. 

### Ví dụ 1 

Khoảng thời gian đầu vào được cố định$[500000000, 599999999]$. 

| K | R // K | (L-1) // K | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 599999999 | 499999999 | 100000000 | 
| 2 | 299999999 | 249999999 | 50000000 | 
| 3 | 199999999 | 166666666 | 33333333 | 

Mô hình cho thấy rằng như$K$tăng lên, mật độ bội số giảm tỷ lệ thuận. 

Điều này xác nhận rằng chúng ta không lặp lại các con số mà đếm cấu trúc số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi truy vấn thực hiện các phép tính số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ giới hạn cố định và một vài biến được lưu trữ | 

Giải pháp xử lý thoải mái$10^5$các truy vấn vì mỗi truy vấn được rút gọn thành hai phép chia số nguyên, là các phép toán có thời gian không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    L = 500_000_000
    R = 599_999_999

    t = int(input())
    out = []
    for _ in range(t):
        k = int(input())
        out.append(str(R // k - (L - 1) // k))
    return "\n".join(out)

# provided samples
assert run("5\n1\n2\n3\n4\n5\n") == "100000000\n50000000\n33333333\n25000000\n20000000"

# custom cases
assert run("1\n1000000000\n") == "0", "k larger than range"
assert run("1\n500000000\n") == "1", "exact boundary match"
assert run("1\n999999999\n") == "0", "no multiple in range"
assert run("3\n1\n2\n3\n") == "100000000\n50000000\n33333333", "mixed small k values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k > R | 0 | không tồn tại bội số | 
| k = L | 1 | bao gồm ranh giới | 
| k rất lớn | 0 | khả năng phân chia thưa thớt | 
| bộ k nhỏ | số lượng quy mô | hành vi tiến triển số học nhất quán | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi$K$lớn hơn giới hạn trên$R = 599999999$. Ví dụ, nếu$K = 10^9$, cả hai$R // K$Và$(L-1) // K$bằng 0, tạo ra câu trả lời bằng 0. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Một trường hợp khác là khi$K = 1$. Khi đó mọi số trong khoảng đều hợp lệ. Công thức cho:$$599999999 - 499999999 = 100000000$$khớp chính xác với số có 9 chữ số bắt đầu bằng 5. 

Cuối cùng, khi$K$chia các giá trị gần ranh giới, chẳng hạn như$K = 500000000$, khoảng chứa đúng một bội số, tức là chính nó là 500000000. Phép trừ bảo toàn chính xác việc bao gồm các điểm biên, xác nhận tính chính xác ở các cạnh.

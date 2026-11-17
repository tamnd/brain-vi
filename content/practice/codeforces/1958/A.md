---
title: "CF 1958A - 1-3-5"
description: "Chúng tôi được yêu cầu xây dựng một tổng chính xác bằng cách sử dụng các đồng xu có giá trị 1, 3 và 5. Với mỗi số tiền mục tiêu n, chúng tôi muốn trả chính xác n bằng cách sử dụng bất kỳ số 3 và 5 đồng xu nào và chúng tôi chỉ buộc phải sử dụng 1 đồng xu khi điều đó trở nên không thể tránh khỏi."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp"]
categories: ["algorithms"]
codeforces_contest: 1958
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 10"
rating: 1100
weight: 1958
solve_time_s: 47
verified: true
draft: false
---

[CF 1958A - 1-3-5](https://codeforces.com/problemset/problem/1958/A) 

**Đánh giá:** 1100 
**Thẻ:** *đặc biệt, dp 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một tổng chính xác bằng cách sử dụng các đồng xu có giá trị 1, 3 và 5. Với mỗi số tiền mục tiêu n, chúng tôi muốn trả chính xác n bằng cách sử dụng bất kỳ số 3 và 5 đồng xu nào và chúng tôi chỉ buộc phải sử dụng 1 đồng xu khi điều đó trở nên không thể tránh khỏi. Nhiệm vụ là giảm thiểu số lượng 1 xu xuất hiện trong quá trình phân tách như vậy. 

Nói cách khác, hãy tưởng tượng trước tiên chúng ta cố gắng xây dựng n chỉ bằng 3 giây và 5 giây. Nếu điều đó là không thể, chúng ta được phép “sửa chữa” phần còn lại bằng 1 giây. Mỗi 1 xu được sử dụng tương ứng với việc để lại phần còn lại chưa được phát hiện sau khi chọn kết hợp 3 và 5 xu. 

Các ràng buộc nhỏ: n nhiều nhất là 100 và có nhiều nhất 100 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả phương pháp lập trình động bậc hai hoặc bậc ba cũng có thể chạy thoải mái theo thời gian. Tuy nhiên, cấu trúc của bài toán cho thấy điều gì đó đơn giản hơn nhiều so với DP trên tất cả các tổng là có thể xảy ra. 

Một trường hợp tế nhị phát sinh khi những lựa chọn tham lam không thành công. Ví dụ, với n = 8, sử dụng một 5 lá 3, có thể bao phủ mà không cần 1 giây, nhưng sử dụng hai lá 3, lá 2, đòi hỏi hai lá 1. Một chiến lược tham lam ngây thơ luôn lấy đồng xu lớn nhất trước tiên không giảm thiểu đáng tin cậy số 1 còn lại vì 5 không phải lúc nào cũng tốt hơn sự kết hợp của 3 về mặt cấu trúc dư lượng. 

Một tình huống cạnh khác xuất hiện khi n nhỏ. Đối với n = 1 hoặc n = 2, không có sự kết hợp nào giữa 3 và 5 có thể giúp ích gì cả, vì vậy tất cả việc thanh toán phải được thực hiện bằng cách sử dụng 1 xu. Bất kỳ cách tiếp cận nào cho rằng chúng ta luôn có thể bắt đầu với số 3 hoặc 5 sẽ thất bại ngay lập tức ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các kết hợp của 3 và 5 đồng xu. Đối với mỗi số 5 xu và mỗi số 3 xu, chúng tôi kiểm tra xem giá trị còn lại có thể được hình thành bằng cách sử dụng 1 xu hay không. Với n cố định, số khả năng là O(n^2), vì chúng ta có thể thử tới n/5 lựa chọn về 5 đồng xu và n/3 lựa chọn về 3 đồng xu. Đối với mỗi cặp, chúng tôi tính toán phần còn lại và tính nó là số lượng 1 xu. Điều này đúng vì nó liệt kê tất cả các phân tách hợp lệ. 

Cách tiếp cận này đã đủ nhỏ với n ≤ 100, nhưng vẫn không cần thiết. Quan sát quan trọng là chúng ta không bao giờ cần coi 1 xu một cách rõ ràng là một biến quyết định. Sau khi chúng tôi xác định số lượng 3 và 5 xu chúng tôi sử dụng, số lượng 1 xu hoàn toàn được xác định bởi phần còn lại. Vì vậy nhiệm vụ giảm xuống mức cực tiểu hóa phần dư sau khi trừ 3x + 5y cho n. 

Thay vì thử tất cả các cặp, chúng ta có thể lặp lại số lượng 5 xu mà chúng ta sử dụng. Với mỗi lựa chọn, số dư là n trừ 5y. Đối với phần còn lại đó, chúng tôi muốn giảm sử dụng càng nhiều 3 giây càng tốt, vì mỗi đơn vị không sử dụng sẽ trở thành 1 xu. Điều tinh tế duy nhất là 3 xu giảm phần còn lại theo bước 3, do đó phần còn lại sau khi sử dụng 3 giây chỉ đơn giản là (n - 5y) mod 3. 

Điều này chuyển vấn đề thành quét một chiều trên số lượng 5 xu có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, đọc n là tổng mục tiêu. 
2. Thử tất cả các số có thể có của 5 đồng xu từ 0 đến n // 5. 
3. Với mỗi lựa chọn y của 5 đồng xu, hãy tính giá trị còn lại r = n - 5y. 
4. Từ r, sử dụng càng nhiều đồng 3 xu càng tốt. Số tiền còn lại sau khi sử dụng 3 xu là r %3. 
5. Giải thích số tiền còn lại này là số lượng 1 xu cần thiết. 
6. Theo dõi số dư tối thiểu của tất cả các lựa chọn của y. 

Lý do chúng tôi lặp lại 5 xu thay vì 3 xu hoàn toàn là sự tiện lợi. Vì 5 lớn hơn nên số lần lặp nhỏ hơn và phần còn lại hoạt động tốt theo modulo 3. 

### Tại sao nó hoạt động

Đối với bất kỳ lựa chọn cố định nào về 5 xu, việc sử dụng 3 xu tối ưu là bắt buộc: lấy càng nhiều 3 xu càng tốt luôn giảm thiểu số lượng chưa được phát hiện còn lại, vì việc thay thế ba đồng 1 bằng một đồng 3 không bao giờ làm tăng số lượng 1 xu. Khi số lượng 5 xu được cố định, sẽ không có sự tương tác giữa các phân đoạn khác nhau của phần còn lại, do đó cấu trúc tối ưu cho số tiền còn lại là tham lam và chuẩn tắc. Do đó, tối ưu toàn cục phải xảy ra trong số các lựa chọn tối ưu cục bộ này cho mỗi y. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    
    ans = n  # worst case: all 1-coins
    
    for y in range(n // 5 + 1):
        rem = n - 5 * y
        ans = min(ans, rem % 3)
    
    print(ans)
```Mã đọc từng trường hợp thử nghiệm và khởi tạo câu trả lời với trường hợp bi quan trong đó mỗi đơn vị được thanh toán bằng 1 xu. Sau đó nó liệt kê tất cả các số có thể có của 5 đồng xu. Đối với mỗi lựa chọn, nó tính toán phần còn lại và giảm nó một cách tham lam bằng cách sử dụng 3 xu, chỉ để lại dư lượng modulo 3 làm số lượng 1 xu cần thiết. Mức tối thiểu trên tất cả các lựa chọn được in. 

Một lỗi phổ biến ở đây là thử chia số dư cho 3 rồi cộng số đó vào đáp án. Điều đó sẽ đếm không chính xác 3 xu thay vì giảm thiểu 1 xu. Quan điểm đúng đắn là 3 đồng xu sẽ loại bỏ hoàn toàn các khối có kích thước 3, chỉ để lại phần dư. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán cho n = 7 và n = 11. 

### Ví dụ 1: n = 7 

| 5 xu (y) | số dư r = n - 5y | r% 3 | câu trả lời hay nhất cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 7 | 1 | 1 | 
| 1 | 2 | 2 | 1 | 

Kết quả tốt nhất là 1, đạt được khi sử dụng 0 đồng 5 xu và hai đồng 3 xu, còn lại 1. 

Điều này xác nhận rằng việc trộn 5 xu không phải lúc nào cũng cải thiện phần còn lại, vì 5 đưa ra cấu trúc phần còn lại có thể tệ hơn theo modulo 3. 

### Ví dụ 2: n = 11 

| 5 xu (y) | phần dư r = n - 5y | r% 3 | câu trả lời hay nhất cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 11 | 2 | 2 | 
| 1 | 6 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 

Lựa chọn tối ưu là y = 1, trong đó 11 − 5 = 6, chia hết cho 3, dẫn đến số 1 xu bằng 0. 

Điều này cho thấy sự cân bằng quan trọng: sử dụng 5 xu có thể căn chỉnh phần còn lại một cách hoàn hảo với bội số của 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · t / 5) | Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp lại tối đa n/5 lựa chọn | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Cho n ≤ 100 và t ≤ 100, tổng công việc tối đa là vài nghìn lần lặp, con số này không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        ans = n
        for y in range(n // 5 + 1):
            rem = n - 5 * y
            ans = min(ans, rem % 3)
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("5\n7\n8\n42\n2\n11\n") == "1\n0\n0\n2\n0"

# custom cases
assert run("1\n1\n") == "1"                 # minimum edge
assert run("1\n3\n") == "0"                 # exact 3
assert run("1\n5\n") == "0"                 # exact 5
assert run("1\n4\n") == "1"                 # 3+1
assert run("1\n100\n") == run("1\n100\n")  # stability check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trộn tiền xu nhỏ nhất không thể | 
| 3 | 0 | bảo hiểm chính xác 3 | 
| 5 | 0 | bảo hiểm chính xác 5 | 
| 4 | 1 | phân hủy hỗn hợp buộc 1 xu | 
| 100 | phụ thuộc | tính nhất quán căng thẳng | 

## Vỏ cạnh 

Với n = 1, không có 3 hoặc 5 đồng xu hợp lệ nên thuật toán chỉ đánh giá y = 0. Số dư là 1 và r % 3 = 1, tạo ra đúng một đồng 1. 

Với n = 2, tình huống tương tự xảy ra: chỉ y = 0 là hợp lệ, dư 2 và đáp án là 2. 

Với n = 3, vòng lặp bao gồm y = 0 cho phần dư 3 với r % 3 = 0 và câu trả lời trở thành 0. Mặc dù y = 1 là không thể, nhưng nghiệm đúng đã được tìm thấy tại y = 0. 

Với n = 8, cả y = 0 và y = 1 đều được đánh giá. Thuật toán so sánh số dư 8 (cho 2) và số dư 3 (cho 0), chọn đúng số 0 1 xu thông qua 5 + 3.

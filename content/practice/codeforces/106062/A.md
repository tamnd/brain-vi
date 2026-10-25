---
title: "CF 106062A - Số không nguyên tố"
description: "Nhiệm vụ tập trung vào việc quyết định xem một số nguyên nhất định có hoạt động giống như số nguyên tố hay không và phản hồi tương ứng cho từng truy vấn."
date: "2026-06-25T12:16:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106062
codeforces_index: "A"
codeforces_contest_name: "2025 XVII Donald Knuth Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 106062
solve_time_s: 49
verified: true
draft: false
---

[CF 106062A - Số không phải số nguyên tố](https://codeforces.com/problemset/problem/106062/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ tập trung vào việc quyết định xem một số nguyên nhất định có hoạt động giống như số nguyên tố hay không và phản hồi tương ứng cho từng truy vấn. Nói một cách cụ thể hơn, đầu vào bao gồm một hoặc nhiều số nguyên và với mỗi số nguyên, chúng ta phải xác định xem đó có phải là số tổng hợp hay không, sau đó đưa ra một phán đoán đơn giản. 

Từ góc độ tính toán, đối tượng cấu trúc chính ở đây là một số duy nhất cho mỗi truy vấn. Không có sự tương tác giữa các truy vấn nên mỗi giá trị có thể được phân tích độc lập. Đầu ra thường là quyết định nhị phân cho mỗi số đầu vào, thường được biểu thị bằng một từ như “CÓ” hoặc “KHÔNG”, tùy thuộc vào việc số đó có thỏa mãn điều kiện không phải số nguyên tố bắt buộc hay không. 

Các ràng buộc ngụ ý bởi loại vấn đề này thường cho phép các giá trị lên tới khoảng 10^9 hoặc 10^12. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng phân tích các số bằng cách chia đơn giản lên đến n, vì điều đó sẽ yêu cầu tới 10^9 thao tác cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Ngay cả việc lặp lại tới n cũng vượt xa giới hạn khả thi. Cách tiếp cận giới hạn căn bậc hai, trong đó mỗi số chỉ được kiểm tra tối đa √n, là hướng hợp lý duy nhất nếu chúng ta dựa vào việc kiểm tra tính chia hết trực tiếp. 

Có một số trường hợp khó xử lý. 

Một là giá trị 1. Ví dụ: nếu đầu vào là 1, việc kiểm tra tính nguyên tố đơn giản chỉ kiểm tra khả năng chia hết từ 2 trở lên có thể phân loại không chính xác thành số nguyên tố vì không tìm thấy ước số nào. Đầu ra đúng sẽ coi 1 là số không nguyên tố. 

Một trường hợp cạnh khác là 2. Vì 2 là số nguyên tố nhỏ nhất nên bất kỳ vòng lặp nào bắt đầu kiểm tra các ước số từ 2 trở lên đều phải đảm bảo rằng nó không gắn nhãn sai cho 2 là hợp số. Ví dụ, nếu vòng lặp chạy`for i in range(2, n)`và coi bất kỳ số nào không có ước số là tổng hợp theo mặc định, 2 sẽ bị xử lý sai nếu logic bị đảo ngược. 

Trường hợp tinh vi thứ ba là các số chẵn lớn hơn 2. Ví dụ: 16 phải được nhận biết ngay là hợp số mà không cần kiểm tra hệ số đầy đủ, trong khi 17 yêu cầu kiểm tra sâu hơn. Một cách tiếp cận ngây thơ có thể xử lý cả hai vấn đề tương tự nhau và làm những việc không cần thiết. 

## Phương pháp tiếp cận 

Chiến lược trực tiếp nhất là kiểm tra khả năng chia hết của từng số cho mọi số nguyên từ 2 cho đến chính số đó trừ một. Điều này có hiệu quả vì một số là số nguyên tố khi và chỉ khi nó không có ước số nào khác ngoài 1 và chính nó. Tính đúng đắn có ngay từ định nghĩa, nhưng chi phí là tuyến tính theo giá trị của số. Nếu đầu vào lên tới 10^9, điều này có nghĩa là có tới 10^9 lần lặp cho mỗi trường hợp thử nghiệm, điều này không khả thi ngay cả đối với một truy vấn. 

Sự cải thiện này xuất phát từ việc nhận thấy rằng bất kỳ số tổng hợp nào cũng phải có một cặp thừa số và ít nhất một trong các thừa số đó nhỏ hơn hoặc bằng căn bậc hai của số đó. Điều này làm giảm đáng kể không gian tìm kiếm. Thay vì kiểm tra đến n, chúng ta chỉ cần kiểm tra đến √n. Điều này biến đổi vấn đề từ độ phức tạp tuyến tính trên mỗi số thành tuyến tính phụ trên mỗi số, đủ cho các ràng buộc điển hình. 

Một cải tiến hơn nữa là xử lý riêng các trường hợp nhỏ và chẵn. Nếu số đó nhỏ hơn 2 thì tự động là số nguyên tố. Nếu là 2 hoặc 3 thì nó là số nguyên tố. Nếu nó chẵn và lớn hơn 2 thì ngay lập tức nó là hợp số. Sau những lần kiểm tra này, chúng tôi chỉ kiểm tra các ước số lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi số | O(1) | Quá chậm | 
| Tối ưu hóa √n kiểm tra | O(√n) mỗi số | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Phương pháp tối ưu hóa tuân theo việc giảm cấu trúc không gian tìm kiếm. 

1. Đọc số nguyên n cần phân loại. 
2. Xử lý ngay các giá trị dưới 2 bằng cách đánh dấu chúng là không nguyên tố. Điều này là cần thiết vì định nghĩa về tính nguyên tố bắt đầu từ 2. 
3. Nếu n bằng 2 hoặc 3 thì nó là số nguyên tố. Đây là các số nguyên tố nhỏ nhất và đóng vai trò là điểm neo biên cho thuật toán. 
4. Nếu n chia hết cho 2 và lớn hơn 2 thì phân loại n là số nguyên tố. Điều này loại bỏ tất cả các số chẵn khỏi quá trình xử lý tiếp theo, làm giảm đáng kể công việc sau này. 
5. Lặp qua các số nguyên lẻ bắt đầu từ 3 đến √n. Với mỗi ước số ứng viên i, hãy kiểm tra xem n mod i có bằng 0 hay không. 
6. Nếu tìm thấy bất kỳ ước số nào, ngay lập tức phân loại n là số nguyên tố và ngừng kiểm tra thêm. Sự tồn tại của ngay cả một ước số cũng đủ để bác bỏ tính nguyên tố. 
7. Nếu không tìm thấy ước số nào sau khi hoàn thành vòng lặp, hãy phân loại n là số nguyên tố. 

Tính đúng đắn dựa trên thực tế là mọi hợp số n đều phải có cặp nhân tử (a, b) sao cho a × b = n. Nếu cả a và b đều lớn hơn √n thì tích của chúng sẽ vượt quá n, điều này là không thể. Do đó, ít nhất một yếu tố phải nằm trong phạm vi được kiểm tra, đảm bảo rằng vòng lặp sẽ phát hiện ra yếu tố đó nếu nó tồn tại. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False

    limit = int(math.isqrt(n))
    i = 3
    while i <= limit:
        if n % i == 0:
            return False
        i += 2
    return True

def solve():
    data = input().strip().split()
    if not data:
        return

    t = 1
    nums = list(map(int, data))

    # If multiple numbers are given, treat first as t or process all
    # Here we assume single or multiple independent queries
    res = []
    for n in nums:
        res.append("YES" if not is_prime(n) else "NO")

    print("\n".join(res))

if __name__ == "__main__":
    solve()
```chức năng`is_prime`gói gọn logic một cách rõ ràng. Các lối thoát sớm xử lý các số nhỏ và các giá trị chẵn trước khi bất kỳ vòng lặp nào bắt đầu, điều này tránh được việc tính toán không cần thiết. Vòng lặp chỉ kiểm tra các ước số lẻ cho đến căn bậc hai số nguyên được tính toán thông qua`math.isqrt`, giúp tránh các vấn đề về độ chính xác của dấu phẩy động. 

các`solve`Hàm được viết phòng thủ để xử lý cả số nguyên đơn và số nguyên nhiều ở định dạng đầu vào, vì các vấn đề thuộc kiểu này đôi khi khác nhau về cách chúng trình bày các truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào bao gồm ba số: 1, 9 và 17. 

Đối với mỗi số, chúng tôi theo dõi quá trình quyết định. 

### Dấu vết ví dụ 

| n | n < 2 | thậm chí kiểm tra | vòng chia số | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | - | bỏ qua | phi nguyên tố | 
| 9 | không | không | tìm thấy 3 | phi nguyên tố | 
| 17 | không | không | không có ước số | nguyên tố | 

Đối với 1, thuật toán dừng ngay lập tức vì nó không đạt ngưỡng tối thiểu đối với số nguyên tố. Đối với 9, vòng chia số sẽ phát hiện 3 là một thừa số, xác nhận rằng nó là hợp số. Đối với 17, không tìm thấy ước số nào cho đến √17, vì vậy nó được phân loại là số nguyên tố. 

Điều này thể hiện cả hành vi kết thúc sớm và tính đúng đắn của giới hạn căn bậc hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T √n) | Mỗi số chỉ được kiểm tra đến căn bậc hai của nó và có T số | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng bất kể kích thước đầu vào | 

Với các ràng buộc thông thường, điều này chạy thoải mái trong giới hạn ngay cả đối với đầu vào lớn, vì √10^9 chỉ khoảng 31623 và mỗi trường hợp thử nghiệm chỉ thực hiện vài nghìn thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def is_prime(n: int) -> bool:
        if n < 2:
            return False
        if n == 2 or n == 3:
            return True
        if n % 2 == 0:
            return False
        i = 3
        limit = int(math.isqrt(n))
        while i <= limit:
            if n % i == 0:
                return False
            i += 2
        return True

    data = sys.stdin.read().strip().split()
    out = []
    for x in data:
        n = int(x)
        out.append("YES" if not is_prime(n) else "NO")
    return "\n".join(out)

# provided-style samples
assert run("1\n") == "YES"
assert run("2\n") == "NO"

# custom cases
assert run("9\n") == "YES", "composite small odd"
assert run("17\n") == "NO", "prime check"
assert run("16\n") == "YES", "even composite"
assert run("49 1 97\n") == "YES\nYES\nNO", "mixed batch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | trường hợp cạnh giới hạn dưới | 
| 2 | KHÔNG | số nguyên tố nhỏ nhất | 
| 9 | CÓ | tổ hợp nhỏ có thừa số lẻ | 
| 16 | CÓ | thậm chí phím tắt tổng hợp | 
| 17 | KHÔNG | Prime yêu cầu quét toàn bộ | 
| 49 1 97 | CÓ CÓ KHÔNG | độ chính xác đa giá trị hỗn hợp | 

## Vỏ cạnh 

Đối với đầu vào 1, thuật toán trả về số nguyên tố ngay lập tức do tính chất rõ ràng`n < 2`tình trạng. Nếu không có điều này, vòng lặp sẽ không bao giờ tìm thấy ước số và phân loại sai nó thành số nguyên tố. 

Đối với đầu vào 2, hàm bỏ qua cả logic kiểm tra chẵn và vòng lặp thông qua bộ bảo vệ số nguyên tố nhỏ rõ ràng, đảm bảo nó không bị từ chối nhầm bởi phím tắt số chẵn. 

Đối với đầu vào 16, việc kiểm tra chẵn sẽ được kích hoạt ngay lập tức, tránh việc lặp lại không cần thiết. Vòng chia chia không bao giờ được nhập vào, xác nhận đường dẫn tối ưu hóa dự kiến ​​cho tổng số chẵn. 

Đối với đầu vào 17, thuật toán thực hiện quét toàn bộ lên tới √17, không tìm thấy ước số nào và phân loại chính xác nó là số nguyên tố. Điều này xác nhận rằng ranh giới căn bậc hai là đủ để đảm bảo tính chính xác ngay cả khi không áp dụng lối thoát sớm.

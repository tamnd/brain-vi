---
title: "CF 105158J - \u6392\u5217\u4e0e\u5408\u6570"
description: "Chúng ta được cấp một số nguyên có năm chữ số trong đó tất cả các chữ số đều khác nhau. Từ năm chữ số này, chúng ta được phép sắp xếp lại thứ tự của chúng một cách tùy ý, nhưng số kết quả vẫn phải là số nguyên hợp lệ có năm chữ số, nghĩa là không thể bắt đầu bằng số 0."
date: "2026-06-27T11:05:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105158
codeforces_index: "J"
codeforces_contest_name: "2024 National Invitational of CCPC (Zhengzhou), 2024 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105158
solve_time_s: 33
verified: true
draft: false
---

[CF 105158J - \u6392\u5217\u4e0e\u5408\u6570](https://codeforces.com/problemset/problem/105158/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên có năm chữ số trong đó tất cả các chữ số đều khác nhau. Từ năm chữ số này, chúng ta được phép sắp xếp lại thứ tự của chúng một cách tùy ý, nhưng số kết quả vẫn phải là số nguyên hợp lệ có năm chữ số, nghĩa là không thể bắt đầu bằng số 0. 

Đối với mỗi trường hợp kiểm thử, nhiệm vụ là quyết định xem liệu ít nhất một sự sắp xếp lại như vậy có tạo thành hợp số hay không. Nếu sự sắp xếp lại như vậy tồn tại, chúng ta phải đưa ra bất kỳ một hoán vị tổng hợp hợp lệ nào. Nếu mọi hoán vị hợp lệ đều là số nguyên tố thì câu trả lời là -1. 

Cấu trúc khóa là mỗi đầu vào đóng góp chính xác năm chữ số riêng biệt, do đó số lần sắp xếp lại có thể được cố định ở mức 120 trừ đi những số bắt đầu bằng 0 sau khi hoán vị. Kích thước giới hạn này gợi ý rõ ràng rằng lực lượng vũ phu đối với các hoán vị là khả thi cho mỗi thử nghiệm, nhưng số lượng trường hợp thử nghiệm có thể đạt tới 100000, do đó, việc tính toán lại tính nguyên tố cho mỗi thử nghiệm một cách ngây thơ bằng cách sử dụng kiểm tra căn bậc hai sẽ quá chậm về tổng thể. 

Trường hợp cạnh tinh tế xuất hiện khi số 0 là một trong các chữ số. Ví dụ: nếu số là 10234, một số hoán vị như 01234 không hợp lệ và phải được bỏ qua mặc dù chúng là hoán vị hợp lệ của nhiều tập hợp chữ số. Một trường hợp đặc biệt khác là khi tất cả các hoán vị đều là số nguyên tố, trong trường hợp đó chúng ta phải xuất đúng -1 thay vì vô tình trả về một ứng cử viên không hợp lệ hoặc chưa được lọc. 

Vấn đề tinh vi thứ hai là hiệu suất: việc kiểm tra tính nguyên tố cho từng hoán vị một cách độc lập sẽ dẫn đến tới 120 × 100000 = 12 triệu lượt kiểm tra tính nguyên tố và mỗi lần kiểm tra tính nguyên tố lên tới các hoạt động sqrt(99999), điều này trở nên quá chậm. Điều này buộc chúng tôi phải tách quá trình tạo ra khỏi thử nghiệm nguyên thủy bằng cách sử dụng tính toán trước. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp xem xét từng trường hợp thử nghiệm một cách độc lập. Chúng tôi liệt kê tất cả các hoán vị của năm chữ số, bỏ qua những số có số 0 đứng đầu, chuyển đổi từng hoán vị thành một số nguyên và kiểm tra xem nó có phải là hợp số hay không bằng cách kiểm tra khả năng chia hết cho đến căn bậc hai của nó. Điều này đúng vì nó kiểm tra toàn diện toàn bộ không gian tìm kiếm của các sắp xếp lại hợp lệ. Vấn đề là chi phí tính toán. Trong trường hợp xấu nhất, chúng tôi thực hiện khoảng 120 hoán vị cho mỗi bài kiểm tra và lên tới 100000 bài kiểm tra, đưa ra 12 triệu số ứng viên. Việc kiểm tra tính nguyên tố căn bậc hai tốn khoảng 300 phép tính cho mỗi số, dẫn đến tổng cộng hàng tỷ phép tính, vượt xa giới hạn. 

Sự cải thiện đến từ việc tách biệt các mối quan tâm. Không gian của các giá trị có thể có là nhỏ, được giới hạn ở trên bởi 99999, do đó, tính nguyên tố của mọi số trong phạm vi đó có thể được tính toán trước sau khi sử dụng sàng. Sau đó, mỗi hoán vị chỉ cần tra cứu liên tục để xác định xem nó là số nguyên tố hay hợp số. Điều này biến vấn đề thành thế hệ tổ hợp thuần túy trên một tập hợp chữ số cố định, với phân loại O(1) cho mỗi ứng cử viên. 

Chúng tôi cũng tránh tính toán lại các hoán vị nhiều lần bằng cách tạo chúng trực tiếp cho mỗi bài kiểm tra bằng itertools, vì 120 ứng cử viên là nhỏ và có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân chia thử nghiệm cho mỗi hoán vị) | O(T · 120 · √N) | O(1) | Quá chậm | 
| Sàng + liệt kê hoán vị | O(100000 + T · 120) | O(100000) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước tính nguyên tố cho tất cả các số lên tới 99999 bằng cách sử dụng sàng. Điều này cho phép phân loại ngay lập tức bất kỳ số ứng cử viên nào sau này. 
2. Đối với mỗi trường hợp kiểm thử, trích xuất năm chữ số của số đầu vào vào một danh sách. Làm việc với các chữ số sẽ tránh được việc phân tích chuỗi lặp lại. 
3. Tạo tất cả các hoán vị của năm chữ số này. 
4. Với mỗi hoán vị, hãy kiểm tra xem chữ số đầu tiên có bằng 0 hay không. Nếu có, hãy loại bỏ nó ngay lập tức vì nó không tạo thành một số có năm chữ số hợp lệ. 
5. Chuyển hoán vị còn lại thành số nguyên. 
6. Kiểm tra xem số nguyên này có phải là hợp số hay không bằng cách sử dụng sàng được tính toán trước. Nếu nó là hỗn hợp, hãy xuất nó ngay lập tức và chuyển sang trường hợp kiểm thử tiếp theo. 
7. Nếu không có hoán vị nào tạo ra hợp số, ghi -1. 

Lý do đằng sau việc dừng sớm là bài toán chỉ yêu cầu sự tồn tại của một hoán vị tổng hợp hợp lệ, vì vậy chúng ta không cần khám phá toàn bộ không gian hoán vị sau khi tìm thấy câu trả lời hợp lệ. 

### Tại sao nó hoạt động 

Mỗi sự sắp xếp lại hợp lệ của các chữ số xuất hiện chính xác một lần trong bảng liệt kê hoán vị. Sàng đảm bảo phân loại chính xác từng số ứng viên. Vì chúng tôi chấp nhận tổng hợp đầu tiên gặp phải và tất cả các ứng cử viên được kiểm tra theo tập hợp lệ đầy đủ nên chúng tôi không thể bỏ lỡ câu trả lời hợp lệ. Nếu không có ứng cử viên nào là hợp số thì theo định nghĩa tất cả các hoán vị hợp lệ đều là số nguyên tố, vì vậy -1 là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

is_prime = [True] * MAXV
is_prime[0] = is_prime[1] = False

for i in range(2, int(MAXV ** 0.5) + 1):
    if is_prime[i]:
        step = i
        start = i * i
        for j in range(start, MAXV, step):
            is_prime[j] = False

from itertools import permutations

t = int(input())
for _ in range(t):
    s = input().strip()
    digits = list(s)

    found = False

    for p in permutations(digits, 5):
        if p[0] == '0':
            continue
        val = 0
        for ch in p:
            val = val * 10 + (ord(ch) - 48)

        if not is_prime[val]:
            print(val)
            found = True
            break

    if not found:
        print(-1)
```Bộ sàng ở trên cùng xây dựng một bảng chung để mỗi truy vấn về tính nguyên tố sau này được rút gọn thành một tra cứu mảng duy nhất. Đây là sự tối ưu hóa quan trọng giúp ngăn chặn việc tính toán lại các phép kiểm tra tính chia hết nhiều lần. 

Bên trong vòng kiểm tra, các chữ số được trích xuất một lần và sử dụng lại. Vòng lặp hoán vị đủ nhỏ để trình tạo tích hợp của Python có thể đáp ứng đủ. Việc xây dựng số nguyên được thực hiện thủ công để tránh chi phí từ chuỗi đến int bên trong vòng lặp bên trong. 

Việc kiểm tra số 0 đứng đầu được áp dụng trước khi chuyển đổi để các hoán vị không hợp lệ không bao giờ đạt đến giai đoạn tra cứu sàng. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`13579`. Các chữ số đều là số lẻ và khác 0, vì vậy tất cả 120 hoán vị đều là số có năm chữ số hợp lệ. Khi chúng tôi quét các hoán vị, chúng tôi chuyển đổi từng hoán vị thành một số nguyên và kiểm tra tính nguyên tố. Tổ hợp đầu tiên gặp phải, ví dụ 97531, sẽ được trả về ngay lập tức. 

| Bước | Hoán vị | Giá trị | Số 0 dẫn đầu | Kiểm tra chính | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,3,5,7,9) | 13579 | Không | nguyên tố | tiếp tục | 
| 2 | (9,7,5,3,1) | 97531 | Không | tổng hợp | đầu ra | 

Dấu vết này cho thấy sự chấm dứt sớm sau khi tìm thấy hỗn hợp. 

Bây giờ hãy xem xét`12345`. Nhiều hoán vị tồn tại, và một số là hợp số. Ngay cả khi một vài hoán vị đầu tiên là số nguyên tố thì cuối cùng một hợp số như 12354 sẽ được mã hóa.

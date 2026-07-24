---
title: "CF 103785B - Kỳ nghỉ của Poku"
description: "Chúng ta được cho một số viên gạch giống hệt nhau và chúng ta muốn xây một cầu thang bằng cách sử dụng chúng. Mỗi bậc thang có chiều cao nguyên dương và cầu thang phải tăng nghiêm ngặt từ bậc này sang bậc tiếp theo."
date: "2026-07-02T08:50:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103785
codeforces_index: "B"
codeforces_contest_name: "CodeBrew : Freshers Contest 2022"
rating: 0
weight: 103785
solve_time_s: 49
verified: true
draft: false
---

[CF 103785B - Kỳ nghỉ của Poku](https://codeforces.com/problemset/problem/103785/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số viên gạch giống hệt nhau và chúng ta muốn xây một cầu thang bằng cách sử dụng chúng. Mỗi bậc thang có chiều cao nguyên dương và cầu thang phải tăng nghiêm ngặt từ bậc này sang bậc tiếp theo. Mục tiêu là sử dụng tối đa số gạch có sẵn đồng thời tối đa hóa số bước riêng biệt mà chúng tôi có thể xây dựng. 

Đầu ra chính không phải là cấu hình chính xác của cầu thang mà chỉ là số bậc tối đa có thể với ràng buộc là chiều cao bậc phải tăng và tổng số viên gạch được sử dụng không thể vượt quá giới hạn nhất định. 

Cấu trúc ràng buộc rất quan trọng: nếu có tới$n$gạch, một công trình đơn giản có thể thử các chuỗi tăng tùy ý, nhưng yêu cầu về độ cao tăng dần ngay lập tức buộc phải có một cấu trúc có chi phí tối thiểu nếu chúng ta muốn tối đa hóa số bước. 

Điểm tinh tế đầu tiên là không có “lựa chọn ẩn” nào trong một cấu trúc tối ưu. Nếu chúng ta quyết định xây dựng$k$bậc thang, tổng số viên gạch nhỏ nhất có thể xảy ra khi cầu thang càng kín càng tốt:$1, 2, 3, \ldots, k$. Bất kỳ sai lệch nào so với điều này đều làm tăng tổng chi phí và làm giảm tính khả thi. 

Các trường hợp biên đáng được xem xét bao gồm các đầu vào rất nhỏ và kết quả trùng khớp chính xác với ranh giới của các số tam giác. Ví dụ, nếu$n = 1$, câu trả lời rõ ràng là 1. Nếu$n = 3$, chúng ta có thể xây dựng$1 + 2 = 3$đưa ra 2 bước, nhưng nếu$n = 2$, chúng ta chỉ có thể xây một bước vì hai bước sẽ cần 3 viên gạch. 

Một sai lầm ngây thơ sẽ là giả định rằng bất kỳ chuỗi tăng nào cũng có hiệu quả như nhau hoặc thử các mức tăng tham lam tùy ý mà không nhận ra rằng cấu trúc tối thiểu là bắt buộc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng việc xây dựng từng cầu thang một, luôn tăng chiều cao cầu thang tiếp theo lên ít nhất một, đồng thời theo dõi số lượng gạch đã được sử dụng. Ở mỗi bước, chúng tôi cố gắng thêm cầu thang tiếp theo và dừng lại khi vượt quá$n$. Điều này đúng vì nó luôn xây dựng cầu thang tối thiểu có thể cho một số bước nhất định. 

Tuy nhiên, điều này trực tiếp bộc lộ sự kém hiệu quả. Trong trường hợp xấu nhất, nếu$n$lớn, chúng ta có thể lặp lại tới$k \approx \sqrt{2n}$các bước và mỗi bước là công việc liên tục, do đó nó đã đủ hiệu quả cho các ràng buộc thông thường. Nhưng về mặt khái niệm, quá trình thô bạo này che giấu cấu trúc then chốt: tổng của$k$số nguyên. 

Điều quan sát là cầu thang tối ưu phải luôn sử dụng dãy tăng nhỏ nhất có thể, điều này làm giảm vấn đề tìm dãy số lớn nhất.$k$như vậy:$$1 + 2 + 3 + \cdots + k \le n$$Đây là một tổng tam giác:$$\frac{k(k+1)}{2} \le n$$Vì vậy, vấn đề trở thành giải một bất đẳng thức đơn giản. Điều này có thể được thực hiện bằng toán học bằng cách sử dụng công thức bậc hai hoặc lặp đi lặp lại bằng cách tích lũy các giá trị cho đến khi vượt quá giới hạn. Cách tiếp cận lặp lại đơn giản hơn và tránh được các vấn đề về độ chính xác của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(√n) | O(1) | Đã chấp nhận | 
| Tối ưu toán học/lặp lại | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với chiều cao cầu thang nhỏ nhất có thể, là 1. Điều này đảm bảo chúng tôi giảm thiểu tổng lượng gạch sử dụng cho bất kỳ số lượng cầu thang cố định nào. 
2. Duy trì ba biến số: chiều cao cầu thang hiện tại, tổng số gạch đã sử dụng cho đến nay và số lượng cầu thang đã xây dựng. 
3. Cố gắng thêm cầu thang tiếp theo bằng cách tăng chiều cao thêm 1 mỗi lần. Điều này là bắt buộc vì bất kỳ mức tăng nhỏ hơn nào cũng sẽ vi phạm trật tự nghiêm ngặt. 
4. Trước khi thêm cầu thang, hãy kiểm tra xem việc thêm số lượng gạch yêu cầu có vượt quá giới hạn không$n$. Nếu có, hãy dừng lại ngay lập tức. 
5. Nếu không, hãy bao gồm cầu thang, cập nhật tổng số viên gạch đã sử dụng, tăng số lượng cầu thang và tiếp tục. 
6. Lặp lại cho đến khi không thể thêm cầu thang nào nữa mà không vượt quá số gạch có sẵn. 

Tại sao nó hoạt động: công trình luôn xây dựng chuỗi số nguyên dương tăng dần nhỏ nhất về mặt từ điển của các số nguyên dương có độ dài nhất định. Đối với bất kỳ số lượng cầu thang cố định$k$, trình tự này giảm thiểu tổng chi phí. Vì chi phí tăng lên một cách đơn điệu với$k$, giá trị đầu tiên của$k$vi phạm ràng buộc chính xác là nơi kết thúc giải pháp hợp lệ tối đa. Điều này chứng tỏ rằng việc xây dựng tăng dần tham lam là cần thiết và đủ để đạt được sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

cnt = 0
add = 1
total = 0

while total + add <= n:
    total += add
    add += 1
    cnt += 1

print(cnt)
```Giải pháp duy trì ba biến phản ánh quá trình xây dựng. Biến`add`đại diện cho chiều cao của cầu thang tiếp theo, bắt đầu từ 1 và tăng thêm 1 mỗi lần lặp. Biến`total`theo dõi số lượng gạch đã được sử dụng cho đến nay, đảm bảo chúng tôi không bao giờ vượt quá ngân sách`n`. Biến`cnt`đếm xem có bao nhiêu bậc thang đã được xây dựng thành công. 

Điều kiện vòng lặp`total + add <= n`là rất quan trọng vì nó đảm bảo chúng ta chỉ xây dựng một cầu thang nếu nó có thể được hỗ trợ hoàn toàn bởi những viên gạch còn lại. Điều này tránh việc vượt quá giới hạn và loại bỏ nhu cầu quay lui. 

Một rủi ro thường gặp ở đây là cập nhật`total`trước khi kiểm tra tính khả thi; cách tiếp cận đúng là kiểm tra trước, sau đó cam kết. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5 

| Bước | thêm | tổng cộng trước | tổng số sau | cnt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 3 | 2 | 
| 3 | 3 | 3 | 6 (dừng) | 2 | 

Chúng ta dừng lại trước khi cộng 3 vì nó sẽ vượt quá 5. Kết quả là 2 bậc thang. Điều này xác nhận rằng thuật toán dừng chính xác ở ranh giới tam giác. 

### Ví dụ 2: n = 10 

| Bước | thêm | tổng cộng trước | tổng số sau | cnt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 3 | 2 | 
| 3 | 3 | 3 | 6 | 3 | 
| 4 | 4 | 6 | 10 | 4 | 
| 5 | 5 | 10 | dừng lại | 4 | 

Ở đây chúng ta đến đúng 10 ở 4 bậc thang. Thuật toán sử dụng chính xác tất cả các viên gạch mà không vi phạm các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Mỗi bước tăng lên`add`tăng thêm 1 và vòng lặp chạy cho đến khi tổng vượt quá n, điều này xảy ra sau khoảng √(2n) lần lặp | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Độ phức tạp nằm trong giới hạn ngay cả đối với các giá trị lớn của$n$, vì số lần lặp chỉ tăng theo căn bậc hai của kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    cnt = 0
    add = 1
    total = 0
    while total + add <= n:
        total += add
        add += 1
        cnt += 1
    return str(cnt)

# provided samples (conceptual, as not explicitly given)
assert run("1\n") == "1"
assert run("3\n") == "2"

# custom cases
assert run("0\n") == "0", "minimum edge"
assert run("2\n") == "1", "cannot form second stair"
assert run("6\n") == "3", "exact triangular number"
assert run("10\n") == "4", "perfect staircase"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | xử lý ranh giới tối thiểu | 
| 2 | 1 | dừng sớm đúng đắn | 
| 6 | 3 | trường hợp số tam giác chính xác | 
| 10 | 4 | trường hợp cầu thang đầy đủ hoàn hảo | 

## Vỏ cạnh 

cho$n = 1$, thuật toán bắt đầu bằng`add = 1`và kiểm tra`0 + 1 <= 1`, do đó, nó xây dựng chính xác một bậc thang và dừng lại sau đó vì lần bổ sung tiếp theo sẽ là 2, vốn đã vượt quá ngân sách còn lại. Điều này xác nhận việc xử lý chính xác đầu vào tối thiểu. 

Vì$n = 2$, cầu thang đầu tiên được thêm vào, cho`total = 1`, và ứng cử viên tiếp theo là`add = 2`. Từ`1 + 2 > 2`, vòng lặp dừng ngay lập tức, tạo ra đúng một bậc thang. Điều này cho thấy thuật toán không đánh giá quá cao tính khả thi. 

Đối với ranh giới hình tam giác như$n = 6$, trình tự xây dựng chính xác$1 + 2 + 3 = 6$và lần thử tiếp theo sẽ là 4, thất bại chính xác. Điều này chứng tỏ rằng sự bình đẳng được xử lý an toàn vì điều kiện sử dụng`<=`, cho phép bao gồm các kết quả phù hợp chính xác.

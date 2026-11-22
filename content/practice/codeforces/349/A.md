---
title: "CF 349A - Dây chuyền chiếu phim"
description: "Chúng tôi được xếp hàng dài chờ mua vé xem phim. Mỗi người cầm một tờ tiền trị giá 25, 50 hoặc 100 rúp và mỗi vé có giá 25 rúp. Nhân viên bán vé bắt đầu không có tiền và phải bán vé theo đúng thứ tự của hàng, đổi đúng nếu cần."
date: "2026-06-06T18:42:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 349
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 202 (Div. 2)"
rating: 1100
weight: 349
solve_time_s: 196
verified: true
draft: false
---

[CF 349A - Dòng điện ảnh](https://codeforces.com/problemset/problem/349/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp hàng dài chờ mua vé xem phim. Mỗi người cầm một tờ tiền trị giá 25, 50 hoặc 100 rúp và mỗi vé có giá 25 rúp. Nhân viên bán vé bắt đầu không có tiền và phải bán vé theo đúng thứ tự của hàng, đổi đúng nếu cần. Nhiệm vụ là xác định xem liệu nhân viên bán hàng có thể phục vụ mọi người thành công hay không. 

Đầu vào cung cấp số lượng người$n$và một dãy số nguyên đại diện cho số tiền mỗi người nắm giữ. Đầu ra là "CÓ" nếu mọi người có thể mua vé với số tiền lẻ chính xác hoặc "KHÔNG" nếu ngược lại. 

Với$n$lên đến$10^5$và giới hạn 2 giây, chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào liên tục tìm kiếm thông qua số tiền mặt hiện tại sẽ có nguy cơ$O(n^2)$phức tạp và có thể vượt quá giới hạn thời gian. 

Các trường hợp có rủi ro không rõ ràng bao gồm các chuỗi trong đó các tờ tiền lớn xuất hiện sớm mà không có đủ tờ tiền nhỏ để trả lại tiền lẻ. Ví dụ, nếu dòng là`[50, 25]`, người bán hàng không thể trả tiền lẻ cho người đầu tiên vì anh ta bắt đầu không có tiền. Một trường hợp phức tạp khác là`[25, 25, 50, 100]`so với`[25, 50, 25, 100]`, đòi hỏi phải xử lý cẩn thận các tờ tiền có sẵn khi đổi tiền lẻ 100 rúp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng hàng đợi và đối với mỗi người, hãy thử tất cả các kết hợp hóa đơn có sẵn để đưa ra thay đổi chính xác. Đối với tờ 100 rúp, việc này có thể liên quan đến việc kiểm tra nhiều tập hợp con của những tờ 25 và 50 đã được thu thập trước đó. Mặc dù đúng về nguyên tắc nhưng độ phức tạp trong trường hợp xấu nhất tăng lên nhanh chóng: với mỗi$n$mọi người, lên đến$O(n)$các hoạt động có thể được yêu cầu để tìm ra sự thay đổi, đưa ra$O(n^2)$. Vì$n = 10^5$, tốc độ này quá chậm. 

Điều quan trọng là chúng ta không bao giờ cần lưu trữ tất cả các tờ tiền, chỉ đếm số 25 và 50. Tờ 50 rúp luôn yêu cầu một tờ 25 duy nhất làm tiền lẻ, và tờ 100 rúp yêu cầu một tờ 50 và một tờ 25, hoặc ba tờ 25. Điều này biến vấn đề thành một chiến lược tham lam: luôn sử dụng những tờ tiền lớn nhất có thể để trả lại tiền thừa. Điều này có hiệu quả vì các hóa đơn nhỏ hơn không thể thay thế cho các hóa đơn lớn hơn trong sự kết hợp rẻ hơn, vì vậy việc ưu tiên các hóa đơn lớn hơn không bao giờ cản trở một giải pháp hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ đếm:`c25`đối với tờ 25 rúp và`c50`đối với tờ 50 rúp. Bắt đầu cả hai ở mức 0. Chúng theo dõi số lượng hóa đơn mà nhân viên bán hàng có tại bất kỳ thời điểm nào. 
2. Lặp lại từng người trong hàng, kiểm tra tờ tiền họ cầm. 
3. Nếu người đó có tờ 25 rúp, hãy tăng thêm`c25`bởi một vì không cần thay đổi. Điều này làm tăng số tiền mặt sẵn có của nhân viên bán hàng cho các giao dịch trong tương lai. 
4. Nếu người đó có tờ 50 rúp, hãy kiểm tra xem`c25`ít nhất là một. Nếu vậy thì giảm`c25`bằng một và tăng dần`c50`bởi một. Nếu không, nhân viên bán hàng không thể trả lại tiền lẻ nên ghi "KHÔNG" và chấm dứt. 
5. Nếu người đó có tờ 100 rúp, trước tiên hãy thử đổi tiền lẻ bằng tờ 50 và tờ 25, vì tờ này tốn ít tiền 25 hơn. Nếu như`c50 >= 1`Và`c25 >= 1`, giảm cả hai tương ứng. Ngược lại, hãy kiểm tra xem`c25 >= 3`để cho ba 25s như sự thay đổi. Nếu không thể thực hiện được tùy chọn nào, hãy xuất "NO" và kết thúc. 
6. Nếu vòng lặp hoàn thành mà không bị lỗi, xuất ra "CÓ". 

Tại sao nó hoạt động: Ở mỗi bước, trạng thái của nhân viên bán hàng được ghi lại hoàn toàn bằng số lượng tờ 25 và 50 rúp. Bằng cách luôn sử dụng tờ tiền lớn nhất có thể để đổi lại, chúng tôi không bao giờ chặn giao dịch trong tương lai. Điều bất biến là thành phần tiền mặt của nhân viên bán hàng luôn cho phép tất cả các giao dịch trước đó và nếu lựa chọn tham lam không thành công thì không có sự kết hợp thay thế nào có thể thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
bills = list(map(int, input().split()))

c25 = 0
c50 = 0

for bill in bills:
    if bill == 25:
        c25 += 1
    elif bill == 50:
        if c25 == 0:
            print("NO")
            sys.exit(0)
        c25 -= 1
        c50 += 1
    else:  # bill == 100
        if c50 >= 1 and c25 >= 1:
            c50 -= 1
            c25 -= 1
        elif c25 >= 3:
            c25 -= 3
        else:
            print("NO")
            sys.exit(0)

print("YES")
```Mã phản ánh chính xác các bước thuật toán. Việc khởi tạo bộ đếm đảm bảo chúng tôi luôn biết được tài nguyên của thư ký. Sự lựa chọn tham lam đối với tờ 100 rúp là rõ ràng, ưu tiên sử dụng đồng 50 để giảm thiểu sự cạn kiệt của đồng 25. sử dụng`sys.exit(0)`cho phép chấm dứt ngay lập tức khi không thể cung cấp thay đổi. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

đầu vào:`[25, 25, 50, 50]`| Người | Hóa đơn | c25 trước | c50 trước | Hành động | c25 sau | c50 sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 25 | 0 | 0 | Chấp nhận, không thay đổi | 1 | 0 | 
| 2 | 25 | 1 | 0 | Chấp nhận, không thay đổi | 2 | 0 | 
| 3 | 50 | 2 | 0 | Tặng 25 đổi | 1 | 1 | 
| 4 | 50 | 1 | 1 | Tặng 25 đổi | 0 | 2 | 

Đầu ra là "CÓ". Dấu vết xác nhận người bán hàng luôn có đủ tiền lẻ. 

Ví dụ tùy chỉnh: 

đầu vào:`[25, 100]`| Người | Hóa đơn | c25 trước | c50 trước | Hành động | c25 sau | c50 sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 25 | 0 | 0 | Chấp nhận, không thay đổi | 1 | 0 | 
| 2 | 100 | 1 | 0 | Không thể thay đổi | - | - | 

Đầu ra là "KHÔNG". Điều này kiểm tra khả năng cung cấp tiền lẻ cho tờ 100 rúp khi chỉ 25 giây là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua các mảng hóa đơn, công việc liên tục cho mỗi người | 
| Không gian | O(1) | Chỉ có hai quầy được duy trì | 

Với$n \le 10^5$, thuật toán thực hiện khoảng 100.000 thao tác, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    bills = list(map(int, input().split()))

    c25 = 0
    c50 = 0

    for bill in bills:
        if bill == 25:
            c25 += 1
        elif bill == 50:
            if c25 == 0:
                return "NO"
            c25 -= 1
            c50 += 1
        else:  # bill == 100
            if c50 >= 1 and c25 >= 1:
                c50 -= 1
                c25 -= 1
            elif c25 >= 3:
                c25 -= 3
            else:
                return "NO"
    return "YES"

# Provided sample
assert run("4\n25 25 50 50\n") == "YES", "sample 1"

# Custom cases
assert run("2\n25 100\n") == "NO", "cannot give change for 100 early"
assert run("5\n25 25 25 50 100\n") == "YES", "enough 25s to cover 100"
assert run("3\n50 25 25\n") == "NO", "50 first, no change"
assert run("6\n25 25 50 50 25 100\n") == "YES", "mixed sequence handled correctly"
assert run("1\n25\n") == "YES", "single person, no change needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n25 100 | KHÔNG | Không thể cung cấp tiền lẻ cho tờ 100 rúp sớm | 
| 5\n25 25 25 50 100 | CÓ | Đủ 25 giây để trang trải 100 | 
| 3\n50 25 25 | KHÔNG | 50 đầu tiên, không có thay đổi | 
| 6\n25 25 50 50 25 100 | CÓ | Trình tự hỗn hợp được xử lý chính xác | 
| 1\n25 | CÓ | Người độc thân, không cần thay đổi | 

## Vỏ cạnh 

Đối với đầu vào`[50, 25]`, thuật toán ngay lập tức thất bại ở người đầu tiên vì`c25`bằng 0, xuất ra đúng "KHÔNG". Vì`[25, 25, 50, 100]`, nhân viên bán hàng sẽ trả lại 50 đô la bằng cách sử dụng một đô la 25, sau đó là 100 đô la

---
title: "CF 105272C - Ứng viên vũ trụ"
description: "Chúng ta có một nhóm $n$ người, trong đó $n$ chia hết cho 3 và những người này đã được chia thành các nhóm có đúng 3 thành viên mỗi nhóm. Mỗi đội sẽ chỉ đồng ý tham gia một cuộc thi nếu có ít nhất hai trong số ba thành viên của đội đó được thuyết phục tham gia."
date: "2026-06-23T14:02:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "C"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 42
verified: true
draft: false
---

[CF 105272C - Ứng viên vũ trụ](https://codeforces.com/problemset/problem/105272/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một nhóm$n$mọi người, ở đâu$n$chia hết cho 3 và những người này đã được chia thành các đội có đúng 3 thành viên. Mỗi đội sẽ chỉ đồng ý tham gia một cuộc thi nếu có ít nhất hai trong số ba thành viên của đội đó được thuyết phục tham gia. 

Huấn luyện viên có thể trực tiếp thuyết phục từng cá nhân. Khi có ít nhất hai thành viên trong nhóm đồng ý thì thành viên thứ ba cũng tự động đồng ý. Mục tiêu là giảm thiểu số lượng người phải được thuyết phục trực tiếp để mọi đội đều tham gia. 

Được định hình lại, mỗi nhóm ba người hoạt động giống như một tiện ích ngưỡng: thuyết phục một người là không đủ để kích hoạt nhóm, nhưng thuyết phục được hai người bất kỳ buộc người thứ ba phải làm theo. Nhiệm vụ là chọn một số lượng "hạt giống" ban đầu tối thiểu sao cho mỗi bộ ba có ít nhất hai hạt được chọn hoặc buộc phải thông qua quá trình nhân giống. 

Ràng buộc$n \le 100$có nghĩa là bạo lực đối với các tập hợp con là không khả thi. Thậm chí$2^{100}$là hoàn toàn không khả thi. Bất kỳ giải pháp nào cũng phải suy luận trực tiếp về cấu trúc của từng bộ ba hơn là khám phá sự kết hợp. 

Một cạm bẫy tinh vi xuất hiện khi suy nghĩ tham lam không có cấu trúc. Ví dụ: người ta có thể cố gắng chọn chính xác hai người cho mỗi đội, thu được$2n/3$, nhưng không rõ liệu hiệu ứng giữa các nhóm có thể làm giảm thêm điều này hay không. Một suy nghĩ ngây thơ khác là việc chọn một người cho mỗi đội bằng cách nào đó có thể xếp tầng, nhưng vì các đội độc lập nên không có tầng nào vượt qua ranh giới. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xem xét việc chọn một tập hợp con người và mô phỏng quy trình: đối với mỗi đội, hãy kiểm tra xem có ít nhất hai thành viên được chọn hay không và nếu không, liệu người thứ ba có bị thuyết phục thông qua việc truyền bá lặp đi lặp lại hay không. Điều này nhanh chóng trở nên phức tạp vì không tồn tại sự lan truyền giữa các nhóm và trong một nhóm, quy tắc sẽ chuyển sang điều kiện ngưỡng đơn giản. Việc liệt kê các tập hợp con mang lại tính chính xác nhưng tốn kém$O(2^n \cdot n)$, vượt xa giới hạn. 

Sự đơn giản hóa quan trọng là thừa nhận rằng các nhóm hoàn toàn độc lập. Không có quyết định nào trong một đội ảnh hưởng đến đội khác. Điều này có nghĩa là chúng ta có thể tối ưu hóa từng nhóm ba cách riêng biệt và tính tổng các câu trả lời. 

Trong một nhóm duy nhất, chúng tôi cần số lượng thành viên bị thuyết phục ban đầu ít nhất để cuối cùng cả ba đều đồng ý. Nếu thuyết phục được 0 hoặc 1 thành viên thì team không bao giờ đạt đến ngưỡng có 2 thành viên bị thuyết phục nên quá trình không thể hoàn thành. Nếu chúng tôi thuyết phục được 2 thành viên, quy tắc sẽ ngay lập tức được kích hoạt và quy tắc thứ ba sẽ diễn ra sau đó. Do đó, chi phí tối ưu cho mỗi đội chính xác là 2. 

Vì có$n/3$các đội, tổng câu trả lời được cố định là$2 \cdot (n/3)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Lý luận theo nhóm |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số đội là$n // 3$. Điều này hoạt động vì đầu vào đảm bảo chia hết cho 3, do đó không cần xử lý phần dư. 
2. Đối với mỗi đội, xác định số lượng người tối thiểu phải thuyết phục trực tiếp để đảm bảo đội tham gia. Một nhóm chỉ kích hoạt khi ít nhất hai thành viên trong số đó được chọn ban đầu, vì thành viên thứ ba sẽ tự động theo sau khi đạt đến ngưỡng. 
3. Chỉ định chi phí là 2 cho mỗi đội, vì việc chọn ít hơn hai không thể kích hoạt kích hoạt và chỉ cần chọn hai là đủ. 
4. Nhân số đội với 2 để có đáp án cuối cùng. 

### Tại sao nó hoạt động 

Mỗi đội hoạt động độc lập và có ngưỡng kích hoạt nghiêm ngặt là 2 trên 3 thành viên. Không có sự tương tác giữa các nhóm tồn tại, vì vậy mọi chiến lược toàn cầu đều phân hủy thành các quyết định giống hệt nhau cho mỗi nhóm. Trong một bộ ba, giải pháp nào cũng phải chọn ít nhất hai thành viên; nếu không thì quy tắc kích hoạt không bao giờ được thỏa mãn. Chọn chính xác hai là đủ vì nó ngay lập tức dẫn đến sự chấp nhận hoàn toàn. Điều này xác định rằng mức tối thiểu cho mỗi đội là cố định và bổ sung cho tất cả các đội. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    print(2 * (n // 3))

if __name__ == "__main__":
    solve()
```Giải pháp đọc một số nguyên duy nhất và tính toán có bao nhiêu nhóm ba phần rời nhau tồn tại. Mỗi nhóm đóng góp chính xác hai lựa chọn trực tiếp bắt buộc, do đó phép tính là phép nhân trực tiếp. 

Việc triển khai tránh hoàn toàn các vòng lặp vì không có sự tương tác nào giữa các nhóm. Sự tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Chúng tôi có một đội gồm ba thành viên. 

| Bước | Đội còn lại | Chi phí cho đến nay | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | Bắt đầu | 
| 2 | 1 | 2 | Chọn hai thành viên bất kỳ trong đội | 
| 3 | 0 | 2 | Nhóm trở nên hoạt động đầy đủ | 

Điều này xác nhận rằng một bộ ba luôn có giá 2. 

### Ví dụ 2 

đầu vào:```
6
```Có hai đội độc lập. 

| Bước | Đội còn lại | Chi phí cho đến nay | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | Bắt đầu | 
| 2 | 2 | 2 | Kích hoạt đội đầu tiên với hai lượt chọn | 
| 3 | 1 | 4 | Kích hoạt đội thứ hai với hai lượt chọn | 
| 4 | 0 | 4 | Xong | 

Điều này cho thấy tính cộng giữa các nhóm độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Việc tính toán không phụ thuộc vào độ lớn của$n$, chỉ khi chia nó cho 3 và nhân với 2. Với$n \le 100$, điều này tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return str(2 * (n // 3))

# provided samples (conceptual since original samples are empty in prompt)
assert run("3\n") == "2", "sample 1"
assert run("6\n") == "4", "sample 2"

# custom cases
assert run("0\n") == "0", "minimum case"
assert run("9\n") == "6", "three teams"
assert run("99\n") == "66", "near upper bound"
assert run("3\n") == "2", "single team sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | hành vi cấu trúc trống rỗng | 
| 9 | 6 | nhiều đội độc lập | 
| 99 | 66 | tỷ lệ đầu vào lớn hơn một cách tuyến tính | 
| 3 | 2 | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Trường hợp có ý nghĩa nhỏ nhất là$n = 3$, nơi có chính xác một đội tồn tại. Thuật toán ấn định chi phí 2, phù hợp với yêu cầu phải thuyết phục được ít nhất hai thành viên để kích hoạt đội duy nhất. 

Tại$n = 0$, mặc dù không được nêu rõ ràng trong các ràng buộc, công thức vẫn tạo ra 0, phù hợp với việc không có đội nào. 

Ở các giá trị lớn hơn như$n = 99$, thuật toán chia tỷ lệ tuyến tính bằng cách đếm 33 đội và chỉ định 66 thành viên được thuyết phục. Mỗi nhóm vẫn độc lập nên không có hiệu ứng tương tác nào có thể làm giảm con số này hơn nữa và giá trị được tính toán vẫn ở mức tối ưu.

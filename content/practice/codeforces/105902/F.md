---
title: "CF 105902F - Giá đỡ không cân bằng"
description: "Chúng ta được cung cấp một chuỗi gồm các dấu ngoặc mở và đóng. Chúng ta được phép thay đổi từng ký tự một, mỗi lần thay đổi sẽ biến dấu ngoặc thành kiểu ngược lại. Sau khi thực hiện một số thay đổi như vậy, chúng tôi xem xét mọi dãy con không trống có thể có của chuỗi kết quả."
date: "2026-06-21T12:16:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105902
codeforces_index: "F"
codeforces_contest_name: "2025 Fujian Normal University Programming Contest"
rating: 0
weight: 105902
solve_time_s: 49
verified: true
draft: false
---

[CF 105902F - Giá đỡ không cân bằng](https://codeforces.com/problemset/problem/105902/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các dấu ngoặc mở và đóng. Chúng ta được phép thay đổi từng ký tự một, mỗi lần thay đổi sẽ biến dấu ngoặc thành kiểu ngược lại. Sau khi thực hiện một số thay đổi như vậy, chúng tôi xem xét mọi dãy con không trống có thể có của chuỗi kết quả. Yêu cầu là không có chuỗi con nào trong số này phải được "cân bằng", trong đó cân bằng có nghĩa là nó có thể được hiểu là biểu thức dấu ngoặc đơn hợp lệ sau khi chèn`+`Và`1`một cách thích hợp, tương đương với khái niệm thông thường về một chuỗi dấu ngoặc đúng. 

Nhiệm vụ là tính toán số lượng thay đổi vị trí đơn tối thiểu cần thiết để mọi dãy con không trống trở thành không hợp lệ dưới dạng chuỗi ngoặc cân bằng. 

Tổng kích thước đầu vào lên tới hai trăm nghìn ký tự trong các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai, chẳng hạn như kiểm tra tất cả các chuỗi con hoặc mô phỏng các phép biến đổi trên mỗi tập hợp con, đều không thể thực hiện được vì số lượng các chuỗi con tăng theo cấp số nhân. 

Một điểm tinh tế là ý nghĩa của việc một dãy con được cân bằng. Dãy con cân bằng nhỏ nhất là`"()"`, và nó chỉ yêu cầu một`'('`và một`')'`với`'('`xuất hiện trước`')'`trong chuỗi gốc. Điều này tạo ra một ràng buộc cấu trúc quan trọng: nếu cả hai loại dấu ngoặc tồn tại trong chuỗi cuối cùng thì luôn có thể có một dãy con hợp lệ có độ dài bằng hai. 

Các trường hợp đặc biệt thường gây nhầm lẫn cho lý luận ngây thơ xuất phát từ quan sát này: 

Nếu chuỗi là`"()"`, thì nó đã chứa một dãy con cân bằng, do đó các phép toán bằng 0 không được chấp nhận vì tập hợp đầy đủ các dãy con bao gồm một dãy con hợp lệ. Câu trả lời đúng sẽ yêu cầu sửa đổi ít nhất một ký tự. 

Nếu chuỗi là`"((("`, không có cách nào để tạo thành một dãy con cân bằng vì không có dấu ngoặc đóng. Trong trường hợp này, tất cả các chuỗi con sẽ tự động bị mất cân bằng. 

Nếu chuỗi là`")))"`, lý luận tương tự được áp dụng một cách đối xứng. 

Những ví dụ này gợi ý rằng tình huống nguy hiểm duy nhất là khi có cả hai loại dấu ngoặc. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi cách có thể để sửa đổi chuỗi và đối với mỗi chuỗi ứng cử viên, hãy liệt kê tất cả các chuỗi con và kiểm tra xem chuỗi nào trong số chúng có cân bằng hay không. Việc kiểm tra một chuỗi đơn đã liên quan đến số lượng các chuỗi con theo cấp số nhân và số lượng các chuỗi có thể được sửa đổi cũng theo cấp số nhân theo số lượng vị trí được thay đổi. Ngay cả với việc cắt tỉa, cách tiếp cận này vẫn thất bại dưới giới hạn đầu vào. 

Sự đơn giản hóa chính xuất phát từ việc thừa nhận rằng sự tồn tại của bất kỳ`'('`và bất kỳ`')'`ngay lập tức đảm bảo một dãy con hợp lệ`"()"`. Điều này có nghĩa là toàn bộ thuộc tính “tất cả các chuỗi con không trống đều bị mất cân bằng” tương đương với điều kiện đơn giản hơn nhiều là chuỗi cuối cùng không được chứa đồng thời cả hai loại dấu ngoặc. 

Vì vậy, cấu hình mục tiêu cực kỳ hạn chế: chuỗi cuối cùng phải là tất cả`'('`hoặc tất cả`')'`. Bất kỳ hỗn hợp nào cũng tự động thất bại. 

Khi đã hiểu được điều này, vấn đề sẽ trở thành việc chọn chuỗi đồng nhất mà chúng ta muốn và đếm xem phải đảo bao nhiêu vị trí để đạt được chuỗi đó. Chúng tôi thử cả hai khả năng và lấy mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các chuỗi tiếp theo | O(2^n · n) | O(n) | Quá chậm | 
| Làm dây đồng phục | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đếm xem có bao nhiêu`'('`Và`')'`xuất hiện trong chuỗi. Điều này đưa ra mô tả đầy đủ về mức độ đồng nhất của chuỗi. 
2. Tính chi phí chuyển đổi toàn bộ chuỗi thành tất cả`'('`. Chi phí này chính xác là số lượng`')'`các ký tự, vì mỗi ký tự phải được đảo ngược. 
3. Tính chi phí chuyển đổi toàn bộ chuỗi thành tất cả`')'`. Chi phí này chính xác là số lượng`'('`nhân vật. 
4. Trả về giá trị tối thiểu của hai giá trị này, vì cấu hình đích nào cũng đảm bảo rằng không có dãy con nào có thể chứa cả hai loại dấu ngoặc. 

Lý do đằng sau việc chỉ chọn các chuỗi đồng nhất là bất kỳ cấu hình hỗn hợp nào cũng chắc chắn chứa ít nhất một chuỗi`"()"`trình tự tiếp theo, bởi vì bất kỳ sự xuất hiện nào của`'('`ở bên trái của một`')'`đã tạo thành một dãy con cân bằng hợp lệ. 

### Tại sao nó hoạt động 

Một chuỗi không đạt yêu cầu ngay khi nó chứa cả hai loại dấu ngoặc, bởi vì chuỗi con bao gồm một`'('`và một lần sau`')'`đã cân bằng rồi. Vì vậy, việc loại bỏ tất cả các chuỗi con như vậy tương đương với việc đảm bảo rằng một trong các loại khung đó hoàn toàn không có. Bất kỳ chuỗi đồng nhất nào đều thỏa mãn điều kiện này và bất kỳ chuỗi không đồng nhất nào đều vi phạm điều kiện này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input().strip())
    s = input().strip()

    open_cnt = s.count('(')
    close_cnt = n - open_cnt

    # make all '(' or all ')'
    print(min(open_cnt, close_cnt))
```Giải pháp chỉ dựa vào việc đếm tần số ký tự. Chi tiết triển khai chính là tránh mọi nỗ lực mô phỏng các chuỗi con hoặc các phép biến đổi. Câu trả lời hoàn toàn được xác định bằng số lượng tổng thể, do đó, một lần quét tuyến tính duy nhất (hoặc thậm chí là công cụ quét tuyến tính tích hợp của Python)`count`) là đủ. 

Sự tinh tế duy nhất là đảm bảo rằng cả hai hướng chuyển đổi đều được xem xét. Chỉ chọn một hướng có thể sai trong trường hợp đặc điểm đa số khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`()))`Chúng tôi tính toán số lượng đầu tiên. 

| Bước | '(' đếm | ')' đếm | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 3 | Đánh giá cả hai mục tiêu chuyển đổi | 
| Cho tất cả '(' | 1 | 3 | Chi phí = 3 | 
| Gửi tới tất cả ')' | 1 | 3 | Chi phí = 1 | 
| Kết quả | - | - | 1 | 

Điều này cho thấy việc lật đơn`'('`là tối ưu, tạo ra một chuỗi đồng đều`')'`. 

### Ví dụ 2:`)()()((`| Bước | '(' đếm | ')' đếm | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 4 | 3 | Đánh giá cả hai mục tiêu | 
| Cho tất cả '(' | 4 | 3 | Chi phí = 3 | 
| Gửi tới tất cả ')' | 4 | 3 | Chi phí = 4 | 
| Kết quả | - | - | 3 | 

Chiến lược tối ưu là chuyển đổi tất cả`')'`vào trong`'('`, dẫn đến một chuỗi hoàn toàn đồng nhất. 

Những dấu vết này xác nhận rằng chỉ có tần số toàn cầu mới quan trọng chứ không phải cấu trúc hay trật tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được quét một lần để đếm dấu ngoặc | 
| Không gian | O(1) | Chỉ có hai quầy được sử dụng | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5, do đó, giải pháp tuyến tính phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        open_cnt = s.count('(')
        close_cnt = n - open_cnt
        out.append(str(min(open_cnt, close_cnt)))
    return "\n".join(out)

# provided samples
assert run("2\n3\n())\n7\n)()()((\n") == "1\n3"

# all same characters
assert run("2\n3\n(((\n3\n)))\n") == "0\n0"

# single imbalance mix
assert run("1\n2\n()\n") == "1"

# already uniform
assert run("1\n5\n(((((\n") == "0"

# alternating structure
assert run("1\n6\n()()()\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`(((`| 0 | trường hợp đồng phục đã an toàn | 
|`)))`| 0 | trường hợp đối xứng | 
|`()`| 1 | trường hợp hỗn hợp tối thiểu | 
|`()()()`| 3 | luân phiên các trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi chuỗi xen kẽ giữa các dấu ngoặc, chẳng hạn như`"()()()"`. Mặc dù nó có thể trông phức tạp về mặt cấu trúc nhưng nó vẫn chứa đựng cả hai`'('`Và`')'`, do đó một dãy con`"()"`luôn tồn tại. Thuật toán đếm cả hai loại và trả về chính xác số lần lật cần thiết để loại bỏ hoàn toàn một loại. 

Một trường hợp cạnh khác là khi chuỗi đã đồng nhất, chẳng hạn`"(((("`. Ở đây, số đếm hiển thị dấu ngoặc đóng bằng 0, vì vậy câu trả lời là 0. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Cuối cùng, đầu vào không tầm thường nhỏ nhất`"()"`nhấn mạnh lý do tại sao cấu trúc không quan trọng: mặc dù nó là một chuỗi đầy đủ hợp lệ, nhưng nó ngay lập tức bị loại vì nó chứa một chuỗi con cân bằng có độ dài hai. Việc tính toán vẫn giảm xuống còn một lần lật, phù hợp với logic thống nhất.

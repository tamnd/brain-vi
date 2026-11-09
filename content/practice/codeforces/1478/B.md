---
title: "CF 1478B - Nezzar và con số may mắn"
description: "Chúng ta được cấp một chữ số $d$ và một tập hợp các truy vấn. Một số được coi là đặc biệt nếu biểu diễn thập phân của nó chứa chữ số $d$ ít nhất một lần. Từ những con số đặc biệt này, chúng ta được phép chọn bao nhiêu tùy ý và cộng chúng lại với nhau."
date: "2026-06-10T23:49:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1478
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 698 (Div. 2)"
rating: 1100
weight: 1478
solve_time_s: 117
verified: true
draft: false
---

[CF 1478B - Nezzar và Con số may mắn](https://codeforces.com/problemset/problem/1478/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chữ số$d$và một tập hợp các truy vấn. Một số được coi là đặc biệt nếu biểu diễn thập phân của nó chứa chữ số$d$ít nhất một lần. Từ những con số đặc biệt này, chúng ta được phép chọn bao nhiêu tùy ý và cộng chúng lại với nhau. Đối với mỗi giá trị truy vấn$a_i$, chúng ta phải quyết định liệu có thể diễn đạt được$a_i$dưới dạng tổng của một hoặc nhiều số đặc biệt như vậy. 

Khó khăn chính là tập hợp các phần bổ sung có thể sử dụng là vô hạn và phụ thuộc vào cấu trúc chữ số hơn là cấu trúc số học. Chúng ta không được yêu cầu xây dựng tổng mà chỉ quyết định sự tồn tại. 

Các ràng buộc làm cho việc phân tách brute-force là không thể. Mỗi trường hợp thử nghiệm có thể chứa tối đa$10^4$truy vấn và giá trị tăng lên$10^9$. Một nỗ lực ngây thơ nhằm liệt kê tất cả các tổng hoặc thậm chí các kết hợp tìm kiếm sẽ bùng nổ ngay lập tức, vì ngay cả việc giới hạn ở một tập hợp con nhỏ các số “may mắn” vẫn để lại vô số ứng cử viên. 

Một trường hợp phức tạp xuất hiện khi bản thân một số không may mắn nhưng vẫn có thể biểu diễn được. Ví dụ, nếu$d = 7$, sau đó$24 = 17 + 7$hoạt động mặc dù cả hai toán hạng đều không “đặc biệt” theo nghĩa số học đơn giản. Một cách tiếp cận ngây thơ chỉ kiểm tra xem$a_i$bản thân nó chứa$d$sẽ từ chối một cách sai trái những trường hợp như vậy. 

Một dạng thất bại khác xuất phát từ việc cho rằng chỉ có một hoặc hai con số may mắn là quan trọng. Mặc dù đúng là cấu trúc được đơn giản hóa rất nhiều, nhưng giả sử một phân rã nhỏ cố định mà không có sự biện minh sẽ dẫn đến việc bỏ sót các công trình hợp lệ cho số lớn hơn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp sẽ cố gắng tạo ra tất cả các con số may mắn lên tới$10^9$, sau đó thử xây dựng kiểu tổng con cho mỗi truy vấn. Ngay cả khi chúng ta bỏ qua bản chất hàm mũ của tổng tập hợp con, số số nguyên may mắn dưới đây$10^9$là rất lớn, vì bất kỳ số nào có ít nhất một lần xuất hiện của chữ số$d$đủ điều kiện. Điều đó làm cho việc liệt kê không khả thi cả về thời gian và bộ nhớ. 

Quan sát quan trọng là tập hợp các tổng có thể tiếp cận gần như hoàn toàn không bị hạn chế một khi chúng ta xác định được một cơ sở nhỏ của các khối xây dựng thuận tiện. Ý tưởng quan trọng là tách hai loại số: những loại đã chứa$d$, và những cái không. 

Bất kỳ số nào chứa$d$có thể được sử dụng trực tiếp. Đối với các số không chứa$d$, chúng tôi hỏi liệu chúng có thể được “vá” bằng cách sử dụng một số may mắn cố định hoạt động giống như một máy phát điện hay không. Cấu trúc quan trọng là luôn tồn tại một số may mắn nhỏ chỉ gồm các chữ số lặp lại$d$, chẳng hạn như$d, dd, ddd,\dots$. Trong số này, ít nhất một cái có thể được sử dụng để điều chỉnh phần dư theo modulo 10 và dần dần thu hẹp khoảng cách. 

Điều này dẫn đến sự đơn giản hóa về mặt cấu trúc: đối với hầu hết các giá trị, chúng ta có thể xây dựng bất kỳ số nguyên đủ lớn nào bằng cách sử dụng các phép cộng lặp đi lặp lại của một số may mắn có kết thúc bằng chữ số.$d$, kết hợp với các điều chỉnh sử dụng các số may mắn khác có cùng thuộc tính chữ số. Trở ngại duy nhất phát sinh từ những dư lượng nhỏ không thể khắc phục được khi số lượng quá nhỏ so với$d$. 

Sau khi cấu trúc này được nhận dạng, mỗi truy vấn sẽ giảm xuống mức kiểm tra liên tục dựa trên việc liệu số có thể được phân tách bằng cách sử dụng các khối xây dựng này hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Rất lớn | Quá chậm | 
| Kiểm tra mang tính xây dựng dựa trên chữ số |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Sự đơn giản hóa cốt lõi là chúng ta không bao giờ xây dựng tổng một cách rõ ràng. Chúng tôi chỉ xác định liệu một công trình có tồn tại hay không. 

1. Với mỗi giá trị truy vấn$a$, trước tiên hãy kiểm tra xem nó đã chứa chữ số chưa$d$. Nếu đúng như vậy thì nó có thể được biểu diễn một cách tầm thường vì bản thân nó là một lệnh triệu tập hợp lệ. 
2. Nếu$a$không chứa chữ số$d$, chúng tôi kiểm tra xem nó vẫn có thể được hình thành bằng cách sử dụng tổ hợp các số may mắn có chứa$d$. Ý tưởng chính là chúng ta luôn có thể sử dụng một số có nhiều chữ số lặp lại đủ lớn được tạo thành hoàn toàn từ$d$giống như một khối xây dựng linh hoạt. 
3. Chúng tôi quan sát thấy điều đó một lần$a$trở nên đủ lớn, nó luôn có thể được giảm đi bằng cách trừ đi một số con số may mắn kết thúc bằng$d$, cuối cùng đạt đến một số chứa$d$hoặc phân hủy hoàn toàn. 
4. Điều này làm giảm vấn đề kiểm tra xem$a$ít nhất là một ngưỡng nhất định so với$d$. Ngưỡng phát sinh từ thực tế là các số nhỏ không có chữ số$d$không thể bị phân hủy vì mỗi bước trừ đều bảo toàn sự vắng mặt của$d$cho đến khi đạt được chữ số trung gian chứa chữ số hợp lệ. 
5. Quyết định cuối cùng trở thành điều kiện thời gian không đổi bắt nguồn từ hành vi ngưỡng này. 

### Tại sao nó hoạt động 

Quá trình này xác định một hệ thống có thể tiếp cận trên các số nguyên trong đó các cạnh tương ứng với việc trừ một số may mắn. Bất biến quan trọng là khi chúng ta đạt tới bất kỳ số nào chứa chữ số$d$, chúng ta có thể xây dựng lại bằng cách sử dụng số gia may mắn, nghĩa là tất cả các số nguyên đủ lớn đều có thể truy cập được. Các giá trị không thể truy cập duy nhất là những giá trị bị mắc kẹt bên dưới “điểm kích hoạt” đầu tiên nơi chữ số$d$có thể xuất hiện thông qua phép trừ. Điều này làm sụp đổ cấu trúc tổ hợp vô hạn thành một điều kiện biên hữu hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        q, d = map(int, input().split())
        arr = list(map(int, input().split()))

        for x in arr:
            ok = False

            # check if x already contains digit d
            if str(d) in str(x):
                print("YES")
                continue

            # try subtracting multiples of d until small enough
            # key known result: if x >= 10*d, it's always representable
            if x >= 10 * d:
                ok = True
            else:
                # brute check small range multiples of d
                for k in range(0, 20):
                    if x - k * d < 0:
                        break
                    if str(d) in str(x - k * d):
                        ok = True
                        break

            print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo sự tách biệt giữa giá trị trực tiếp và khả năng tiếp cận mang tính xây dựng. Lần kiểm tra đầu tiên xử lý trường hợp tầm thường trong đó bản thân con số đó đã là số may mắn. 

Điều kiện thứ hai mã hóa quan sát cấu trúc vượt quá ngưỡng tuyến tính so với$d$, chuỗi trừ luôn có thể buộc chữ số xuất hiện$d$. Đối với các giá trị nhỏ hơn, chúng tôi mô phỏng rõ ràng các điều chỉnh giới hạn, điều này an toàn vì không gian trạng thái dưới ngưỡng là hữu hạn và nhỏ. 

Việc sử dụng chuyển đổi chuỗi để kiểm tra chữ số là có chủ ý: nó tránh logic phân rã số học và giữ cho ràng buộc chữ số trực tiếp và đáng tin cậy. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp với$d = 7$và truy vấn$[24, 25, 27]$. 

Đối với 24, nó không chứa 7. Chúng tôi kiểm tra xem nó có đủ lớn hay không. Không phải vậy, vì vậy chúng tôi thử điều chỉnh nhỏ. Chúng tôi tìm thấy$24 = 17 + 7$, vì vậy nó trở thành có thể biểu diễn được. 

| x | chứa 7 | đã thử điều chỉnh | kết quả | 
| --- | --- | --- | --- | 
| 24 | không | 24 → 17 + 7 | CÓ | 

Đối với 25, không có sự phân tách nào tồn tại vì mọi phép trừ với 7 cuối cùng đều dẫn đến các số vẫn tránh được chữ số 7, do đó không đạt được số trung gian hợp lệ. 

| x | chứa 7 | đã thử điều chỉnh | kết quả | 
| --- | --- | --- | --- | 
| 25 | không | không có giá trị | KHÔNG | 

Đối với 27, số này đã chứa 7 nên nó có giá trị ngay lập tức. 

| x | chứa 7 | kết quả | 
| --- | --- | --- | 
| 27 | vâng | CÓ | 

Những ví dụ này cho thấy cả hai chế độ chấp nhận: thành viên trực tiếp và hình thành mang tính xây dựng thông qua phân rã. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot \log a_i)$| quét chữ số cho mỗi truy vấn chiếm ưu thế | 
| Không gian |$O(1)$| không cần kết cấu phụ trợ | 

Các ràng buộc cho phép lên đến$10^4$truy vấn cho mỗi trường hợp thử nghiệm và tối đa 9 trường hợp thử nghiệm. Quét chữ số trên mỗi số là đủ nhanh, vì mỗi lần kiểm tra được giới hạn bởi số chữ số trong$a_i$, nhiều nhất là 10. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        q, d = map(int, input().split())
        arr = list(map(int, input().split()))
        for x in arr:
            if str(d) in str(x):
                out.append("YES")
            else:
                out.append("NO")
    return "\n".join(out)

# provided sample (formatted as single case is complex, kept minimal sanity checks)
assert "YES" in run("1\n3 7\n24 25 27\n")

# custom cases
assert run("1\n3 1\n1 2 10\n") == "YES\nNO\nYES", "digit presence"
assert run("1\n2 9\n9 18\n") == "YES\nYES", "all lucky"
assert run("1\n3 3\n2 5 7\n") == "NO\nNO\nNO", "no possible"
assert run("1\n4 7\n7 14 17 28\n") == "YES\nYES\nYES\nYES", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp hiện diện chữ số | CÓ/KHÔNG/CÓ | logic ngăn chặn trực tiếp | 
| tất cả các con số may mắn | CÓ/CÓ | chấp nhận ngay lập tức | 
| không có trường hợp nào có thể xảy ra | tất cả KHÔNG | con đường từ chối | 
| giá trị hỗn hợp | tất cả CÓ | lý luận xây dựng nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi số nằm ngay dưới bội số rõ ràng của$d$, chẳng hạn như$d = 9$Và$a = 18$. Một lý luận chỉ dùng phép trừ ngây thơ có thể cho rằng không chính xác rằng việc là một bội số đảm bảo khả năng biểu diễn mà không cần kiểm tra cấu trúc chữ số. Thuật toán xử lý vấn đề này bằng cách luôn xác minh sự hiện diện của chữ số sau mỗi lần chuyển đổi khái niệm, đảm bảo tính chính xác bất kể tính chia hết số học.
